---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7014548235"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014548235"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $transformers / KeyValue Parsers"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# KeyValue Parsers

> [!info] Раздел: System.Collections.Hashtable[@{Id=7014548235; ReuseId=; Title=KeyValue Parsers; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / KeyValue Parsers; Segments=System.Object[]; Index=632}.Id])
> @{Id=7014548235; ReuseId=; Title=KeyValue Parsers; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / KeyValue Parsers; Segments=System.Object[]; Index=632}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014548235)

---

Структура трансформера:

```
$transformers:
    <Имя трансформера>:
           $template: <Имя плагина>
           $separator: <Разделительный символ>
           $strip_chars: <Символ для удаления>
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
    KeyValueParser:
           $template: key_value_parser
           $separator: test_separator
           $strip_chars: test_strip_chars
           $skip_empty: true
           $ignorecase: true
           $schema:
               Field_one:
                   type: String
                   attribute: STRING_FIELD
               Field_two:
                   type: Int
                   attribute: INT_FIELD
               Field_three:
                   type: Array(Int)
                   attribute: ARRAY_INT_FIELD
```
