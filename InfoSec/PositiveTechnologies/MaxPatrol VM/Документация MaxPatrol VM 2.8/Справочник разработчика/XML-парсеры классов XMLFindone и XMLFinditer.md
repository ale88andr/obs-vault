---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7023417483"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7023417483"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $transformers / XML-парсеры классов XMLFindone и XMLFinditer"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# XML-парсеры классов XMLFindone и XMLFinditer

> [!info] Раздел: System.Collections.Hashtable[@{Id=7023417483; ReuseId=; Title=XML-парсеры классов XMLFindone и XMLFinditer; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / XML-парсеры классов XMLFindone и XMLFinditer; Segments=System.Object[]; Index=634}.Id])
> @{Id=7023417483; ReuseId=; Title=XML-парсеры классов XMLFindone и XMLFinditer; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $transformers / XML-парсеры классов XMLFindone и XMLFinditer; Segments=System.Object[]; Index=634}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7023417483)

---

Структура трансформера:

```
$transformers:
    <Имя пользовательского трансформера>
        $template: <Имя плагина>
        $schema: <Схема получаемых данных>
            <Поле>:
                data_type: <Тип данных>
                xml_function: <Название функции поиска>
                xml_path: <Путь до элемента XML>
        $root: null
        $prefix: '*'
    <Имя исходного трансформера>:
        $template: <Имя плагина>
        $schema: <Схема получаемых данных>
            <Поле>:
                data_type: <Тип данных>
                xml_function: <Название функции поиска>
                xml_path: <Путь до элемента XML>
        $root: null
        $prefix: '*'
```

## Пример

```
$transformers:
    RuleTermParser:
        $template: xml_findone_parser
        $schema:
            Name:
                data_type: String
                xml_function: text
                xml_path: name
            Source:
                data_type: String
                xml_function: text
                xml_path: from/source-address/name
            Gateway:
                data_type: String
                xml_function: text
                xml_path: then/backup-remote-gateway
        $root: 'root'
        $prefix: '*'
    AddressFamilyFinditer:
        $template: xml_finditer_parser
        $schema:
            AddressFamily:
                data_type: String
                xml_function: text
                xml_path: ns:address-family-name
            InterfaceID:
                data_type: String
                xml_function: text
                xml_path: ../ns:name
            FamilyFlags:
                data_type: Array(String)
                xml_function: tags
                xml_path: ns:address-family-flags
        $root: null
        $prefix: '*'
```

Тип данных, название функции поиска, путь до элемента XML в `$schema` являются обязательными полями.

Пример заполнения секции `$loaders`:

```
$loaders:
    VirtualBox:
    -   $template: mapping
        $target: Core.Software
        $kind: detect
        $parent: OperatingSystem.Windows.WindowsHost
        $origin:
            $template: get_extractor_path
            $name: VirtualBoxRegistry
        $field_map:
            Name: VirtualBox
            Vendor: Oracle
            OsFamily: Windows
            Architecture: $row.SourceRegistryKey.Arch
            InstallPath: $row.InstallDirectory
            RuleTerm:
                $template: apply_transformer
                $name: RuleTermParser
                $source: $row.InstallDirectory
            AddressFamily:
                $template: apply_transformer
                $name: AddressFamilyFinditer
                $source: $row.InstallDirectory
```
