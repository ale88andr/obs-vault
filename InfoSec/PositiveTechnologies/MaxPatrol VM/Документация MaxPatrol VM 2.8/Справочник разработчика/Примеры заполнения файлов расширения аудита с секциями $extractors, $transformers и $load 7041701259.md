---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7041701259"
reuse_id: "7040508939"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7041701259"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Примеры заполнения файлов расширения аудита с секциями $extractors, $transformers и $loaders"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Примеры заполнения файлов расширения аудита с секциями $extractors, $transformers и $loaders

> [!info] Раздел: System.Collections.Hashtable[@{Id=7041701259; ReuseId=7040508939; Title=Примеры заполнения файлов расширения аудита с секциями $extractors, $transformers и $loaders; Depth=3; Path=Справочник разработчика / Создание файла расширения аудита / Примеры заполнения файлов расширения аудита с секциями $extractors, $transformers и $loaders; Segments=System.Object[]; Index=637}.Id])
> @{Id=7041701259; ReuseId=7040508939; Title=Примеры заполнения файлов расширения аудита с секциями $extractors, $transformers и $loaders; Depth=3; Path=Справочник разработчика / Создание файла расширения аудита / Примеры заполнения файлов расширения аудита с секциями $extractors, $transformers и $loaders; Segments=System.Object[]; Index=637}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7041701259)

---

## Пример заполнения файла расширения аудита для поиска приложения Telegram с сохранением только первых двух секций версии

```
$extractors:
    GetTelegramExeInfo:
        $template: windows_file_info
        $filepath: C:\Users\aivanov\AppData\Roaming\Telegram Desktop\Telegram.exe
$transformers:
    TelegramTwoSections:
        $template: regexp_search
        $pattern: '(?P<Value>^\d+\.\d+).*'
        $schema:
            Value: String
    FirstTwoSections:
        $template: function
        $apply: TelegramTwoSections
        $output: Value
$loaders:
    TelegramExeInfo:
    -   $template: mapping
        $target: Core.Software
        $kind: detect
        $parent: OperatingSystem.Windows.WindowsHost
        $origin:
            $template: get_extractor_path
            $name: GetTelegramExeInfo
        $field_map:
            Vendor: $row.CompanyName
            Name: $row.ProductName
            InstallPath: $row.Path
            Version:
                $template: apply_transformer
                $name: FirstTwoSections
                $source: $row.FileVersion
```

## Пример заполнения файла расширения аудита для приложения VirtualBox

```
$extractors:
    VirtualBoxRegistry:
        $template: registry_select
        $paths:
        - HKEY_LOCAL_MACHINE\SOFTWARE\Oracle\VirtualBox
        - HKEY_LOCAL_MACHINE\SOFTWARE\Sun\xVM VirtualBox
        $mapping:
            InstallDir: InstallDirectory
        $schema:
            InstallDirectory: String
            Version: String
            VersionExt: String
            SourceRegistryKey: RegistryKey
$transformers:
    VirtualBoxVersionParser:
        $template: regexp_search
        $pattern: '^.*(?P<Value>\d+\.\d+\.\d+).*$'
        $schema:
            Value: String
    get_virtualbox_version:
        $template: function
        $apply: VirtualBoxVersionParser
        $output: Value
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
            Version:
                $template: apply_transformer
                $name: get_virtualbox_version
                $source:
                     $template: first_not_empty_string
                     $source:
                     - $row.Version
                     - $row.VersionExt
```

## Пример заполнения файла расширения аудита для поиска приложения Telegram в папке C:\\Users

```
$extractors:
    TelegramFileInfo:
        $template: windows_file_info_recursive
        $filemasks:
        - telegram.exe
        - телеграм.exe
        $usesymlinks: false
        $dirs:
        - C:\Users
$loaders:
    Telegram:
    -   $template: mapping
        $target: Core.Software
        $kind: detect
        $parent: OperatingSystem.Windows.WindowsHost
        $origin:
            $template: get_extractor_path
            $name: TelegramFileInfo
        $field_map:
            Name: Telegram
            Vendor: Telegram
            OsFamily: Windows
            InstallPath: $row.Path
            Version:
                $template: first_not_empty_string
                $source:
                    - $row.ProductVersion
                    - $row.ProductVersionEx
```
