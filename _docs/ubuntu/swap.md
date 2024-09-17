---
title: "swap"
permalink: /docs/ubuntu/swap/
toc: true
---

# Подкачка swapiness

Свободная память
free --giga --total --human

## Использование файла подкачки swappiness

Значение swapiness определяет перемещение процесов в файл подкачки

где  0 - отключает swapiness;
    10 - рекомендуемое значение для систем с большой памятью;
    60 - по умолчанию;

### Текущее значение swapiness
cat /proc/sys/vm/swappiness

### Установка swapiness
```
sudo sysctl -w vm.swappiness=10
sudo sysctl -f
```

### Очистка
```
sudo swapoff -a
sudo swapon -a
```
