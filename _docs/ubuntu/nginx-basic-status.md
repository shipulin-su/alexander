# Nginx - basic_status

Отображение страницы статистики
Используется для мониторинга в zabbix.

```
server {

        listen 80;
        server_name _;

        location /basic_status {
            stub_status;
            allow 10.128.0.0/24;
            allow 10.129.0.0/24;
            allow 127.0.0.1;
            deny all;
          }
}
```
