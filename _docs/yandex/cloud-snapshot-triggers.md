---
title: "Создание / удалением snapshot c использованием serverless"
permalink: /docs/yandex/cloud-snapshot-triggers/
toc: true
---

## Цель

Создавать и удалять моментальные по таймеру и с разделение на критичные данных и по умолчанию.

## Описание

Критически важные снимки создаются каждый день в 18:00, срок жизни 7 дней.
Все снимки один раз в неделю в воскресение в 08:00, срок жизни 14 день.
Удаление снимков выполняется ежедневно в 23:59.

Графическая схема [ссылка](https://viewer.diagrams.net/?highlight=0000ff&edit=https%3A%2F%2Fapp.diagrams.net%2F%23G1MS-fS_s-agVfnpWCHP8Fk6SK-ALd0doj&layers=1&nav=1&title=%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BC%D0%BE%D0%BC%D0%B5%D0%BD%D1%82%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BA%D0%BE%D0%B2.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1MS-fS_s-agVfnpWCHP8Fk6SK-ALd0doj%26export%3Ddownload)

За основу взята публикация https://cloud.yandex.ru/blog/posts/2020/01/snapshot-triggers.
После прочтения выявились нюансы:
- наименование моментального диска формируется из id, что не позволяет различить их
- нет инструкции по удалению снимков

Ниже приводится пример где
- имя моментального снимка будет в формате: `<Название диск>-ГГГГ-ММ-ДД`
- моментальные снимки будут иметь атрибут срок жизни
- триггер по удалению снимков с истекшим сроком жизни

## Пометим диски для создания снимков

Возможно потребуется инициализация cli,
[начало работы с CLI](https://cloud.yandex.ru/docs/cli/quickstart)

Получим список дисков
```
yc compute disk list
```
чтобы получить с метками воспользуемся ConvertFrom-Json
```
(yc compute disk list --format json | ConvertFrom-Json) | Select id,name,status,labels
```

Дискам в зависимости от цели установим метки (labels)
- snapshot-default - все диски для которых выполняются снимки
- snapshot-critical - только те диски для которых нужно выполнять снимки чаще

```
yc compute disk update --id <id диска> --labels snapshot-default=14 --labels snapshot-critical=7
yc compute disk update --id <id диска> --labels snapshot-default=14
```

Если потребуется удалить labels
```
yc compute disk remove-labels --id <id диска> --labels snapshot-default
yc compute disk remove-labels --id <id диска> --labels snapshot-critical
```

## Создадим функции

Описание метода create https://cloud.yandex.ru/docs/compute/api-ref/Snapshot/create

### Создание моментальных снимков для критичных данных с жизненным циклом 7 дней

Будет запускается каждый день

**create-snapshot-critical**

```
// Префикс имени диска ГГГГ-ММ-ДД
let today = new Date();
var yyyymmdd = today.toISOString().substring(0, 10);
var hh = today.getHours().toString();
var mm = today.getMinutes().toString();
var ss = today.getSeconds().toString();
var yyyymmddhhmmss = yyyymmdd + "-" + hh + mm + ss;

// Метка диска жизненный цикл
let label = {
   "lifecycle": "7"
};

const ycsdk = require("yandex-cloud/api/compute/v1");
const FOLDER_ID = process.env.FOLDER_ID;
const snapshotService = new ycsdk.SnapshotService();
const diskService = new ycsdk.DiskService();

async function handler(event, context) {

    const diskList = await diskService.list({
        folderId: FOLDER_ID,
    });

    for (const disk of diskList.disks) {
        if ('snapshot-critical' in disk.labels) {
            snapshotService.create({
                folderId: FOLDER_ID,
                diskId: disk.id,
                name: disk.name + "-" + yyyymmddhhmmss,
                labels: label
            });
        }
    }

   return {
        statusCode: 200,
        body: JSON.stringify({
            event: event,
            context: context,
            labels: label
        })
    }

}
exports.handler = handler;
```

### Создание моментальных снимков по умолчанию с жизненным циклом 14 дней

Будет запускаться один раз в неделю

**create-snapshot-default**

```
// Префикс имени диска ГГГГ-ММ-ДД
let today = new Date();
var yyyymmdd = today.toISOString().substring(0, 10);
var hh = today.getHours().toString();
var mm = today.getMinutes().toString();
var ss = today.getSeconds().toString();
var yyyymmddhhmmss = yyyymmdd + "-" + hh + mm + ss;

// Метка диска жизненный цикл
let label = {
   "lifecycle": "21"
};

const ycsdk = require("yandex-cloud/api/compute/v1");
const FOLDER_ID = process.env.FOLDER_ID;
const snapshotService = new ycsdk.SnapshotService();
const diskService = new ycsdk.DiskService();

async function handler(event, context) {

    const diskList = await diskService.list({
        folderId: FOLDER_ID,
    });

    for (const disk of diskList.disks) {
        if ('snapshot-default' in disk.labels) {
            snapshotService.create({
                folderId: FOLDER_ID,
                diskId: disk.id,
                name: disk.name + "-" + yyyymmddhhmmss,
                labels: label
            });
        }
    }

   return {
        statusCode: 200,
        body: JSON.stringify({
            event: event,
            context: context,
            labels: label
        })
    }

}
exports.handler = handler;
```

### Удаление моментальных снимков с истекшим жизненным циклом

Будет выполнятся каждый день

**delete-snapshot-lifecycle**

```
// yc compute snapshot list - список дисков
// yc compute snapshot get <id> - метка диска
// yc compute snapshot update --id <id>  --labels lifecycle=14 // изменение метки

const today = Date.parse(new Date());
const ycsdk = require("yandex-cloud/api/compute/v1");
const FOLDER_ID = process.env.FOLDER_ID;
const snapshotService = new ycsdk.SnapshotService();
const diskService = new ycsdk.DiskService();

module.exports.handler = async function (event, context) {

    const snapList = await snapshotService.list({
    folderId: FOLDER_ID
    });

    for (const snapshot of snapList.snapshots) {
        if ('lifecycle' in snapshot.labels) {
            let createdAt = snapshot.createdAt.seconds.low * 1000;
            let diff = (today - createdAt) / (60 * 60 * 24 * 1000);
            let snapid = snapshot.id;
            let d = snapshot.labels.lifecycle;
            //throw new Error(diff);
            //throw new Error(d);
            if (diff > d) {
                snapshotService.delete({
                folderId: FOLDER_ID,
                snapshotId: snapshot.id
                });
            }
        }
    }

    return {
        statusCode: 200,
        today: `${today}`,
        body: FOLDER_ID//,
        //createdAt: `${createdAt}`,
    };
};
```

## Создадим триггеры таймеры

Описание cron выражение https://cloud.yandex.ru/docs/functions/concepts/trigger/timer#cron-expression

### Создание моментальных снимков каждый день в 18:00

**cron-snapshot-create-critical**

Cron-выражение: 00 18 ? * 2-6 *
Функция: create-snapshot-critical

### Создание моментальных снимков каждое воскресение в 18:00

**cron-snapshot-create-default**

Cron-выражение: 00 18 ? * 1 *
Функция: create-snapshot-default

### Удаление моментальных снимков каждый день в 12:00

**cron-snapshot-delete-lifecycle**

Cron-выражение: 00 12 ? * * *
Функция: delete-snapshot-lifecycle

## Просмотр моментальных снимков
(yc compute snapshot list --format json | ConvertFrom-Json) | Select id,name,labels,status
