---
title: "nginx"
permalink: /docs/ubuntu/nginx/
toc: true
---

# Подготовка к установке

```
sudo apt-get update    
sudo apt-get install mc
sudo apt-get install nano
sudo mc			
```

## Установить временную зону

```
sudo timedatectl set-timezone Asia/Yekaterinburg
```

# Установка nginx

```
sudo apt-get install nginx
```		

# Настройка nginx

В директории `/etc/nginx/conf.d/demo`
Создать файлы конфигураций для сайта

Например пример mysite.arctl.ru.conf	   	
```
server {
		server_name mysite.mydomen.ru www.mysite.mydomen.ru;
		location / {
				proxy_pass http://130.193.41.115:80;
				proxy_read_timeout 1200;
				proxy_send_timeout 1200;
				proxy_buffer_size 128k;
				proxy_buffers 4 256k;
				proxy_busy_buffers_size 256k;
				proxy_set_header Host $host;
				proxy_set_header X-Real-IP $remote_addr;
				proxy_set_header X-Forvarder-for $remote_addr;
				proxy_set_header X-Colibri-Version "1.1.1";
				proxy_hide_header X-AspNet-Version;
				client_max_body_size 0m;
		}
```

## Перезагрузка конфигурационного файла  

```
nginx -s reload
```

## Установка Сertbot (для выпуска сетефикатов)

  - Automatically enable HTTPS on your website with EFF's Certbot, deploying Let's Encrypt certificates
  - https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx

### Установка

```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python-certbot-nginx
```

### Начало настройки

```
sudo certbot --nginx
```

### Автоматическое продление

```
sudo certbot renew --dry-run
```

### Продление сетефикатов

```
certbot -q renew
```

### Тест настроек nginx

```
service nginx configtest
```
