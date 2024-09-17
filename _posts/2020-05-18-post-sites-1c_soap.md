---
title: "Публикация: Получение данных в Power BI из 1С через канал OData"
date: 2020-04-21
toc: true
categories:
  - Публикация
tags:
  - 1С
  - SOAP
  - Power BI
---

## Цель
Получить на прямую данных в Power BI из 1С через канал OData

## Установить IIS
- Общие функции HTTP (Common HTTP Features)
  - Статическое содержимое (Static Content)
  - Документ по умолчанию (Default Document)
  - Обзор каталогов (Directory Browsing)
  - Ошибки HTTP (HTTP Errors)
- Разработка приложений (Application Development)
  - ASP
  - ASP.NET 3.5
  - Расширяемость .NET 3.5 (.NET Extensibility 3.5)
  - Расширения ISAPI (ISAPI Extensions)
  - Фильтры ISAPI (ISAPI Filters)
- Исправление и диагностика (Health and Diagnostics)
  - Ведение журнала HTTP (HTTP Logging)
  - Монитор запросов (Request Monitor)
- Средства управления (Management Tools)
  - Консоль управления IIS (IIS Management Console)

## Установить 1С
- WEB расширениея

## Настроить IIS
- Пользователю IIS_User предоставить полный доступ:
  - wsisapi.dll в паке C:\Program Files\1cv8
  - C:\inetpub\wwwroot\
- Перейти в "сопоставление обработчиков"
  - выполнить "Добавление сопоставления сценария ..."
	- 1.  Путь запроса: `*.1crs`
		- исполняемый файл: `C:\Program Files\1cv8\8.3.13.1690\bin\wsisapi.dll`
		- Имя: `1C:Enterprise.1crs`
		- Ограничения запроса:  выполнение
	- 2.  Путь запроса: `*.1cws`
		- исполняемый файл: `C:\Program Files\1cv8\8.3.13.1690\bin\wsisapi.dll`
		- Имя: `1C:Enterprise.1cws`
		- Ограничения: выполнение
	- 3. Измениnm разрешение функции: + `выполнение``
- Проверить настройки `C:\inetpub\wwwroot\web.config`
```
<?xml version="1.0" encoding="UTF-8"?>
  <configuration>
    <system.webServer>
       <handlers accessPolicy="Read, Execute, Script">
	<add name="1C:Enterprise.1crs" path="*.1crs" verb="*" modules="IsapiModule" scriptProcessor="C:\Program Files\1cv8\[Версия]\bin\wsisapi.dll" resourceType="File" requireAccess="Execute" preCondition="bitness64" />
        <add name="1C:Enterprise.1cws" path="*.1cws" verb="*" modules="IsapiModule" scriptProcessor="C:\Program Files\1cv8\[Версия]\bin\wsisapi.dll" resourceType="File" requireAccess="Execute" preCondition="bitness64" />
       </handlers>
       <security>
	  <requestFiltering allowDoubleEscaping="true" />
	</security>
    </system.webServer>
	<system.web>
		<pages validateRequest="false" />
		<httpRuntime requestPathInvalidCharacters="" />
	</system.web>
</configuration>
```

## Опубликовать опубликовать информационную базу 1С
- 1С конфигуратор
  - Администрирование > Публикация на веб-сервере
    - Каталог  C:\inetpub\wwwroot\1c_[ИмяИнформационнойБазы]
    - Публиковать стандартный интерфейс OData
    - Использовать аутентификацию операционной системы
  - Проверить публикацию
    - На сервер зайти по адресу `http://127.0.0.1/1cv8_uat/odata/standard.odata/`
    - Указать пользователя и пароль
    - В результате должен отобразиться xml с перечислением всех объектов конфигурации

## Загрузит данные в Power Bi
- Подготовить путь
  - Из списка `http://127.0.0.1/1cv8_uat/odata/standard.odata/` выбрать объект
  - Посмотреть реквизиты объекта
```
= OData.Feed("http://127.0.0.1/1cv8_uat/odata/standard.odata/[Объект]?$top=10", null, [Implementation="2.0"])
```
   - Сформировать запрос. Пример для документа
```
= OData.Feed("http://127.0.0.1/1cv8_uat/odata/standard.odata/[Объект]?$filter=Posted eq true and Date ge datetime'2019-01-01T00:00:00'&$select=Ref_Key, Number, Date, [Реквизит1], [Реквизит2], [Реквизит2]", null, [Implementation="2.0"])
```
- Добавить источник данных ODATA
  - Вставить запрос
  - Выбрать windows авторизацию, указать пользователя и пароль
