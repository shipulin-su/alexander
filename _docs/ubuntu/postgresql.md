---
title: "postgresql"
permalink: /docs/ubuntu/postgresql/
toc: true
---

# Установка (Install) PostgreSQL

Пункты с маркировкой `(дополнительно)` приведены справочно, действий не требую  

## Подготовка

```
sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove
sudo timedatectl set-timezone Asia/Yekaterinburg
sudo apt-get install nano
sudo apt-get install mc
sudo mc
```

## Установка locale

[Инструкция help.ubuntu.ru](https://help.ubuntu.ru/wiki/%D1%80%D1%83%D1%81%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F_ubuntu)

Просмотр установленных locale

```
locale -a | grep ru
```

Установка locale, через консоль (рекомендуемый вариант)

```
sudo locale-gen ru_RU
sudo locale-gen ru_RU.UTF-8
```

Проверка установики

```
locale -a | grep ru
```

## Операции с locale (дополнительно)

Альтернативный вариант или через графический интерфейс

```
sudo dpkg-reconfigure locales
```

Переустановка locale, выполнять в случае проблем

```
sudo apt-get install --reinstall locales
```

Обновление locale, Выполнять в случае проблем

```
sudo update-locale
sudo update-locale LANG=ru_RU.UTF-8
```

Проверка locale по умолчанию

```
sudo nano /etc/default/locale
```

Если должны быть RU, то привести к виду ниже

```
LANGUAGE=ru_RU:ru
LANG=ru_RU.UTF-8
```

## Установка языкового пакета language-pack-ru (дополнительно)

Важно! ubuntu станет русская
Выполнять в случае проблем в приложении

```
sudo apt-get install language-pack-ru
sudo reboot
```

# Установка PostgreSQL

[postgresql.org - Linux downloads (Ubuntu)](https://www.postgresql.org/download/linux/ubuntu/)

Создать файл с репозиторием

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

Импортировать ключ подписи

```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

Установить последнюю версию

```
sudo apt-get update
sudo apt-get -y install postgresql
```

Если нужно установить определенную версию например 14

```
apt-get install postgresql-14
```

устаревшие версии: postgresql-11, postgresql-10, postgresql-9.6

Проверить работу кластера, посмотрев список установленных кластеров

```
pg_lsclusters
```

Запуск кластера

```
sudo service postgresql start
```

Остановка кластера

```
sudo service postgresql stop
```

Перезагрузка конфигурации

```
sudo service postgresql reload
```

## Настроить pg_hba.conf

Настройка доступа по сетям, в файле `pg_hba.conf`

Пример для версии 14
```
sudo nano /etc/postgresql/14/main/pg_hba.conf
```

Доступ для подсетей через md5
```
# IPv4 local connections:
host    all             all             10.128.0.0/24           md5
host    all             all             10.129.0.0/24           md5
host    all             all             10.130.0.0/24           md5
```

Доступ без авторизации (не рекомендуемый вариант) (дополнительно)

```
# IPv4 local connections:
host    all             all             10.128.0.0/24           trust
host    all             all             10.129.0.0/24           trust
host    all             all             10.130.0.0/24           trust
```

Доступ для DataLens (дополнительно)
```
# DataLens
host    all             all             178.154.242.176/28      trust
host    all             all             178.154.242.192/28      trust
host    all             all             178.154.242.208/28      trust
host    all             all             178.154.242.128/28      trust
host    all             all             178.154.242.144/28      trust
host    all             all             178.154.242.160/28      trust
```

## Настроить postgresql.conf

Настройка доступа по адресам, в файле `postgresql.conf`

Пример для версии 14
```
sudo nano /etc/postgresql/14/main/postgresql.conf
```

Доступ со всех адресов (изменить)
```
# - Connection Settings -
listen_addresses = '*'
```

Настройка авторизации (проверить)
```
# - Authentication -
password_encryption = scram-sha-256     # scram-sha-256 or md5
```

Настройка SSL (изменить)
```
# - SSL -
ssl = on
ssl_cert_file = '/etc/ssl/certs/ssl-cert-snakeoil.pem'
ssl_key_file = '/etc/ssl/private/ssl-cert-snakeoil.key'
```


Доступ только локально
```
# - Connection Settings -
listen_addresses = 'localhost'
```

## Установка пароля postgres

Зайти в psql
```
sudo -u postgres psql template1
```

Выполнить скрипт изменяющий пароль. Важно! [Пароль] заменить на нужный

```
ALTER USER postgres with encrypted password '<PASSWORD>';
```

Выйти из psql

```
\q
```

Перезапустить PostgreSQL
```
sudo /etc/init.d/postgresql restart
```

# Проверка версии Postgres (дополнительно)

Версия psql

```
sudo psql --version
```

Установленные пакеты

```
dpkg -l | grep postgres
```

# Удаление Postgres (дополнительно)

Список установленных пакетов
```
dpkg -l | grep postgres
```

Удаление пакетов
```
sudo apt-get --purge remove <НазваниеПакета>
```

Проверка удалённых пакетов
```
dpkg -l | grep postgres
```
