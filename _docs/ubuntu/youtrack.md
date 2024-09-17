---
title: "youtrack"
permalink: /docs/ubuntu/youtrack/
toc: true
---

# Установка и обновление YouTrack (Install / Upgrade﻿)

## Подготовка

Обновление репозиториев, установка коммандера, редактора и временной зоны
```
sudo apt-get update
sudo apt-get install mc
sudo apt-get install nano
sudo timedatectl set-timezone Asia/Yekaterinburg
```

## Установка java

Проверка версии java
```
java -version
```

Если нет, то установка пакета java
```
sudo apt-get install java
```

## Установка JRE

Установка JRE (Java Runtime Environment)
```
sudo apt install default-jre
```

## Установка пакета rng-tools

Установим rng-tools - компонент необходимы для YouTrack
```
sudo apt-get install rng-tools
```

## Установка YouTrack

[Инструкция на jetbrains.com](https://www.jetbrains.com/help/youtrack/standalone/Install-YouTrack-JAR-as-Service-Linux.html)

### 1. Добавить пользователя

Создание пользователя для запуска службы YouTrack
```
sudo adduser youtrack --disabled-password
```

### 2. Загрузка релиза

Для загрузки используем wget
```
wget -O /home/youtrack/youtrack.jar https://download.jetbrains.com/charisma/youtrack-<version>.jar
```
Где
- version - номер версии. Например: 2021.1.14501

Последнюю версию можно узнать по [ссылке](https://www.jetbrains.com/youtrack/download/get_youtrack.html#section=standalone), где перейти в **Release Notes**.

### 3. Выдаем права на архив
```
chown youtrack:youtrack /home/youtrack/youtrack.jar
```

### 4. Создаем в systemd службу

Путь где надо создать службу
```
/etc/systemd/system/youtrack.service
```

Содержимое в файле службы
```
[Unit]
Description=Youtrack
Requires=network.target
After=syslog.target network.target

[Service]
Type=simple
WorkingDirectory=/home/youtrack
ExecStart=/usr/bin/java -jar /home/youtrack/youtrack.jar --J-Xmx1G 8080
User=youtrack

[Install]
WantedBy=default.target
```

### 5. Настройка службы

Инструкция содержит перезагрузку демона, включение службы
```
sudo systemctl daemon-reload
sudo systemctl enable youtrack.service
```

### 6. Запуск службы

```
sudo service youtrack start
```

Возможно потребуется перезагрузка
```
sudo reboot
```

### 7. Открытие YouTrack

```
http://<your-host-name>:8080
```
где
- your-host-name - хост на котором установлен YouTrack

## Восстановление из backup

[Инструкция на jetbrains.com](https://www.jetbrains.com/help/youtrack/standalone/Restore-JAR-Installation.html)

Кратка инструкция для случая когда восстанавливаем в новом месте:
- установить YouTrack
- скачать backup
- запустить YouTrack
- выбрать режим восстановление из backup

### Скачать backup youtrack.tar.gz
в папку /home/youtrack/
```
wget -O /home/youtrack/youtrack.tar.gz <url>
```
где
- url - адрес загрузки


## Обновление (Upgrade﻿)

[Инструкция на jetbrains.com](https://www.jetbrains.com/help/youtrack/standalone/Install-YouTrack-JAR-as-Service-Linux.html#upgrade-jar-as-service-linux)

Краткая инструкция по обновлению:
- Удалить предыдущую версию
- Загрузить последнюю версию
- Установить разрешения на файл
- Перезапустить

```
mv /home/youtrack/youtrack.jar /home/youtrack/youtrack-old.jar
wget -O /home/youtrack/youtrack.jar https://download.jetbrains.com/charisma/youtrack-<version>.jar
chown youtrack:youtrack /home/youtrack/youtrack.jar
service youtrack restart
```
