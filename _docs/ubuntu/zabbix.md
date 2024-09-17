---
title: "zabbix"
permalink: /docs/ubuntu/zabbix/
toc: true
---

# Установка (install) Zabbix

Устанавливаем Zabbix с использованием:
- nginx
- PostgreSQL

Скачать и установить Zabbix [ссылка на zabbix.com](https://www.zabbix.com/ru/download?zabbix=5.4&os_distribution=ubuntu&os_version=20.04_focal&db=postgresql&ws=nginx)

## Подготовка

```
sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove
sudo apt-get install mc
sudo apt-get install nano
sudo mc
sudo timedatectl set-timezone Asia/Yekaterinburg
sudo locale-gen ru_RU
sudo locale-gen ru_RU.UTF-8
sudo apt install traceroute
```

## Установка репозитория Zabbix:

[Выбрать платформу на zabbix.com](https://www.zabbix.com/ru/download)

```
sudo wget https://repo.zabbix.com/zabbix/<взять с сайта>
sudo dpkg -i zabbix-release_<взять с сайта>
sudo apt update
```

### Установка zabbix-server, frontend, php, nginx, agent:

Возможно потребуется установить php
```
sudo apt install php
```

установка компонентов с nginx
```
sudo apt -y install zabbix-server-pgsql zabbix-frontend-php zabbix-nginx-conf php-pgsql zabbix-agent
```

## Создать базу данных в PostgreSQL

Добавить роль
```
sudo -u postgres createuser --pwprompt zabbix
```

Добавить базу
```
sudo -u postgres createdb -O zabbix zabbix
```

Импортировать схему
```
sudo zcat /usr/share/doc/zabbix-server-pgsql*/create.sql.gz | sudo -u zabbix psql zabbix
```

## Настройть базу данных для Zabbix

В файле
```
sudo nano /etc/zabbix/zabbix_server.conf
```

Изменить строку
```
DBPassword=password
```

## Настройте PHP для веб-интерфейса

В файле /etc/zabbix/nginx.conf
```
sudo nano /etc/zabbix/nginx.conf
```

Снимаем комментарий
```
listen 8080;
server_name _;
```

перезапустить nginx
```
nginx -s reload
```

Важно! сервер будет доступен по адресу localhost:8080

В файле /etc/zabbix/php-fpm.conf

```
sudo nano /etc/zabbix/php-fpm.conf
```

изменемя врменную зонц

```
php_value date.timezone Asia/Yekaterinburg
```

## Запускаем процессы Zabbix сервера и агента

Запуск при загрузке ОС:
```
sudo systemctl restart zabbix-server zabbix-agent nginx php7.2-fpm
sudo systemctl enable zabbix-server zabbix-agent nginx php7.2-fpm
```

## Настроить postgresql

### Настроить pg_hba.conf

Настройка доступа по сетям, в файле pg_hba.conf
```
sudo nano /etc/postgresql/10/main/pg_hba.conf
```

Пример: разрешить доступ локально из подсетей в режиме trust
```
# Database administrative login by Unix domain socket
local   all             postgres                                trust

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
```

### Настроить postgresql.conf

Настройка доступа по адресам, в файле postgresql.conf
```
sudo nano /etc/postgresql/10/main/postgresql.conf
```

Пример: Доступ со всех адресов
```
# - Connection Settings -
#listen_addresses = 'localhost'
listen_addresses = '*'
```

## Настройте веб-интерфейс Zabbix

Открываем веб-интерфейс Zabbix: http://[IP_ADRES]:8080
Имя пользователя: Admin
Пароль: zabbix

Важно! После входа необходимо поментья пароль пользователя

## Запуск и проверка статуса

```
service zabbix-server start
```

```
service zabbix-server status
```

## Обновление

https://www.zabbix.com/documentation/5.0/ru/manual/installation/upgrade/packages/debian_ubuntu

```
sudo service zabbix-server stop
sudo rm -Rf /etc/apt/sources.list.d/zabbix.list
sudo wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+bionic_all.deb
sudo dpkg -i zabbix-release_5.0-1+bionic_all.deb
sudo apt update
sudo apt-get install --only-upgrade zabbix-server-pgsql zabbix-frontend-php zabbix-nginx-conf php-pgsql zabbix-agent
sudo service zabbix-server start
```
