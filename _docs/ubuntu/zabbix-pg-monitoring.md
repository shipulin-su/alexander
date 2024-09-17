---
title: "zabbix-pg-monitoring"
permalink: /docs/ubuntu/zabbix-pg-monitoring/
toc: true
---

# Template DB PostgreSQL

[zabbix.com - Postgresql](https://www.zabbix.com/integrations/postgresql)
[zabbix.com - Template DB PostgreSQL](https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/db/postgresql)

## Установить Zabbix agent

### Создать пользователя read-only zbx_monitor
For PostgreSQL version 10 and above:
```
CREATE USER zbx_monitor WITH PASSWORD '<PASSWORD>' INHERIT;
GRANT pg_monitor TO zbx_monitor;
```

## Копировать postgresql/.sql в Zabbix agent
Загрузить репозиторий с файлами templates
```
mkdir -p ~/zabbix/postgresql
sudo wget -P ~/zabbix/postgresql/ https://git.zabbix.com/rest/api/latest/projects/ZBX/repos/zabbix/archive?format=zip
```
Из папки (храниться основные метрики мониторинга за postgresql)
```
~/zabbix/postgresql/archive?format=zip/templates/db/postgresql/postgresql/
```
скопировать в
```
/var/lib/zabbix/postgresql/
```

## Копировать template_db_postgresql.conf в Zabbix agent
Из папки (храниться основные метрики мониторинга за postgresql)
```
~/zabbix/postgresql/archive?format=zip/templates/db/postgresql/
```
скопировать
```
 /etc/zabbix/zabbix_agentd.d/
 ```
и перезапустить Zabbix agent
```
service zabbix-agent restart
```
## Отредактировать pg_hba.conf, где разрешить подключение Zabbix agent
https://www.postgresql.org/docs/current/auth-pg-hba-conf.html.
```
host all zbx_monitor 127.0.0.1/32 trust
host all zbx_monitor 0.0.0.0/0 md5
host all zbx_monitor ::0/0 md5
```

Перезапустить PostgreSQL
```
sudo /etc/init.d/postgresql restart
```
