---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "6997330699"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6997330699"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Экстракторы WMI"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Экстракторы WMI

> [!info] Раздел: System.Collections.Hashtable[@{Id=6997330699; ReuseId=; Title=Экстракторы WMI; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Экстракторы WMI; Segments=System.Object[]; Index=630}.Id])
> @{Id=6997330699; ReuseId=; Title=Экстракторы WMI; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Экстракторы WMI; Segments=System.Object[]; Index=630}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6997330699)

---

## Экстрактор wmi_select_adapter

Экстрактор wmi_select_adapter предназначен для WMI-запросов формата `SELECT [%field%,]+ FROM [%wmi_class%]`.

Структура экстрактора:

```
$extractors:
    <Имя экстрактора>:
        $template: wmi_select
        $query: <Класс WMI, объекты которого необходимо получить>
        $ns: <Пространство WMI для подключения>
        $mapping: <Отображение параметров системы в параметрах источника данных>
            <Свойство класса WMI>: <Поле в $schema>
        $schema: <Схема получаемых данных>
            <Поле>: <Тип данных>
```

Имя экстрактора, класс WMI и схема данных являются обязательными полями.

## Пример

Экстрактор для запроса `SELECT Name`, `Domain`, `TotalPhysicalMemory`, `Manufacturer FROM Win32_ComputerSystem` с преобразованием поля `Manufacturer` в поле `Vendor`.

```
$extractors:
    Win32ComputerSystem:
        $template: wmi_select
        $query: Win32_ComputerSystem
        $mapping:
            Manufacturer: Vendor
        $schema:
            Name: String
            Domain: String
            TotalPhysicalMemory: Int
            Vendor: String
```

## Экстрактор wmi_query_adapter

Экстрактор wmi_query_adapter предназначен для произвольных WMI-запросов.

Структура экстрактора:

```
$extractors:
    <Имя экстрактора>:
        $template: wmi_query
        $query: <Шаблон выполняемого запроса с обязательными операторами SELECT и FROM>
        $ns: <Пространство WMI для подключения>
        $mapping: <Отображение параметров системы в параметрах источника данных>
            <Свойство из WMI-запроса>: <Поле в $schema>
        $schema: <Схема получаемых данных>
            <Поле>: <Тип данных>
```

Имя экстрактора, шаблон запроса и схема данных являются обязательными полями.

## Пример

Экстрактор для запроса `Get-WmiObject -namespace "root\cimv2" -query "SELECT * FROM Win32_Group WHERE LocalAccount = TRUE"`

```
$extractors:
    Win32LocalGroup:
        $template: wmi_query
        $query: SELECT * FROM Win32_Group WHERE LocalAccount = TRUE
        $ns: root\cimv2
        $schema:
            Name: String
            Domain: String
            SID: String
```

Особенности заполнения полей экстракторов WMI:

- В названии класса WMI в поле `$query` можно использовать латинские буквы, цифры или символы подчеркивания. Название класса должно начинаться с буквы и не должно заканчиваться символом подчеркивания.
- Если поле `$ns` не заполнено, запрос будет выполнен в пространство `root\cimv2`.
