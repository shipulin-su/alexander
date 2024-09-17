---
title: "file-server"
permalink: /docs/ubuntu/file-server/
toc: true
---

# Фйаловый серевр с бакетом Object Storage

## Ссылки

- Инстркция по созданию файлового сервера
  (ссылка)[https://cloud.yandex.ru/docs/solutions/archive/single-node-file-server]
- Инстркция по монтированию бакета Object Storage
  (ссылка)[https://cloud.yandex.ru/docs/storage/tools/s3fs]

## Установка mc и nano

```
sudo apt-get update
sudo apt-get install mc nano
```

## Создаем папку для монитрования
`sudo mkdir /mnt`
`sudo mkdir /mnt/obj`

## Установливаем Samba

```
sudo apt-get install nfs-kernel-server samba
```

### Конфигурируем NFS в файле
Запускаем
```
sudo nano /etc/exports
```
Дописываем разрешения
```
/mnt/obj <IP-Adres>(rw,no_subtree_check,fsid=100)
/mnt/obj 127.0.0.1(rw,no_subtree_check,fsid=100)
```
где <IP-Adres> - адреса с которых будем подключать диск

Например:
```
/mnt/obj 10.130.0.3(rw,no_subtree_check,fsid=100)
/mnt/obj 10.128.0.3(rw,no_subtree_check,fsid=100)
/mnt/obj 127.0.0.1(rw,no_subtree_check,fsid=100)
```

## Конфигурируем Samba

```
sudo nano /etc/samba/smb.conf
````

Файл должен принять следющий вид:
```
[global]
   workgroup = WORKGROUP
   server string = %h server (Samba)
   dns proxy = no
   log file = /var/log/samba/log.%m
   max log size = 1000
   syslog = 0
   panic action = /usr/share/samba/panic-action %d
   server role = standalone server
   passdb backend = tdbsam
   obey pam restrictions = yes
   unix password sync = yes
   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes
[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700
[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
[data]
   comment = /data
   path = /mnt/obj
   browseable = yes
   read only = no
   writable = yes
   guest ok = yes
   hosts allow = 10.130.0.3 10.128.0.3 127.0.0.1
   hosts deny = 0.0.0.0/0
```

### Раздача прав
Дадим полные права
```
sudo chmod -R 777 /mnt
```

### Перезапускаем службы
Перезапукс nfs-kernel
```
sudo service nfs-kernel-server restart
```
Перезапукс smbd
```
sudo service smbd restart
```

## Подключение к диску из windows
windows
```
net use x: \\<публичный IP-адрес виртуальной машины>\data
```
