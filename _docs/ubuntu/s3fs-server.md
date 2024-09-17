---
title: "file-server"
permalink: /docs/ubuntu/s3fs/
toc: true
---

## Монтируем диск object storage

[manpages.ubuntu.com](http://manpages.ubuntu.com/manpages/focal/man1/s3fs.1.html)

### Установливаем s3fs
s3fs — программа для Linux позволяющя монтировать объекты Object Storage
```
sudo apt install s3fs
```

### Создаем сервисный аккаунт
https://console.cloud.yandex.ru/folders/b1gdf022fs6ma7rj92d8?section=service-accounts
с ролями:
- storage.viewer
- storage.uploader

### Сохраняем серетный ключь в файл
Выполняем скрипт
Для запуска от текщего пользователя `/root/.passwd-s3fs`
```
echo <идентификатор ключа>:<секретный ключ> >  /root/.passwd-s3fs
chmod 600  /root/.passwd-s3fsre
```
Для автозагрузки `/etc/passwd-s3fs`
```
echo <идентификатор ключа>:<секретный ключ> >  /root/.passwd-s3fs
chmod 600  /root/.passwd-s3fsre
```

### Добавляем в автозагрузку

Добавляем запись в /etc/fstab
Важно! В этом случае пароль должен храниться в папке `/root/.passwd-s3fs`

```
sudo echo "arctl-backup-files /mnt/obj fuse.s3fs _netdev,allow_other,use_path_request_style,url=http://storage.yandexcloud.net 0 0" | sudo tee -a /etc/fstab
```

Где
  `_netdev` - Файловая система находится на устройстве, которое требует доступа к сети (проверка наличия сети)
  `rw` - режим "чтение/запись"
  `allow_other` - отменяет безопасности, ограничивающую доступ к монтированому файлу

Примечание. Описание опций
- https://ru.wikipedia.org/wiki/Fstab
- https://manpages.ubuntu.com/manpages/focal/man1/s3fs.1.html
- https://www.opennet.ru/man.shtml?topic=mount&category=8

Проверяем добавление
`sudo nano /etc/fstab`

Вариант с размещение кэша на дркугом диске -o use_cache
```
sudo echo "arctl-backup-files /mnt/obj fuse fuse.s3fs _netdev,allow_other,use_cache=/mnt/vdb,use_path_request_style,url=http://storage.yandexcloud.net 0 0" | sudo tee -a /etc/fstab
```

### Монтируем диск от текущего пользователя

```
sudo s3fs arctl-backup-files /mnt/obj -o passwd_file=~/.passwd-s3fs \
    -o url=http://storage.yandexcloud.net -o use_path_request_style
```

Вариант с размещение кэша на дркугом диске -o use_cache
```
sudo s3fs arctl-backup-files /mnt/obj -o passwd_file=~/.passwd-s3fs \
    -o use_cache=/mnt/vdb -o url=http://storage.yandexcloud.net -o use_path_request_style
```
