# Google Dorks — Шпаргалка

## 1. Базовые поисковые операторы

| Оператор             | Назначение                                                | Пример                                                   |
| -------------------- | --------------------------------------------------------- | -------------------------------------------------------- |
| `site:`              | Поиск только в пределах указанного домена/поддомена       | `site:example.com`                                       |
| `-site:`             | Исключить определённый домен/поддомен из поиска           | `site:*.example.com -site:www.example.com`               |
| `inurl:`             | Поиск по ключевому слову в URL-адресе                     | `inurl:admin site:example.com`                           |
| `intitle:`           | Поиск по ключевому слову в заголовке страницы (`<title>`) | `intitle:"index of" site:example.com`                    |
| `intext:`            | Поиск по ключевому слову в теле страницы                  | `intext:"password" site:example.com`                     |
| `filetype:` / `ext:` | Поиск по расширению файла                                 | `filetype:pdf site:example.com`                          |
| `OR`                 | Логическое «ИЛИ»                                          | `("backup" OR "bak") site:example.com`                   |
| `()`                 | Группировка условий                                       | `(confidential OR "internal use only") site:example.com` |
| `-`                  | Исключение слова/условия                                  | `site:example.com -inurl:blog`                           |

## 2. Разведка структуры сайта

|Цель|Дорк|Пояснение|
|---|---|---|
|**Поиск поддоменов**|`site:*.example.com -site:www.example.com`|Помогает найти дополнительные поддомены, проиндексированные Google.|
|**Списки директорий (Directory Listing)**|`intitle:"index of" site:example.com`|Находит открытые директории с включённым листингом файлов.|
|**Директории с бэкапами**|`intitle:"index of" ("backup" OR "bak" OR "old") site:example.com`|Часто в таких директориях лежат копии сайта, конфиги или дампы.|
|**Карты точек аутентификации**|`site:example.com inurl:login OR inurl:signin OR inurl:admin OR inurl:portal`|Поиск страниц входа и административных панелей.|

## 3. Поиск конфигураций, бэкапов и служебных файлов

|Цель|Дорк|Пояснение|
|---|---|---|
|**Файлы БД**|`site:example.com ext:sql OR ext:dbf OR ext:mdb`|Дамп SQL, Access и другие файлы баз данных.|
|**Конфигурационные файлы**|`site:example.com ext:env OR ext:ini OR ext:conf OR ext:cfg OR ext:yml`|Могут содержать креды, строки подключения, ключи.|
|**Логи, бэкапы, временные файлы**|`site:example.com ext:log OR ext:bak OR ext:old OR ext:swp OR ext:~`|Часто содержат отладочную информацию или старые настройки.|
|**Метаданные VCS**|`intitle:"index of" ".git" site:example.com`|Открытая папка `.git` может позволить восстановить исходный код проекта.|

## 4. Утечки чувствительных данных

|Цель|Дорк|Пояснение|
|---|---|---|
|**Таблицы с PII/Email**|`site:example.com ext:xls OR ext:xlsx OR ext:csv intext:email`|Excel/CSV файлы, нередко содержащие персональные данные.|
|**Секреты в .env**|`site:example.com ext:env intext:DB_PASSWORD`|Один из самых частых источников утечки БД-кредов.|
|**Приватные SSH/RSA ключи**|`intext:"-----BEGIN RSA PRIVATE KEY-----" site:example.com`|Крайне чувствительная утечка. Ключ должен быть немедленно отозван.|
|**AWS-ключи**|`site:example.com intext:"AKIA" ext:txt OR ext:log OR ext:env`|Поиск префикса AWS Access Key ID.|
|**API-ключи и секреты в JSON**|`filetype:json intext:"api_key" OR intext:"client_secret" site:example.com`|Распространено в экспортированных конфигах и бэкапах.|

## 5. Поиск админ-панелей и популярных фреймворков

|Цель|Дорк|Пояснение|
|---|---|---|
|**phpMyAdmin**|`intitle:"phpMyAdmin" "Welcome to phpMyAdmin" site:example.com`|Панель управления MySQL.|
|**Grafana**|`intitle:"Grafana" inurl:/login site:example.com`|Панель мониторинга и визуализации метрик.|
|**Jenkins**|`intitle:"Dashboard [Jenkins]" site:example.com`|CI/CD панель. Часто имеет доступ к скриптам и секретам.|
|**WordPress**|`inurl:"/wp-login.php" OR inurl:"/wp-admin" site:example.com`|Поиск точек входа в CMS WordPress.|

## 6. GitHub Dorking

|Цель|Дорк|Пояснение|
|---|---|---|
|**Поиск AWS-ключей в репозитории организации**|`org:exampleorg AWS_SECRET_ACCESS_KEY`|Классика для поиска утечек секретов.|
|**Поиск по имени файла**|`filename:.env DB_PASSWORD`|Конкретно ищет упоминания пароля в файле `.env`.|
|**Поиск по пути и языку**|`org:exampleorg path:**/config language:yaml "password:"`|Более точный поиск с фильтром по пути и языку.|
|**Поиск приватных ключей**|`org:exampleorg "-----BEGIN PRIVATE KEY-----"`|Поиск закоммиченных приватных ключей.|

## 7. Shodan

Shodan используется для поиска устройств и веб-сервисов, доступных в открытом доступе из интернета.

|Цель|Запрос|Пояснение|
|---|---|---|
|**Фильтр по хостнейму и заголовку**|`hostname:example.com http.title:"Login"`|Поиск хостов с доменом и определённым заголовком страницы.|
|**Поиск по TLS-сертификату**|`ssl.cert.subject.cn:example.com port:443 http.status:200`|Поиск по CN/SAN сертификата, часто помогает найти скрытые хосты.|
|**Фингерпринтинг по продукту**|`product:"Apache httpd" country:US http.title:"Index of"`|Поиск уязвимых/необычных конфигураций по ПО.|

## 8. Censys

Censys сфокусирован на поиске по сертификатам, хостам и сетевым сервисам с большим акцентом на TLS.

| Цель                              | Запрос                                                                                               | Пояснение                                                                                |
| --------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Поиск по домену в сертификате** | `services.tls.certificates.leaf_data.subject.common_name=example.com and services.service_name=HTTP` | Находит хосты, у которых в TLS-сертификате указан искомый домен и открыт HTTP(S)-сервис. |