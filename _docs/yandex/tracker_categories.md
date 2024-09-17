---
title: "Создание категории"
permalink: /docs/yandex/tracker_categories/
toc: true
---

Раздел содержит недокументированные возможности
[REST API Яндекс.Трекера](https://yandex.ru/dev/connect/tracker/api/about.html) (на момент написания).

Категории позволяю группировать поля в задаче.
Возможности создать категории из интерфейса нет
а так-же нет в описании API.

Важно! Категорию переименовать нельзя.

Создание категории используя HTTP-запрос с методом POST.
Параметры тела в формате JSON.
```
POST /v2/fields/categories?expand=fields HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-ID: <orgid>
Content-Type: application/json

{
    "name": {
        "en": "<en>",
        "ru": "<ru>"
    }
}
```
где
- `<token>` - ваш токен
- `<orgid>` - номер организации
- `<en>` - название поля на английском
- `<ru>` - название поля на русском


## Получить список полей
```
GET /v2/fields/ HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-Id: <orgid>
```

## Получить поле
```
GET /v2/fields/<fieldsid> HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-Id: <orgid>
If-Match: "<version>"
Content-Type: application/json
```
где
- `<fieldsid>` - id поля
