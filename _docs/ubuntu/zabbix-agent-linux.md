---
title: "zabbix-agent-linux"
permalink: /docs/ubuntu/zabbix-agent-linux/
toc: true
---

# Установка (install) Zabbix Agent

[Zabbix Агенты](https://www.zabbix.com/ru/download_agents)

## Ubuntu

### Установите репозиторий Zabbix:
```
sudo wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+focal_all.deb
sudo dpkg -i zabbix-release_5.0-1+focal_all.deb
sudo apt update
```

### Установите Zabbix агента:
```
sudo apt -y install zabbix-agent
```

### Настройка Zabbix агента
в файле
```
sudo nano /etc/zabbix/zabbix_agentd.conf
```
указать
```
Server=[IP-ADDRESS]
ServerActive=[IP-ADDRESS]
Hostname=[HOSTNAME]
```
где [IP-ADDRESS] - адрес zabbix сервера;
    [HOSTNAME]   - имя компьютера где запущен агент

Важно! [HOSTNAME] должен совпадать с [Имя узла сети] в zabbix

### Установить nmap
Утилита проверки сети и аудита безопасности
```
sudo apt update
sudo apt install nmap
```
Разрешение на запуск
```
sudo nano /etc/sudoers
```
и дописать
```
zabbix  ALL=(root) NOPASSWD: /usr/bin/nmap
```
или дописать автоматом (рекомендуемый вариант)
```
sudo echo "zabbix  ALL=(root) NOPASSWD: /usr/bin/nmap" >> /etc/sudoers
```

### Запуск
```
sudo systemctl enable zabbix-agent
sudo service zabbix-agent start
```

Важно! Первый раз нужно сделать перезапуск агента.
```
sudo service zabbix-agent restart
```

Перезагрузка агента
```
systemctl reload zabbix-agent
```

## Проверка агента

Проверка что агент запущен
```
ps aux | grep zabbix
```

Проверка статуса
```
systemctl status zabbix-agent
```

Проверка доступности агента с сервера
```
nc -v -z <IP Агента> 10050
```

Проверка доступности сервера с агента
```
nc -v -z <IP Агента> 10051
```

Открытие порта если не доступен
```
iptables -A INPUT -s <IP Сервера>32 -p tcp -m tcp --dport 10050 -j ACCEPT
iptables-save > /etc/iptables/rules.v4
```
