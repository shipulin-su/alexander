# firewall

[Инструкция help.ubuntu.ru](https://help.ubuntu.ru/wiki/%D1%80%D1%83%D0%BA%D0%BE%D0%B2%D0%BE%D0%B4%D1%81%D1%82%D0%B2%D0%BE_%D0%BF%D0%BE_ubuntu_server/%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D1%8C/firewall)

## Включаем ufw
```
sudo ufw enable
```

## Настраиваем

### Открываем порт 22 обязательно!!!
Иначе прострелим себе ногу, не будет доступа к машине
```
sudo ufw allow 22
```

### Открываем порт 5432 для PostgreSQL

```
sudo ufw allow 5432
```
