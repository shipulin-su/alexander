---
title: "Удаление загрузок"
permalink: /docs/aws_cli/abort-multipart-upload/
toc: true
---

## Удаление загрузки
Описание
```
aws --endpoint-url=https://storage.yandexcloud.net s3api abort-multipart-upload --bucket <bucket> --key <Key> --upload-id <UploadId>
```
где <bucket> - имя бакета
    <Key> - имя файлв
    <UploadId> - id загрузкиу

Пример. Выборочное удаление
```
aws --endpoint-url=https://storage.yandexcloud.net s3api abort-multipart-upload --bucket arctl-backup-files --key pg-pro/etl_arc_20190729_020002.backup --upload-id 00058F419C4A53EA
```


## Массове удаление загрузок

1. Выгружаем спсиок в фалй json
2. Конфервтируем JSON в исполянемый файл
3. Выполняем исполняемый файл

### Конвертация list-multipart-uploads.json в исполянемый файл powershell

```
# Читаем json
$j = Get-Content C:\tmp\list-multipart-uploads.json

## Конфертируем в bat
($j | ConvertFrom-Json) |
select -expand Uploads |
ForEach-Object { "aws --endpoint-url=https://storage.yandexcloud.net s3api abort-multipart-upload --bucket arctl-backup-files --key $($_.Key) --upload-id $($_.UploadId) # Initiated: $($_.Initiated)" } |
Out-File c:\tmp\abort-multipart-upload.ps1
```
