---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7082864523"
reuse_id: "8852525835"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7082864523"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Просмотр и изменение параметров конфигурации MaxPatrol VM / Настройка прокси-сервера для онлайн-активации PT MC"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка прокси-сервера для онлайн-активации PT MC

> [!info] Раздел: System.Collections.Hashtable[@{Id=7082864523; ReuseId=8852525835; Title=Настройка прокси-сервера для онлайн-активации PT MC; Depth=3; Path=Администрирование MaxPatrol VM / Просмотр и изменение параметров конфигурации MaxPatrol VM / Настройка прокси-сервера для онлайн-активации PT MC; Segments=System.Object[]; Index=86}.Id])
> @{Id=7082864523; ReuseId=8852525835; Title=Настройка прокси-сервера для онлайн-активации PT MC; Depth=3; Path=Администрирование MaxPatrol VM / Просмотр и изменение параметров конфигурации MaxPatrol VM / Настройка прокси-сервера для онлайн-активации PT MC; Segments=System.Object[]; Index=86}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7082864523)

---

**Задача.** Чтобы настроить прокси-сервер,

1. измените конфигурацию роли Management and Configuration, указав значения следующих параметров:
   ```
   ProxyPassword: <Пароль для доступа к прокси-серверу>
   ProxyUrl: <URL прокси-сервера>
   ProxyUserName: <Логин для доступа к прокси-серверу>
   UseProxy: True
   ```
