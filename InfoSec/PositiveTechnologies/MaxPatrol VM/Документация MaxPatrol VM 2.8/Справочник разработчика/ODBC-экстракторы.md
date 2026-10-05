---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "6995496843"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6995496843"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $extractors / ODBC-экстракторы"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# ODBC-экстракторы

> [!info] Раздел: System.Collections.Hashtable[@{Id=6995496843; ReuseId=; Title=ODBC-экстракторы; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / ODBC-экстракторы; Segments=System.Object[]; Index=627}.Id])
> @{Id=6995496843; ReuseId=; Title=ODBC-экстракторы; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / ODBC-экстракторы; Segments=System.Object[]; Index=627}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6995496843)

---

ODBC-экстракторы позволяют делать SQL-запросы в СУБД.

## Экстрактор Select Extractor

Экстрактор Select Extractor должен иметь следующую структуру:

```
$extractors:
    <Имя экстрактора>:
        $template: <Имя плагина>
        $table: <Имя таблицы, из которой будут собираться данные>
        $mapping: <Отображение параметров системы в параметрах источника данных>
            <Параметр источника данных>: <Поле в $scheme>
        $schema: <Схема получаемых данных>
            <Поле>: <Тип данных>
```

Имена экстрактора, плагина, таблицы, а также схема данных являются обязательными полями.

## Пример

```
$extractors:
    MSSQLSelect:
        $template: mssql_select
        $table: ?db_name.system_table
        $mapping:
            table_catalog: TableCatalog
            table_schema: TableSchema
            table_name: TableName
        $schema:
            TableCatalog: String
            TableSchema: String
            TableName: String
```

## Экстрактор Query Extractor

Экстрактор Query Extractor должен иметь следующую структуру:

```
$extractors:
    <Имя экстратора>
        $template: <Имя плагина>
        $query: | <Запрос к СУБД, из которой будут собираться данные>
            USE [sys]
            SELECT table_catalog, table_schema, table_name
            FROM [information_schema].[?table_name]
        $mapping: <Отображение параметров системы в параметрах источника данных>
            <Параметр источника данных>: <Поле в $scheme>
        $schema: <Схема получаемых данных>
            <Поле>: <Тип данных>
```

## Пример

```
$extractors:
    MSSQLQuery:
        $template: mssql_query
        $query:
            USE [sys]
            SELECT table_catalog, table_schema, table_name
            FROM [information_schema].[?table_name]
        $mapping:
            table_catalog: TableCatalog
            table_schema: TableSchema
            table_name: TableName
        $schema:
            TableCatalog: String
            TableSchema: String
            TableName: String
```

Имя экстрактора, имя плагина, запрос к СУБД, а также схема данных являются обязательными полями.

Особенности заполнения полей ODBC-экстракторов:

- Допустимые значения `$template`:

- `mssql_select`;
- `mssql_query`;
- `mysql_select`;
- `mysql_query`;
- `oracle_select`;
- `oracle_query`;
- `postgresql_select`;
- `postgresql_query`.
