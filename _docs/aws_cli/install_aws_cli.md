---
title: "Установка AWS CLI"
permalink: /docs/aws_cli/install_aws_cli/
toc: true
---

# Установка AWS CLI

орядок установки
1. Загрузить нужную версию [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
2. Установить AWS CLI
3. В коммандной среде cmd или powershell выполнить
```
aws configure
```
после чего будет запрошен
- <AWS Access Key ID> - идентификатор ключа
- <AWS Secret Access Key> - секретный ключ
- <default region > - укказать `ru-central1` (не обязательно)


Важной! При использованиии для Yandex Object Storege
нужно всегда указвать параметр `--endpoint-url`

Пример:
```
aws --endpoint-url=https://storage.yandexcloud.net \
    s3 ls --recursive s3://<bucket-name>
```
где <bucket-name> - имя бакета

[Документация](https://docs.aws.amazon.com/cli/latest/reference/s3/)
