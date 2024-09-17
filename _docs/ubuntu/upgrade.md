---
title: "upgrade"
permalink: /docs/ubuntu/upgrade/
toc: true
---

# Обновление пакетов и обновление Ubuntu до новой версии

## Ссылки

- [Релиз Ubuntu](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes?)
- [Загрузка Ubuntu](https://ubuntu.com/download/server)
- [ru.wikipedia.org/wiki/Ubuntu](https://ru.wikipedia.org/wiki/Ubuntu)

## Обновление пакетов

```
sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove
```
apt update - обновление установленных пакетов
apt dist-upgrade - обновление и удаление не старых пакетов
autoremove - автоматическое удаление пакетов не используемых пакетов

## Обновление ядра

[Upgrades](https://help.ubuntu.com/community/Upgrades)

```
sudo apt install update-manager-core
sudo do-release-upgrade
```

## Версия ubuntu

[Checking your Ubuntu Version](https://help.ubuntu.com/community/CheckingYourUbuntuVersion)

```
lsb_release -a
```
