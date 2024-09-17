---
title: "Cервер SFTP"
permalink: /docs/ubuntu/sftp-server/
toc: true
---

# Установка SFTP сервера

Установка командной оболочки и текстового редактора
```
sudo apt install mc nano
```

Открыть конфигурацию `/etc/ssh/sshd_config`

```
sudo nano /etc/ssh/sshd_config
```

Добавьте строки
```
# Доступ SFTP
Match User fuser
ForceCommand internal-sftp
PasswordAuthentication no
ChrootDirectory /var/sftp
PermitTunnel no
AllowAgentForwarding no
AllowTcpForwarding no
X11Forwarding no
```
где
  `Match User fuser` - указывает на то, что все
  последующие строчки будут применены только при подключении пользователя fuser.
  `ForceCommand internal-sftp` — подключать пользователя только в режиме SFTP и не разрешать доступ в shell.
  `PasswordAuthentication no` — отключить доступ по логину и паролю.
  `ChrootDirectory /var/sftp` — ограничить доступ пользователя только в рамках папки /var/sftp.
  `PermitTunnel no, AllowAgentForwarding no, AllowTcpForwarding no, X11Forwarding no` - отключить туннелирование, проброс портов и графических приложений через SSH-сессию.

Проверка
```
cat /etc/ssh/sshd_config | grep -v -e '^#' -e '^$'
```

Рестарт
```
sudo systemctl restart sshd
```

Группа ftpusers
```
sudo groupadd ftpusers
```

Папка для пользователей
```
sudo mkdir -p /var/sftp
```

Разрешения на папку
```
sudo chown root:ftpusers /var/sftp
sudo chmod 770 /var/sftp
```

Проверка разрешений
```
ls -la /var | grep sftp
ls -la /var/sftp
```

## Создание SFTP пользователя

Новый пользователь
```
sudo useradd fuser -m
```
где `m` - создание домашней папки

```
sudo passwd fuser
```

Генерация ключей
```
sudo runuser -l  fuser -c 'ssh-keygen'
```
sudo runuser -l octonica -c 'ssh-keygen'
где
  `Enter file in which to save the key (/home/fuser/.ssh/id_rsa)` - /tmp/fuser_key
  `Enter passphrase (empty for no passphrase):` - отсавить пустым

Раздача прав
```
sudo touch /home/fuser/.ssh/authorized_keys
sudo chmod 600 /home/fuser/.ssh/authorized_keys
sudo chown fuser:fuser /home/fuser/.ssh/authorized_keys
```
