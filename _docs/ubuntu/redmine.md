---
title: "redmine"
permalink: /docs/ubuntu/redmine/
toc: true
---

# Redmine

## Настройка отправки уведомлений

В файле конфигурации
```
sudo nano /usr/share/redmine/config/configuration.yml
```

Добавить запись
```
  # ==== SMTP server Yandex
  #
      delivery_method: :smtp
      smtp_settings:
        address: "[smtp.yandex.ru]"
        port: 25
        authentication: :login
        domain: "[domain]"
        user_name: "[user_name]"
        password: "[password]"
  #
```
