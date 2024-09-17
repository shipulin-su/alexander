# remove apache2

Остановка службы
```
sudo service apache2 stop
```

Удаление пакета (или сразу следующий шаг)
```
sudo apt remove apache2
sudo apt remove zabbix-apache-conf
```

Удаление пакета с конфигурационными файлами
```
sudo apt-get purge apache2
sudo apt-get purge zabbix-apache-conf
sudo apt-get purge apache2-data
sudo apt-get purge libapache2-mod-php

sudo apt-get purge apache2-utils
sudo apt-get purge apache2.2-bin
```

Удаляем зависимости
```
 sudo apt-get autoremove
```

Проверка на наличие конфигурационных файлов или мануалов
Фалйов быть не должно, если есть то удаляем
```
whereis apache2
```

Удаление файлов
```
sudo rm -rf /etc/apache2
```

Проверка пакетов dpkg (файла быть не должно)
```
sudo dpkg -S /usr/bin/apache2
```
