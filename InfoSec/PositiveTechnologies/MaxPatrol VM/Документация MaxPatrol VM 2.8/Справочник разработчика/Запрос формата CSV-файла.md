---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "2699912715"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2699912715"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Виды запросов к API / Импорт активов из CSV / Запрос формата CSV-файла"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Запрос формата CSV-файла

> [!info] Раздел: System.Collections.Hashtable[@{Id=2699912715; ReuseId=; Title=Запрос формата CSV-файла; Depth=4; Path=Справочник разработчика / Виды запросов к API / Импорт активов из CSV / Запрос формата CSV-файла; Segments=System.Object[]; Index=646}.Id])
> @{Id=2699912715; ReuseId=; Title=Запрос формата CSV-файла; Depth=4; Path=Справочник разработчика / Виды запросов к API / Импорт активов из CSV / Запрос формата CSV-файла; Segments=System.Object[]; Index=646}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2699912715)

---

Запрос для получения формата CSV-файла.

Для выполнения запроса требуется аутентификация по протоколу OAuth с токеном доступа типа Bearer.

Метод и URL запроса:

```
GET <Корневой URL API>/api/assets_processing/v2/csv/example
```

Параметры в теле запроса отсутствуют.

## Ответ на запрос

В ответ на успешный запрос сервис возвращает код 200 (OK). Тело ответа содержит пример содержимого CSV-файла с данными актива.

Возможные коды ошибок и их значения:

- 400 (Bad Request) — синтаксическая ошибка в запросе;
- 401 (Unauthorized) — ошибка аутентификации.

## Пример

Запрос:

```
GET https://localhost/api/assets_processing/v2/csv/example
```

Ответ:

```
"typealias";"Fqdn";"Hostname";"Ip";"Mac";"IsVirtual"
"";"dns.somedomain.ru";"w2k3sp2x86s5";"192.168.0.4|182.168.10.1";"00:50:56:A6:0C:36|00:60:56:A6:0C:36";"false"
```
