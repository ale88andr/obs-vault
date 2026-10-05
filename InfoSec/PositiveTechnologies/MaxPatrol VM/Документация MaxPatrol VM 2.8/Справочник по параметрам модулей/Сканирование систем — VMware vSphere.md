---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-параметрам-модулей"
doc_id: "2478370187"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2478370187"
section: "Справочник по параметрам модулей"
breadcrumb: "Справочник по параметрам модулей / Модули для сбора информации об активах / Модуль Audit / Сканирование систем — VMware vSphere"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Сканирование систем — VMware vSphere

> [!info] Раздел: System.Collections.Hashtable[@{Id=2478370187; ReuseId=; Title=Сканирование систем — VMware vSphere; Depth=4; Path=Справочник по параметрам модулей / Модули для сбора информации об активах / Модуль Audit / Сканирование систем — VMware vSphere; Segments=System.Object[]; Index=580}.Id])
> @{Id=2478370187; ReuseId=; Title=Сканирование систем — VMware vSphere; Depth=4; Path=Справочник по параметрам модулей / Модули для сбора информации об активах / Модуль Audit / Сканирование систем — VMware vSphere; Segments=System.Object[]; Index=580}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2478370187)

---

Секция содержит следующие параметры для настройки сбора данных с VMware vCenter Server:

- **Учетная запись** — раскрывающийся список для выбора учетной записи, которая будет использоваться MP 10 Collector при аутентификации на сервере.
- **Порт** — дополнительное поле для ввода номера порта подключения к серверу (по умолчанию 443).
- **Использовать протокол SSL** — при включении используется протокол SSL.
- **Проверять сертификат SSL** — при включении для аутентификации на сервере используется сертификат SSL.

  > [!note] Примечание
  > Для использования сертификата на узле VMware vCenter Server необходимо выпустить сертификат (поле Subject Alternative Name должно содержать IP-адрес) и с помощью утилиты vSphere Certificate Manager добавить его в VMware Endpoint Certificate Store; на узле MP 10 Collector необходимо штатными средствами ОС добавить этот сертификат в хранилище сертификатов от доверенных корневых центров сертификации.
