---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7014550539"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014550539"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $transformers / Search Parser"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Search Parser

> [!info] Раздел: System.Collections.Hashtable[@{Id=7014550539; ReuseId=; Title=Search Parser; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / Search Parser; Segments=System.Object[]; Index=633}.Id])
> @{Id=7014550539; ReuseId=; Title=Search Parser; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / Search Parser; Segments=System.Object[]; Index=633}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014550539)

---

Для передачи данных в секцию `$loaders` из трансформера Search Parser используются функции.

Трансформер Search Parser ищет текст по заданному регулярному выражению, в котором можно определить именованные группы. В результате поиска в группы попадает текст, соответствующий описанному шаблону. Чтобы получить текст из именованной группы, необходимо применить функцию, которая извлекает значение только из этой группы.

Структура трансформера:

```
$transformers:
    <Имя трансформера>
        $template: <Имя плагина>
        $pattern: <Регулярное выражение>
        $schema: <Схема получаемых данных>
            <Поле>: <Тип данных>
     <Название функции>:
         $template: function
         $apply: <Имя трансформера>
         $output: <Поле регулярного выражения>
```

Имя трансформера, имя плагина и схема данных являются обязательными полями. Функции используются в секции `$loaders` для получения результата работы трансформера.

## Пример

```
$transformers:
    NameVersionParser:
        $template: regexp_search
        $pattern: '^.*(?P<Name>\S+)\s+(?P<Version>\d+\.\d+\.\d+).*$'
        $schema: 
            Name: String
            Version: String
     get_name:
         $template: function
         $apply: NameVersionParser
         $output: Name
     get_version:
         $template: function
         $apply: NameVersionParser
         $output: Version
```
