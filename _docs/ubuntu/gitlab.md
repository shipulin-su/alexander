---
title: "GitLab CE"
permalink: /docs/ubuntu/gitlab/
toc: true
---

# Обновление GitLab CE

Инструкция написана по мотивам документации
https://docs.gitlab.com/ee/update/package/

Обновимся все пакеты и почистим

```
sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove
```

Установим пакет с репозиторием

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

Обнови gitlab

```
sudo apt update && sudo apt install gitlab-ce
```

Получим список пакетов

```
sudo apt-cache madison gitlab-ce
```

Выберим на какой можем перейтий согласно документации
https://docs.gitlab.com/ee/update/index.html#upgrade-paths

Важно! Следуя указаним очередности возникали ошибки, поэтому лучше не спешить и обновлять по одной версии.

```
13.12.1 -> 13.12.15 -> 14.0.12 -> 14.3.6 (upd pg v 13) -> 14.9.5 -> 14.10.Z -> 15.0.Z -> latest 15.Y.Z
```

Установим обновление

```
sudo apt install gitlab-ce=<version>
```

Пример

```
sudo apt install gitlab-ce=13.12.15-ce.0
sudo apt install gitlab-ce=14.0.12-ce.0
sudo apt install gitlab-ce=14.1.0-ce.0
sudo apt install gitlab-ce=14.3.6-ce.0 (поставился после установки pg v 13)
sudo apt install gitlab-ce=14.9.5-ce.0 - ошибка
sudo apt install gitlab-ce=14.4.0-ce.0
sudo apt install gitlab-ce=14.5.0-ce.0
sudo apt install gitlab-ce=14.6.7-ce.0
sudo apt install gitlab-ce=14.7.7-ce.0
sudo apt install gitlab-ce=14.8.6-ce.0
sudo apt install gitlab-ce=14.9.5-ce.0
sudo apt install gitlab-ce=14.10.5-ce.0
sudo apt install gitlab-ce=15.0.5-ce.0
sudo apt install gitlab-ce=15.1.5-ce.0
sudo apt install gitlab-ce=15.2.3-ce.0
sudo apt install gitlab-ce=15.3.1-ce.0
```

## Полезные команды

Переконфигурировать gitlab

```
sudo gitlab-ctl reconfigure
```

Размер базы pg

```
sudo du -sh /var/opt/gitlab/postgresql/data
```

Место на диске

```
sudo df -h
```

Обновление postgres до 13 если gitlab 14.3.6

```
sudo gitlab-ctl pg-upgrade -V 13
```

Удаление старой папки PG

```
sudo rm -rf /var/opt/gitlab/postgresql/data.12
sudo rm -f /var/opt/gitlab/postgresql-version.old
```
