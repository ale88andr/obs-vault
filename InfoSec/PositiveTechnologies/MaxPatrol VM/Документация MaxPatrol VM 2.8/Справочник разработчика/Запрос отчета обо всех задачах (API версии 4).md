---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "8662329099"
reuse_id: "8863772811"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8662329099"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Виды запросов к API / Управление задачами сканирования / Запрос отчета обо всех задачах (API версии 4)"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Запрос отчета обо всех задачах (API версии 4)

> [!info] Раздел: System.Collections.Hashtable[@{Id=8662329099; ReuseId=8863772811; Title=Запрос отчета обо всех задачах (API версии 4); Depth=4; Path=Справочник разработчика / Виды запросов к API / Управление задачами сканирования / Запрос отчета обо всех задачах (API версии 4); Segments=System.Object[]; Index=683}.Id])
> @{Id=8662329099; ReuseId=8863772811; Title=Запрос отчета обо всех задачах (API версии 4); Depth=4; Path=Справочник разработчика / Виды запросов к API / Управление задачами сканирования / Запрос отчета обо всех задачах (API версии 4); Segments=System.Object[]; Index=683}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8662329099)

---

Запрос для получения отчета обо всех задачах с помощью API версии 4.

Для выполнения запроса требуется аутентификация по протоколу OAuth с токеном доступа типа Bearer.

Метод и URL запроса:

```
POST <Корневой URL API>/api/scanning/v4/scanner_tasks
```

Параметры строки запроса описаны в таблице ниже.

**Параметры строки запроса /api/scanning/v4/scanner_tasks**

| Параметр | Обязательный | Тип данных | Описание |
| --- | --- | --- | --- |
| `mainFilter` | Нет | String | Основной фильтр |
| `additionalFilter` | Нет | String | Дополнительный фильтр. Значение по умолчанию: `all` |
| `orderBy` | Нет | String | Сортировка в списке |
| `orderDirection` | Нет | String | Порядок сортировки |
| `validate` | Нет | Bool | Валидировать ли задачи. Значение по умолчанию: `true` |
| `token` | Нет | String | Токен выборки. Возвращает задачи из кэша. Если параметр не задан, задачи возвращаются из базы данных |
| `offset` | Нет | Integer | Смещение от начала выборки. Значение по умолчанию: `0` |
| `limit` | Нет | Integer | Количество запрашиваемых элементов выборки. Значение по умолчанию: `1000` |

Тело запроса может содержать параметры, описанные в таблице ниже.

**Параметры в теле запроса /api/scanning/v4/scanner_tasks**

| Параметр | Обязательный | Тип данных | Описание |
| --- | --- | --- | --- |
| `text` | Нет | String | Текст |
| `agents` | Нет | Array | Коллекторы |
| `agents` → `agentIds` | Да | Array\[String\] | Идентификаторы коллекторов |
| `agents` → `autoSelect` | Да | Bool | Включен ли автовыбор |
| `modules` | Нет | Array\[String\] | Модули |
| `profiles` | Нет | Array\[String\] | Профили |
| `statuses` | Нет | String | Статус задачи сканирования: - `new` — не запускалась; - `preparing` — подготавливается; - `waiting` — ожидает выполнения; - `running` — выполняется; - `finishing` — завершается; - `finished` — завершена; - `suspending` — приостанавливается; - `suspended` — приостановлена |
| `credentials` | Нет | Array\[String\] | Учетные записи |
| `target` | Нет | Object | Информация об атакованных активах |
| `target` → `target` | Да | Array | Атакованные активы |
| `target` → `target` → `assets` | Да | Array | Активы: - `id` — идентификаторы активов, тип String; - `name` — названия активов, тип String |
| `target` → `target` → `targets` | Да | Array\[String\] | IP-адреса, FQDN или маски подсетей конкретных сетевых адресов |
| `target` → `target` → `assetsGroups` | Да | Array | Группы активов: - `id` — идентификаторы групп активов, тип String; - `name` — названия групп активов, тип String |
| `target` → `type` | Да | String | Тип атакованного актива |
| `scopes` | Нет | Array\[String\] | Задача по инфраструктуре |
| `groups` | Да | Array\[String\] | Задача в группе |

## Ответ на запрос

В ответ на успешный запрос сервис возвращает код 200 (OK). Ответ может содержать поля, описанные в таблице ниже.

**Поля ответа на запрос /api/scanning/v4/scanner_tasks**

