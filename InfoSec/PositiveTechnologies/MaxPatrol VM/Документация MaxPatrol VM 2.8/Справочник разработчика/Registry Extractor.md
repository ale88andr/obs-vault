---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "6997328395"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6997328395"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Registry Extractor"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Registry Extractor

> [!info] Раздел: System.Collections.Hashtable[@{Id=6997328395; ReuseId=; Title=Registry Extractor; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Registry Extractor; Segments=System.Object[]; Index=628}.Id])
> @{Id=6997328395; ReuseId=; Title=Registry Extractor; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Registry Extractor; Segments=System.Object[]; Index=628}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6997328395)

---

Экстрактор Registry Extractor позволяет делать запросы WMI по ключам реестра Windows.

Структура экстрактора:

```
$extractors:
    Registry: <Имя экстрактора>
        $template: <Имя плагина>
        $paths: <Пути к ключам реестра>
        - HKEY_LOCAL_MACHINE\SOFTWARE\Oracle\VirtualBox
        - HKEY_LOCAL_MACHINE\SOFTWARE\Sun\xVM VirtualBox
        - HKLM\SOFTWARE\{{{VirtualBox\d+\.\d+ \(x64\)}}}
        $mapping: <Отображение параметров системы в параметрах источника данных>
            <Параметр источника данных>: <Поле в $scheme>
        $schema: <Схема получаемых данных>
            <Поле>: <Тип данных>
            SourceRegistryKey: RegistryKey
```

Имя экстрактора, имя плагина, пути до ключей реестра, а также схема данных являются обязательными полями.

## Пример

```
$extractors:
    Registry:
        $template: registry_select
        $paths:
        - HKEY_LOCAL_MACHINE\SOFTWARE\Oracle\VirtualBox
        - HKEY_LOCAL_MACHINE\SOFTWARE\Sun\xVM VirtualBox
        - HKLM\SOFTWARE\{{{VirtualBox\d+\.\d+ \(x64\)}}}
        $mapping:
            InstallDir: InstallDirectory
        $schema:
            InstallDirectory: String
            Version: String
            VersionExt: String
            SourceRegistryKey: RegistryKey
```

Особенности заполнения полей экстракторов Registry Extractor:

- Допустимое значение `$template` — `registry_select`.
- Поля `$paths` используют тип данных String, передаются в виде списка и поддерживают регулярные выражения.
- Значения полей `$mapping` должны соответствовать полям в `$shema`.
- В `$schema` поле `SourceRegistryKey` с типом `RegistryKey` является обязательным.
