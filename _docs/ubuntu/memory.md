---
title: "memory"
permalink: /docs/ubuntu/memory/
toc: true
---

# Память (memory)

## Использование оперативной памяти

### В реальном режиме времени
```
watch -n 1 free -m
```

### В реальном времени процессом
top U postgres

### На момент запуска
```
free -m
```
или
```
cat /proc/meminfo
```

## Использование диска
```
df -h
```

## Статистика по диску
```
vmstat -s
```

## высвободить память
```
sudo sysctl -w vm.drop_caches=3
sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches
```


----
Ссылки https://ru.wikipedia.org/wiki/Top
