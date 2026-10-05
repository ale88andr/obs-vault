---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "8018982795"
reuse_id: "8489333259"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8018982795"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы виртуализации / VMware vCenter Server 5.5—8.0: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# VMware vCenter Server 5.5—8.0: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=8018982795; ReuseId=8489333259; Title=VMware vCenter Server 5.5—8.0: настройка актива; Depth=3; Path=Справочник по настройке активов / Системы виртуализации / VMware vCenter Server 5.5—8.0: настройка актива; Segments=System.Object[]; Index=410}.Id])
> @{Id=8018982795; ReuseId=8489333259; Title=VMware vCenter Server 5.5—8.0: настройка актива; Depth=3; Path=Справочник по настройке активов / Системы виртуализации / VMware vCenter Server 5.5—8.0: настройка актива; Segments=System.Object[]; Index=410}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8018982795)

---

Настройку VMware vCenter Server for Windows 5.5—6.5 нужно выполнять от имени учетной записи, имеющей права администратора ОС и права администратора VMware vCenter Server for Windows.

Настройку VMware vCenter Server Appliance 6.7—8.0 нужно выполнять от имени учетной записи, имеющей права администратора VMware vCenter Server Appliance.

> [!warning] Внимание
> При использовании межсетевого экрана требуется настроить в нем правила, разрешающие внешние подключения к используемым портам TCP/IP. Для доступа к VMware vCenter Server 5.5—8.0 по умолчанию используется порт 443/TCP.

Для аудита VMware vCenter Server for Windows 5.5—6.5 через vSphere API нужно:

1. Средствами ОС [[Создание учетной записи ОС|создать учетную запись]] для доступа MP 10 Collector.
2. Добавить учетную запись [[Добавление учетной записи в локальную политику безопасности|в локальную (групповую) политику безопасности]] «Доступ к компьютеру из сети» (Access this computer from the network).
3. Добавить учетную запись в VMware vCenter Server for Windows и назначить ей права с помощью роли. Настройку нужно выполнять по инструкциям из разделов:

- «[[Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0]]»;
- «[[Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 6.5]]».

Для аудита VMware vCenter Server Appliance 6.7—8.0 через vSphere API нужно:

1. Создать [[Создание учетной записи для VMware vCenter Server Appliance 6.7—8.0|учетную запись]] для VMware vCenter Server Appliance.
2. Назначить [[Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0|права учетной записи с помощью роли]].
