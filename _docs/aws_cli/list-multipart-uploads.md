---
title: "Список загрузок"
permalink: /docs/aws_cli/list-multipart-uploads/
toc: true
---

## Получение спсиска загрузок

Описание
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-multipart-uploads --bucket <bucket>
```
где <bucket> - имя вашего бакета


примеры для вызова из Power Shell

Пример. С выводом на экран
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-multipart-uploads --bucket arctl-backup-files
```

Пример. С фильтром по префику 'pg_dump_sheduled/pg' в пути
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-objects --bucket arctl-backup-files --prefix pg_dump_sheduled/pg
```

Пример. С фильтром по префику 'pg_dump_sheduled/pg' в пути и первые 5
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-objects --bucket arctl-backup-files --prefix pg_dump_sheduled/pg --max-items 5
```

Важно! при использваонии в cmd заменить ковычки c `"` на `'`

Пример. С фильтром по префику 'ColibriPacket_' и сортировкой по LastModified
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-objects --bucket colibri-distrib --prefix ColibriPacket_ --query 'reverse(sort_by(Contents,&LastModified))[]'
```

Пример. Получения последнего измененного файла с префиксом
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-objects --bucket colibri-distrib --prefix ColibriPacket_ --query 'reverse(sort_by(Contents,&LastModified))[0]'
```

Пример. Получения последнего измененного файла с префиксом и выводом имени и даты
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-objects --bucket colibri-distrib --prefix ColibriPacket_ --query 'reverse(sort_by(Contents,&LastModified))[0].{Key: Key,  date: LastModified}'
```

Пример. С выводом в файл json
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-multipart-uploads --bucket arctl-backup-files > c:\tmp\list-multipart-uploads.json
```
