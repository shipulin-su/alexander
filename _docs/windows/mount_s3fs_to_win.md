---
title: "Монтирование диска s3fw в Win Srv"
permalink: /docs/windows/mount_s3fs_to_win/
toc: true
---

# Установка клиент NFS


Установить в Server Manager

Вариант 1.
- Features
  - Client for NFS
  - Remote Server Administration Tools
    - Role Administration Tools ->
      - File Services Tools
        - Services for Network File System Management Tools

Вариант 2.
```
Install-WindowsFeature NFS-Client, RSAT-NFS-Admin
```

Запустить

## Ссылки
    (https://mcs.mail.ru/help/disks-and-images/filestorage)
