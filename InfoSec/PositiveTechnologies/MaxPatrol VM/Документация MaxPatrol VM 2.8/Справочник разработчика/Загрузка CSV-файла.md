---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "2699077643"
reuse_id: "6910393355"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2699077643"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Виды запросов к API / Импорт активов из CSV / Загрузка CSV-файла"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Загрузка CSV-файла

> [!info] Раздел: System.Collections.Hashtable[@{Id=2699077643; ReuseId=6910393355; Title=Загрузка CSV-файла; Depth=4; Path=Справочник разработчика / Виды запросов к API / Импорт активов из CSV / Загрузка CSV-файла; Segments=System.Object[]; Index=647}.Id])
> @{Id=2699077643; ReuseId=6910393355; Title=Загрузка CSV-файла; Depth=4; Path=Справочник разработчика / Виды запросов к API / Импорт активов из CSV / Загрузка CSV-файла; Segments=System.Object[]; Index=647}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2699077643)

---

Запрос для загрузки CSV-файла с данными активов.

Для выполнения запроса требуется аутентификация по протоколу OAuth с токеном доступа типа Bearer.

Метод и URL запроса:

```
POST <Корневой URL API>/api/assets_processing/v2/csv/import_operation
```

Параметры строки запроса описаны в таблице ниже.

**Параметры строки запроса /api/assets_processing/v2/csv/import_operation**

| Параметр | Обязательный | Тип данных | Описание |
| --- | --- | --- | --- |
| `scopeId` | Да | UUID | Идентификатор инфраструктуры |

В запрос необходимо добавить заголовок `Content-Disposition` с описанием CSV-файла, а в тело запроса — CSV-файл с данными активов.

## Формат CSV-файла

Файл должен быть представлен в кодировке UTF-8 с BOM. Первая строка файла должна содержать названия полей актива. Вторая и последующие строки — значения полей импортируемых активов (одна строка соответствует одному активу). Значения полей должны быть разделены точкой с запятой. Значения текстовых полей должны быть заключены в кавычки.

**Колонки CSV-файла**

| Колонка | Тип данных | Описание |
| --- | --- | --- |
| `Fqdn` | String | Полное доменное имя |
| `Hostname` | String | Имя узла |
| `Ip` | Array \[String\] | IP-адрес. Поле может содержать несколько значений, которые должны быть разделены вертикальной чертой |
| `IsVirtual` | Bool | Является ли виртуальным |
| `Mac` | String \[String\] | MAC-адрес. Поле может содержать несколько значений, которые должны быть разделены вертикальной чертой |
| `typealias` | Enum | Тип актива: - Alcatel OmniSwitch (под управлением AOS) — `omniswitch`; - BSD, FreeBSD и macOS — `bsd`; - Check Point GAiA OS (межсетевой экран) — `checkpoint`; - Check Point SPLAT (межсетевой экран) — `checkpoint`; - Cisco ACS — `acs`; - Cisco ASA (межсетевой экран) — `asa`; - Cisco FWSM (межсетевой экран) — `fwsm`; - Cisco IOS — `ios`; - Cisco IOS XE — `ios`; - Cisco ISE — `ise`; - Cisco Nexus — `nexus`; - Cisco PIX (межсетевой экран) — `pix`; - FortiNet FortiGate (межсетевой экран) — `fortigate`; - HPE HP-UX — `hp_ux`; - Huawei VRP — `vrp`; - IBM AIX — `aix`; - Juniper JunOS — `junos`; - Linux (все семейство ОС) — `linux`; - Oracle Solaris — `solaris`; - Palo Alto Networks PAN-OS (межсетевые экраны) — `pan_os`; - VMware vSphere Hypervisor (ESXi) — `esxi`; - Windows — `windows` |
| `<Пользовательские поля>` | — | Добавленные пользователем поля актива, не являющиеся стандартными |

## Ответ на запрос

В ответ на успешный запрос сервис возвращает код 200 (OK). Ответ может содержать поля, описанные в таблице ниже.

**Поля ответа на запрос /api/assets_processing/v2/csv/import_operation**

| Поле | Тип данных | Описание |
| --- | --- | --- |
| `id` | UUID | Идентификатор операции импорта |
| `isLogFileCreated` | Bool | Создан ли файл с ошибками |
| `rowsCountExceeded` | Bool | Превышает ли количество строк в импортируемом файле установленное ограничение |
| `totalRowsCount` | Number | Общее количество строк в импортируемом файле |
| `validRowsCount` | Number | Количество строк с валидными данными активов в импортируемом файле |

Возможные коды ошибок и их значения:

- 400 (Bad Request) — синтаксическая ошибка в запросе;
- 401 (Unauthorized) — ошибка аутентификации.

## Пример

Запрос:

```
POST https://localhost/api/assets_processing/v2/csv/import_operation?scopeId=00000000-0000-0000-0000-000000000005
```

Заголовок запроса:

```
Content-Disposition:form-data; name="upfile"; filename="Assets_list.csv"
```

В тело запроса (в представлении `form-data`) необходимо добавить CSV-файл `Assets_list.csv` с ключом `upfile`.

Ответ:

```json
{
  "id": "0e372968-e18b-471e-b3b7-ad0d927c2dd9",
  "isLogFileCreated": true,
  "rowsCountExceeded": false,
  "validRowsCount": 2,
  "totalRowsCount": 3
}
```
