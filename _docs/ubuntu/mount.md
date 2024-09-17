---
title: "mount"
permalink: /docs/ubuntu/mount/
toc: true
---

https://cloud.yandex.ru/docs/storage/tools/s3fs

# Mount - Добавляем (монтируем) новый диск
В Upuntu присоденяются новый диск а новая папка.
Ниже пример монтрования нового диска data.

## Создем папку, где будет находиться диск
`sudo mkdir /mnt/vdb`

## Получем список дисков
Cписка дисков
`sudo fdisk -l`
или
`cat /proc/partitions`
Спсиок дисков с UUID
`blkid`

## Форматируем диск
Диск будет отфрмаматирован с именем data
```
sudo mkfs -t ext4 -L vdb /dev/vdb
```
где `/dev/vdb` - диск найденый на предыдущем шаге

## Монтируем диск
`sudo mount /dev/vdb /mnt/vdb`

## Добавляем диск в автозагрузку

### Вариант 1. По имени (Реекомендуемый)
```
sudo echo "LABEL=vdb /mnt/vdb ext4 defaults 0 0" | sudo tee -a /etc/fstab
```
Проверяем добавление
`sudo nano /etc/fstab`

### Вариант 2.
Выполняем команду показывающую UUID дисков
`blkid`
Запоимнаем UUID нужного диска, заменяем в скрипте и выполняем
```
echo <UUID> /mnt/vdb ext4 defaults 0 0" | sudo tee -a /etc/fstab
```
Проверяем добавление
`sudo nano /etc/fstab`

## Размонтировать диск
Примеры
```
umount /srv/ftp/colibri-distrib
```
