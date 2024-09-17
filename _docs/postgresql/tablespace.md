---
title: "Перенос базы на другой диск"
permalink: /docs/postgresql/tablespace/
toc: false
---

# tablespace - Перенос базы данных на другой диск
{: .no_toc }

## Порядок действий
{: .no_toc .text-delta }

1. TOC
{:toc}

## Присоеденяем диск к виртуальной машине

- Останавливаем виртуальную машину
- Добавляем диск
- Присоеденяем диск

## Добавляем (монтируем) новый диск

### Создем папку
```
sudo mkdir /mnt/pg_base
```

### Получем список дисков
```
sudo fdisk -l
```

### Форматируем диск
```
sudo mkfs.ext4 /dev/vdb
```

### Монтируем диск
```
sudo mount /dev/vdb /mnt/pg_base
```

### Добавляем диск в автозагрузку
```
blkid
sudo nano /etc/fstab
UUID=1116f123-c4c3-406a-8c06-9e694ad0cc9d /mnt/pg_base/ ext4
```

### Создем папку для базы данных
```
sudo mkdir /mnt/pg_base/tetra
```

### Даем разрешения для папки
```
sudo chown postgres /mnt
sudo chown postgres /mnt/pg_base
sudo chown postgres /mnt/pg_base/tetra
```

## Создаем место под диск
```
CREATE TABLESPACE ts_tetra
  OWNER postgres
  LOCATION '/mnt/pg_base/tetra/';
```

## Перенсим базу даных в новое место
```
ALTER DATABASE tetra
  SET TABLESPACE ts_tetra;
```
