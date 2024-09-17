---
title: "FTP Сервер"
permalink: /docs/ubuntu/ftp-server/
toc: true
---

Важно!
Протоколо FTP разработан 40 лет назад.
Безопасную передачу данных можно осуществлять через SSL.
Крпные ИТ производители перестали поддерждивать его в своих приложениях.
Реализы популярного FTP сервера не выпускаются с 2015 года
[security.appspot.com](https://security.appspot.com/vsftpd.html).
Считается более надежный протокло SFTP он как правило встроен

# Установка ftp

Устанавливаем сервер FTP с доступом по SSL.

Установка командной оболочки и текстового редактора
```
sudo apt install mc nano
```

Установка сервера FTP
```
sudo apt install vsftpd
```

## Добавление сервер в автозагрузку

```
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

## Выпускаем сертефикат SSL
```
sudo openssl req -x509 -nodes -days 720 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/private/vsftpd.pem -subj "/C=RU/ST=/L=E-burg/O=arc/OU=it/CN=ftp.arctl.ru/CN=ftp.arctl.ru"
```

## Выполяняем конфигурирование

### Делаем копию конфигурации
```
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
```

Восстановление из копии
```
sudo cp /etc/vsftpd.conf.orig /etc/vsftpd.conf
```

### Изменяеме фалй настроек

Открываем файл
```
sudo nano /etc/vsftpd.conf
```

Приводим его к виду
```
# Обязательные настройки
anon_upload_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES

# Используется для доступа через SSL
ssl_enable=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_ciphers=HIGH
pasv_min_port=40000
pasv_max_port=50000
```

Важно! Папка FTP сервера по умолчанию `/srv/ftp`

## Добавление пользователя

Добавление пользователя
```
sudo useradd ftpuser -d /srv/ftp -s /bin/false -m
```
где `-d /srv/ftp` - домашня папка по-умолчанию
    `-s /bin/false` - запрет на вход в систему

Установка пароля
```
sudo passwd ftpuser
```

Смена домашней папки пользователя.
Если пользователь был добавлен ранее.
```
sudo usermod -md /srv/ftp ftpuser
```
где md - опция смены домашней папки

Удаление пользователя.
Если нужно удалить доступ.
```
userdel -r ftpuser
```
где r - удаление катологов пользователя

## Разадача прав на FTP

Вариант 1. Установка владельца папки
```
sudo chown ftpuser:ftpuser /srv
sudo chown ftpuser:ftpuser /srv/ftp
sudo chown ftpuser:ftpuser /srv/ftp/colibri-distrib
```

Вариант 2. Раздача прав
```
chmod a-wx /srv/ftp/
chmod a+r /srv/ftp/
chmod a+rwx /srv/ftp/colibri-distrib
chmod a+rwx /srv/ftp
chmod a+rwx /srv
```

Проверка прав
```
sudo ls -la /srv
sudo ls -la /srv/ftp
```

## Перезапуск сервера FTP

Перед проверкой выполняем перезапуск сервера FTP
```
sudo systemctl restart vsftpd
```
