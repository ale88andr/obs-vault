---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7277468555"
reuse_id: "7296182411"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7277468555"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Виды запросов к API / Получение токена PDQL-запросов"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Получение токена PDQL-запросов

> [!info] Раздел: System.Collections.Hashtable[@{Id=7277468555; ReuseId=7296182411; Title=Получение токена PDQL-запросов; Depth=3; Path=Справочник разработчика / Виды запросов к API / Получение токена PDQL-запросов; Segments=System.Object[]; Index=644}.Id])
> @{Id=7277468555; ReuseId=7296182411; Title=Получение токена PDQL-запросов; Depth=3; Path=Справочник разработчика / Виды запросов к API / Получение токена PDQL-запросов; Segments=System.Object[]; Index=644}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7277468555)

---

Запрос для получения токена PDQL-запросов.

Для выполнения запроса требуется аутентификация по протоколу OAuth с токеном доступа типа Bearer.

Время действия токена 30 минут.

Метод и URL запроса:

```
POST <Корневой URL API>/api/assets_temporal_readmodel/v1/assets_grid
```

Тело запроса может содержать параметры, описанные в таблице ниже.

**Параметры в теле запроса /api/assets_temporal_readmodel/v1/assets_grid**

| Параметр | Обязательный | Тип данных | Описание |
| --- | --- | --- | --- |
| `previousToken` | Нет | String | Предыдущий PDQL-токен сеанса пользователя |
| `pdql` | Да | String | PDQL-запрос |
| `utcOffset` | Нет | String | Сдвиг временной зоны действия от UTC |
| `additionalFilterParameters` | Нет | Object | Дополнительные параметры фильтра |
| `selectedGroupIds` | Нет | Array | Идентификаторы выбранных групп активов |
| `includeNestedGroups` | Да | Bool | Включать ли вложенные группы |
| `executionTime` | Нет | String (date-time) | Время выполнения запроса |
| Дополнительные параметры фильтра |   |   |   |
| `groupIds` | Нет | Array \[String\] | Список групп, в которых будет происходить выборка |
| `assetIds` | Нет | Array | Список активов, по которым будет выполняться запрос |

## Ответ на запрос

В ответ на успешный запрос сервис возвращает код 200 (OK). Ответ может содержать поля, описанные в таблице ниже.

**Поля ответа на запрос /api/assets_temporal_readmodel/v1/assets_grid**

| Поле | Тип данных | Описание |
| --- | --- | --- |
| `token` | String | PDQL-токен для запроса данных |
| `isPotentiallySlow` | Bool | Является ли запрос потенциально медленным |
| `hasTimepointPipe` | Bool | Есть ли в запросе `timepoint` |
| `hasTimeseriesPipe` | Bool | Есть ли в запросе `timeseries` |
| `fields` | Array \[Object\] | Описание полей |
| Описание полей |   |   |
| `name` | String | Имя поля |
| `localizedName` | String | Локализованное имя поля |
| `type` | String (Enum) | Тип поля |
| `isArray` | Bool | Является ли коллекцией |
| `origin` | String (Enum) | Примененная агрегация |

Возможные коды ошибок и их значения:

- 400 (Bad Request) — синтаксическая ошибка в запросе;
- 500 (Internal Server Error) — внутренняя ошибка сервера;
- 503 (Service Unavailable) — ошибка доступа.
