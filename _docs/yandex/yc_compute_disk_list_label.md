---
title: "Список дисков с label"
permalink: /docs/yandex/yc_compute_disk_list_label/
toc: true
---

# Вариант с использование ConvertFrom-Json

Список дисков
```
(yc compute disk list --format json | ConvertFrom-Json) | Select id,name,status,labels
```

Список снимков
```
(yc compute snapshot list --format json | ConvertFrom-Json) | Select id,name,labels,status
```

# Вариант с использование jq
Чтобы получить список дисков с label
Потребуется использовать jq (JSON-процессор командной строки) а для его установки
chocolatey (менеджер пакетов в среде Windows по аналогии с apt-get в Linux)

https://stedolan.github.io/jq/
https://chocolatey.org/install#individual

# Инструкция для Windows

## Установим chocolatey

Запустим PowerShell от админа и выполним.
Последнюю версию лучшее брать с сайта (chocolatey.org)[https://chocolatey.org/install#individual]
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

## Установим jq
```
choco install jq
```

## Выполним запрос
```
yc compute disk list --format json | jq -r ‘.[]|[.id, .name, .labels[]]|@tsv’
```