| Поле | Тип данных | Описание |
| --- | --- | --- |
| `id` | String | Идентификатор задачи |
| `name` | String | Название задачи |
| `description` | String | Описание задачи |
| `agents` | Object | Коллекторы и их компоненты, которые выбираются для выполнения задачи (расширенный вариант) |
| `agents` → `agents` | Array | Информация о коллекторах |
| `agents` → `agents` → `id` | String | Идентификаторы коллекторов |
| `agents` → `agents` → `name` | String | Названия коллекторов |
| `agents` → `components` | Object | Принадлежность заданных для выполнения задачи коллекторов к конкретному конвейеру или ядру |
| `agents` → `components` → `siemIds` | Array\[String\] | Конвейеры, коллекторы которых можно использовать для выполнения подзадачи |
| `agents` → `components` → `useCoreAgents` | Bool | Возможно ли использовать коллекторы ядра для выполнения подзадачи |
| `scope` | Object | Задача по инфраструктуре |
| `scope` → `id` | String | Идентификатор задачи |
| `scope` → `name` | String | Название задачи |
| `profile` | Object | Профиль сканирования |
| `profile` → `id` | String | Идентификатор профиля |
| `profile` → `name` | String | Название профиля |
| `module` | Object | Модуль |
| `module` → `id` | String | Идентификатор модуля |
| `module` → `name` | String | Название модуля |
| `metatransports` | Array | Метатранспорты и их параметры |
| `status` | String (Enum) | Статус задачи сканирования: - `new` — не запускалась; - `preparing` — подготавливается; - `waiting` — ожидает выполнения; - `running` — выполняется; - `finishing` — завершается; - `finished` — завершена; - `suspending` — приостанавливается; - `suspended` — приостановлена; - `imported` — импортирована |
| `created` | String (Date-time) | Дата создания задачи в формате ISO 8601 |
| `lastRun` | String (Date-time) | Дата последнего запуска в формате ISO 8601 |
| `nextRun` | String (Date-time) | Дата следующего запуска в формате ISO 8601 |
| `lastRunErrorLevel` | String (Enum) | Уровень ошибки последнего запуска: - `green` — все подзадачи завершены без ошибок; - `yellow` — часть подзадач завершена с ошибками; - `red` — все подзадачи завершены с ошибками, задача не может быть выполнена |
| `include` | Object | Признак включения целей сбора данных |
| `include` → `assets` | Array | Активы |
| `include` → `assets` → `id` | String | Идентификатор актива |
| `include` → `assets` → `name` | String | Название актива |
| `include` → `targets` | Array\[String\] | IP-адреса, FQDN или маски подсетей конкретных сетевых адресов |
| `include` → `assetsGroups` | Array | Группы активов |
| `include` → `assetsGroups` → `id` | String | Идентификатор группы активов |
| `include` → `assetsGroups` → `name` | String | Название группы активов |
| `exclude` | Object | Признак исключения целей сбора данных |
| `exclude` → `assets` | Array | Активы |
| `exclude` → `assets` → `id` | String | Идентификатор актива |
| `exclude` → `assets` → `name` | String | Название актива |
| `exclude` → `targets` | Array\[String\] | IP-адреса, FQDN или маски подсетей конкретных сетевых адресов |
| `exclude` → `assetsGroups` | Array | Группы активов |
| `exclude` → `assetsGroups` → `id` | String | Идентификатор группы активов |
| `exclude` → `assetsGroups` → `name` | String | Название группы активов |
| `isFqdnPriority` | Bool | Является ли сканирование активов по FQDN более приоритетным, чем сканирование по IP-адресу |
| `validationState` | String | Статус корректности данных, указанных в задаче: - `valid` — все указанные данные корректны, задача может быть выполнена; - `invalid` — указанные данные некорректны, задача не может быть выполнена |
| `lastRunError` | Object | Последняя ошибка запуска |
| `lastRunError` → `type` | String | Тип ошибки |
| `lastRunError` → `category` | String | Категория ошибки: - `target` — цель сканирования; - `scope` — задача по инфраструктуре; - `profile` — профиль; - `task` — задача; - `agent` — коллектор; - `migration` — миграция |
| `hostDiscovery` | Array | Признак обнаружения узлов до начала сбора данных |
| `hostDiscovery` → `enabled` | Bool | Признак сканирования только отвечающих узлов |
| `hostDiscovery` → `profile` | Object | Профиль, используемый при сканировании |
| `hostDiscovery` → `profile` → `id` | String | Идентификатор профиля |
| `hostDiscovery` → `profile` → `name` | String | Название профиля |
| `hasBookmarks` | Bool | Сохранено ли состояние источника |
| `credentials` | Array | Учетные записи |
| `credentials` → `id` | String | Идентификатор учетной записи |
| `credentials` → `name` | String | Название учетной записи |
| `triggerParameters` | Array | Параметры срабатывания по таймеру |
| `triggerParameters` → `type` | String | Тип |
| `triggerParameters` → `fromDate` | String (Date-time) | Дата начала срабатывания |
| `triggerParameters` → `toDate` | String (Date-time) | Дата окончания срабатывания |
| `triggerParameters` → `isEnabled` | Bool | Признак того, что разрешено срабатывание по таймеру |
| `token` | String | Токен |
| `totalCount` | Integer | Общее количество |

Возможные коды ошибок и их значения:

- 400 (Bad request) — синтаксическая ошибка в запросе. Ответ может содержать поля, описанные в таблице ниже.

**Поля ответа на запрос /api/scanning/v4/scanner_tasks**

| Поле | Тип данных | Описание |
| --- | --- | --- |
| `errors` | Array | Ошибки |
| `errors` → `error` | Object | Ошибка |
| `errors` → `error` → `type` | String | Тип ошибки |
| `errors` → `error` → `category` | String | Категория ошибки: - `target` — цель сканирования; - `scope` — задача по инфраструктуре; - `profile` — профиль; - `task` — задача; - `agent` — коллектор; - `migration` — миграция |
| `errors` → `source` | Object | Источник ошибки |
| `errors` → `source` → `displayName` | String | Отображаемое имя |
| `errors` → `source` → `hostName` | String | Имя узла |
| `errors` → `source` → `ipAddresses` | Array\[String\] | IP-адреса |
| `errors` → `sensitive` | Bool | Признак значимости ошибки |
| `message` | String | Сообщение |
| `code` | Integer | Код ошибки |
