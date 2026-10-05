---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "6993950091"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6993950091"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Экстракторы классов File и FileInfo"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Экстракторы классов File и FileInfo

> [!info] Раздел: System.Collections.Hashtable[@{Id=6993950091; ReuseId=; Title=Экстракторы классов File и FileInfo; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Экстракторы классов File и FileInfo; Segments=System.Object[]; Index=626}.Id])
> @{Id=6993950091; ReuseId=; Title=Экстракторы классов File и FileInfo; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Экстракторы классов File и FileInfo; Segments=System.Object[]; Index=626}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6993950091)

---

## Экстракторы класса File

Экстракторы класса File позволяют получить содержимое файлов.

Экстрактор Unix file adapter должен иметь следующую структуру:

```
$extractors:
    <Имя экстрактора>
        $template: <Имя плагина>
        $filepath: <Путь к файлу>
        $schema: <Схема получаемых данных>
            Path: String
            Data: String
```

Имя экстрактора, имя плагина, путь к файлу, а также схема данных являются обязательными полями.

## Пример

```
$extractors:
UnixFile:
$template: unix_file
$filepath: /etc/testfile
$schema:
Path: String
Data: String
```

Экстрактор Windows file adapter должен иметь следующую структуру:

```
$extractors:
    <Имя экстрактора>
        $template: <Имя плагина>
        $filepath: <Путь к файлу>
        $encoding: <Кодировка для чтения файла>
        $schema: <Схема получаемых данных>
            Path: String
            Data: String
```

Имя экстрактора, имя плагина, путь к файлу, а также схема данных являются обязательными полями.

## Пример

```
$extractors:
    WindowsFile:
        $template: windows_file
        $filepath: C:\Users\File.txt
        $encoding: utf-8
        $schema:
            Path: String
            Data: String
```

Экстраторы Unix file adapter и Windows file adapter recursive возвращают значения следующих полей:

- `Data` — содержимое файла. Обязательное поле;
- `Path` — путь к файлу. Необязательное поле.

## Экстракторы класса FileInfo

Экстракторы класса FileInfo позволяют получить данные о файле.

Экстратор Windows file info adapter должен иметь следующую структуру:

```
$extractors:
    <Имя экстрактора>
        $template: <Имя плагина>
        $filepath: <Путь к файлу>
```

Имя экстрактора, имя плагина и путь к файлу являются обязательными полями.

## Пример

```
$extractors:
    WindowsFileInfo:
        $template: windows_file_info
        $filepath: C:\Users\File.txt
```

Экстрактор Windows file info recursive adapter должен иметь следующую структуру:

```
$extractors:
    <Имя экстрактора>
        $template: <Имя плагина>
        $filemasks: <Список масок регулярных выражений в именах файлов для поиска>
        $ignorlist: <Список масок регулярных выражений в именах каталогов, исключаемых из результатов поиска>
        $dirs: <Список корневых каталогов для рекурсивного обхода>
        $usesymlinks: <Искать ли по символьным ссылкам>
```

Имя экстрактора, имя плагина, список масок регулярных выражений в именах файлов и список корневых каталогов для рекурсивного обхода являются обязательными полями.

## Пример

```
$extractors:
    WindowsFileInfoRecursive
        $template: windows_file_info_recursive
        $filemasks:
        - File\.txt
        - File2\.txt
        $ignorlist:
        - Backup
        - Updates
        $dirs:
        - C:\Users
        $usesymlinks: true
```

Экстракторы Windows file info adapter и Windows file info recursive adapter возвращают следующие данные:

- `CompanyName` — название компании, создавшей файл;
- `FileDescription` — описание файла;
- `FileVersion` — версия файла;
- `FileVersionEx` — расширенная информация о версии файла;
- `InternalName` — внутреннее имя файла при наличии;
- `LegalCopyright` — копирайт, относящийся к файлу;
- `OriginalFilename` — имя файла;
- `ProductName` — название продукта, для которого создан файл;
- `ProductVersion` — версия продукта;
- `ProductVersionEx` — расширенная информация о версии продукта;
- `Path` — путь к файлу.
