---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7014549387"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014549387"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $transformers / DictValue (JSON) Parser"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# DictValue (JSON) Parser

> [!info] Раздел: System.Collections.Hashtable[@{Id=7014549387; ReuseId=; Title=DictValue (JSON) Parser; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / DictValue (JSON) Parser; Segments=System.Object[]; Index=631}.Id])
> @{Id=7014549387; ReuseId=; Title=DictValue (JSON) Parser; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / DictValue (JSON) Parser; Segments=System.Object[]; Index=631}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014549387)

---

Структура трансформера:

```
$transformers:
    <Имя трансформера>
        $template: <Имя плагина>
        $prefix: <Cтрока ключей, разделяется точкой>
        $ignorecase: <Игнорировать ли регистр в полях>
        $schema: <Схема получаемых данных>
            <Поле>:
                type: <Тип данных>
                attribute: <Значение>
```

Имя трансформера, имя плагина и схема данных являются обязательными полями.

## Пример

```
$transformers:
    FindOneParser:
        $template: json_find_one_parser
        $prefix: test_prefix
        $ignorecase: true
        $schema:
            Field_one:
                type: String
                attribute: field_one
            Field_two:
                type: String
                attribute: field_two
```

## Пример

```
$transformers:
    FinditerParser:
           $template: json_finditer_parser
           $prefix: test_prefix
           $ignorecase: true
           $schema:
               Field_one:
                   type: String
                   attribute: field_one
               Field_two:
                   type: String
                   attribute: field_two
```
