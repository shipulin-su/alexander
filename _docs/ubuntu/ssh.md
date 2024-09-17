---
title: "SSH"
permalink: /docs/ubuntu/ssh/
toc: true
---

# Использование SSH

Безопасный способ подключения.
Рассмотрим на примере пользователя **testuser**.

## Создание пары ключей SSH

Сгенерируем ключ:
```
ssh-keygen -t rsa -b 2048
```

После выполнения команды нужно задать:
- Имя ключа - testuser;
- Пароль.

Пар ключей сохраниться в `~/.ssh`:
- testuser - закрытый ключ. Используется для подключения
- testuser.pub - открытый ключ. Должен находиться на машине куда подключаемся

Прочитаем открытый ключ:
```
cat ~/.ssh/id_rsa/testuser.pub
```
Для копирования на другую машину, куда планируем подкачаться.

Прочитаем закрытый ключ:
```
cat ~/.ssh/id_rsa/testuser
```
Если нужно сохранить закрытый ключ в другом месте.

## Добавление SSH ключа пользователю

Создадим пользователя:
```
sudo useradd -m -d /home/testuser -s /bin/bash testuser
```

Переключитесь на него:
```
sudo su - testuser
```

Создайте папку `.ssh`:
```
mkdir .ssh
```

Создадим в `.ssh` файл `authorized_keys`:
```
touch .ssh/authorized_keys
```

Запишем в файл `authorized_keys` публичный ключ:
```
echo "<публичный_ключ>" > /home/testuser/.ssh/authorized_keys
```

Измените права доступа к файлу `authorized_keys` и каталогу `.ssh`:
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
Если не сделать, то будет предупреждении что ключ храниться не безопасно

Если пользователя нужно удалить, то выполнить:
```
sudo deluser --remove-home testuser
```

## Подключение по SSH

В терминале выполните команду:
```
ssh <имя_пользователя>@<имя_машины> -i ~/.ssh/testuser
```

При первом подключении к машине появится предупреждение о неизвестном хосте
```
The authenticity of host '.....' can't be established.
ECDSA key fingerprint is SHA256:.......
Are you sure you want to continue connecting (yes/no)?
```
Нужно ввести `yes` и нажать `Enter`
Ключ сохраняется в файл `~/.ssh/known_hosts`.

где `~/.ssh/testuser` - путь до закрытого ключа

Если что то пошло не так и нужно понять причину, то выполнить
```
ssh -Tvvv <имя_пользователя>@<имя_машины> -i ~/.ssh/testuser
```
