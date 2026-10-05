---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7014551691"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014551691"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $transformers / Resolvers"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Resolvers

> [!info] Раздел: System.Collections.Hashtable[@{Id=7014551691; ReuseId=; Title=Resolvers; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / Resolvers; Segments=System.Object[]; Index=635}.Id])
> @{Id=7014551691; ReuseId=; Title=Resolvers; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / Resolvers; Segments=System.Object[]; Index=635}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7014551691)

---

Трансформер Resolvers позволяет отображать полученные значения параметров системы в нормализованном или измененном виде. Трансформер представляет собой расширенный словарь, ключом которого являются данные из экстракторов или других трансформеров.

Структура трансформера:

```
$transformers:
    <Имя трансформера>
        $template: <Имя плагина>
        $default: <Выходное значение, если не найден ключ>
        $undefined: <Выходное значение, если на вход получено значение null>
        $mapping: <Отображение параметров системы в параметрах источника данных>
            <Ключ>: <Значение>
            <Ключ>: <Значение>
```

Имя трансформера и имя плагина являются обязательными полями.

Особенности заполнения полей трансформера:

- Допустимые значения `$template`:

- `boolean_to_string_resolver`;
- `int_to_boolean_resolver`;
- `int_to_int_array_resolver`;
- `int_to_string_resolver`;
- `int_to_string_array_resolver`;
- `major_version_to_string_resolver`;
- `string_to_boolean_resolver`;
- `string_to_int_resolver`;
- `string_to_int_array_resolver`;
- `string_to_string_resolver`;
- `string_to_string_array_resolver`;
- `strip_string_to_string_resolver`.
