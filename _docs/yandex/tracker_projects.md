---
title: "Создание проектов"
permalink: /docs/yandex/tracker_projects/
toc: true
---

Раздел содержит недокументированные возможности
[REST API Яндекс.Трекера](https://yandex.ru/dev/connect/tracker/api/about.html) (на момент написания).

Создание проекта используя HTTP-запрос с методом POST.
Параметры тела в формате JSON.
```
POST /v2/projects?=null HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-ID: <orgid>
Content-Type: application/json

{
"name": "<name>",
"queues": "<queues>"
}
```
где
- `<token>` - ваш токен
- `<orgid>` - номер организации
- `<name>` - название проекта
- `<queues>` - название очереди
