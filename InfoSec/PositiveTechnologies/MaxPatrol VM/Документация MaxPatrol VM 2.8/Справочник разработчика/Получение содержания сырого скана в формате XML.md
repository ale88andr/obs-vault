---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "4402265355"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/4402265355"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Виды запросов к API / Работа со сканами / Получение содержания сырого скана в формате XML"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Получение содержания сырого скана в формате XML

> [!info] Раздел: System.Collections.Hashtable[@{Id=4402265355; ReuseId=; Title=Получение содержания сырого скана в формате XML; Depth=4; Path=Справочник разработчика / Виды запросов к API / Работа со сканами / Получение содержания сырого скана в формате XML; Segments=System.Object[]; Index=669}.Id])
> @{Id=4402265355; ReuseId=; Title=Получение содержания сырого скана в формате XML; Depth=4; Path=Справочник разработчика / Виды запросов к API / Работа со сканами / Получение содержания сырого скана в формате XML; Segments=System.Object[]; Index=669}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/4402265355)

---

Запрос для получения содержания сырого скана в формате XML.

Для выполнения запроса требуется аутентификация по протоколу OAuth с токеном доступа типа Bearer.

Метод и URL запроса:

```
GET <Корневой URL API>/api/v1/scans/raw/{scanId}/content
```

URL запроса содержит path-параметр `scanId` — идентификатор скана.

## Ответ на запрос

В ответ на успешный запрос сервис возвращает код 200 (OK).

Возможные коды ошибок и их значения:

- 204 (No Content) — скан не найден;
- 400 (Badly formatted request) — синтаксическая ошибка в запросе.
