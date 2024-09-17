---
title: "Публикация: Развёртывание сайта документации"
date: 2020-04-03
toc: true
categories:
  - Публикация
tags:
  - sites
  - mmistakes
  - Jekyll
---

Цель. Создать сайт содержащий разделом документы, публикации,
где редактирование страниц выполняется с использованием разметки markdown.

Для этого потребуется:
- регистрация на GitHub
- GitHub Desktop https://desktop.github.com/
- Atom https://atom.io/

Публикация написана по инструкции от разработчика
https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#remote-theme-method

## Создаем сайт из репозитория

Копируем стартовый репозиторий (без лишних модулей), для этого нужно перейти по ссылке
https://github.com/mmistakes/mm-github-pages-starter/generate
указать название нового репозитория и сохранить

## Настраиваем публикацию в pages

Переходим в настройки репозитория в раздел pages
Затем выбреем source и ветку master, после сохранения сайт будет доступен
по ссылке указанной на той же странице.

Важно! При желании можно настроить переход на сайт по доменному имени.

## Настраиваем сайт

Далее следуем инструкции
https://mmistakes.github.io/minimal-mistakes/docs/configuration/

### Устанавливаем locale

https://mmistakes.github.io/minimal-mistakes/docs/configuration/#site-locale

locale - это языковой стандарт
Правильное наименование locale можно узнать по ссылке https://docs.microsoft.com/en-us/previous-versions/commerce-server/ee825488(v=cs.20)?redirectedfrom=MSDN

1. Открываем `_config.yml`
2. Дописываем `locale: "ru-RU"`
3. В директорию `_data/ui-text.yml` сохраняем файл https://github.com/mmistakes/minimal-mistakes/blob/master/_data/ui-text.yml

### Задаем название сайта title

https://mmistakes.github.io/minimal-mistakes/docs/configuration/#site-title

Используется в теме, шапка сайта и теге `<title>`

```
title: "My Awesome Site"
```

### Определяем временная зону timezone

Список всех зон https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

```
timezone: Asia/Yekaterinburg
````

### Подключаем коллекцию docs

docs - это раздел сайта с документацией

1. Открываем `_config.yml`
2. Дописываем Collections
```
# Collections
collections:
  docs:
    output: true
    permalink: /:collection/:path/
```
3. Дописываем defaults
```
# Defaults
defaults:
  # _docs
  - scope:
      path: ""
      type: docs
    values:
      layout: single
      read_time: false
      author_profile: false
      share: false
      comments: false
      sidebar:
        nav: "docs"
```

### Настраиваем публикации

- при удалении `2010-09-09-post-gallery` удалить ссылку в `_docs\14-helpers.md`
- Кроме `_posts\9999-12-31-post-future-date.md`


### Настраиваем поиск google

https://cse.google.com/cse/all

получить код для сайта по ссылке `https://cse.google.com/cse.js?`
записать в `_config.yml` строку `search_engine_id : код`

Важно! Работает как то криво, обычно достаточно lunr

### Настраиваем поиск lunr

Для включения в `_config.yml` запишем:

```
search                   : true
search_full_content      : true
search_provider          : lunr
```

### Настраиваем аналитику

https://analytics.google.com/analytics/web/#/


### Пример кода страницы /еxample-content/
- /еxample-content/
