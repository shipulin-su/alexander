---
title: "Изменение поля"
permalink: /docs/yandex/tracker_fields/
toc: true
---

Раздел содержит недокументированные возможности
[REST API Яндекс.Трекера](https://yandex.ru/dev/connect/tracker/api/about.html) (на момент написания).

Создание поля выполняется из интерфейса, но редактирования нет.

Выполним изменение используя HTTP-запрос с методом POST.
Параметры тела в формате JSON.
```
POST /v2/fields/categories?expand=fields HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-ID: <orgid>
Content-Type: application/json

{
    "name": {
        "name": "<newname>"
    }
}    

```
где
- `<token>` - ваш токен
- `<orgid>` - номер организации
- `<en>` - название категории на английском
- `<ru>` - название категории на русском
- `<description>` - описание категории
- `<order>` - порядок отображения

## Получить категории
```
GET /v2/fields/categories HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-Id: <orgid>
```

## Изменить категорию
```
PATCH /v2/fields/categories/<categorid> HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-Id: <orgid>
If-Match: "<version>"
Content-Type: application/json

{
"name": {
"en": "Cargo 2",
"ru": "Груз 2"
},
"description": "",
"order": 300
}
```
где
- `<version>` - текущая версия категории
- `<categorid>` - id категории
