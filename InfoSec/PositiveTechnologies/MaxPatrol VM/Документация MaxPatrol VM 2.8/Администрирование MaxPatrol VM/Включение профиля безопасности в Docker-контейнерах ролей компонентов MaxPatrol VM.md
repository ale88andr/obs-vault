---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7616553867"
reuse_id: "8852534667"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7616553867"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Просмотр и изменение параметров конфигурации MaxPatrol VM / Включение профиля безопасности в Docker-контейнерах ролей компонентов MaxPatrol VM"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Включение профиля безопасности в Docker-контейнерах ролей компонентов MaxPatrol VM

> [!info] Раздел: System.Collections.Hashtable[@{Id=7616553867; ReuseId=8852534667; Title=Включение профиля безопасности в Docker-контейнерах ролей компонентов MaxPatrol VM; Depth=3; Path=Администрирование MaxPatrol VM / Просмотр и изменение параметров конфигурации MaxPatrol VM / Включение профиля безопасности в Docker-контейнерах ролей компонентов MaxPatrol VM; Segments=System.Object[]; Index=88}.Id])
> @{Id=7616553867; ReuseId=8852534667; Title=Включение профиля безопасности в Docker-контейнерах ролей компонентов MaxPatrol VM; Depth=3; Path=Администрирование MaxPatrol VM / Просмотр и изменение параметров конфигурации MaxPatrol VM / Включение профиля безопасности в Docker-контейнерах ролей компонентов MaxPatrol VM; Segments=System.Object[]; Index=88}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7616553867)

---

Для повышения уровня безопасности Docker-контейнеров ролей рекомендуется включить в них профили безопасности.

**Задача.** Чтобы включить профиль безопасности в Docker-контейнерах ролей:

1. Измените конфигурацию роли Deployer, указав для параметра `SeccompEnabled` значение `True`.
2. Измените конфигурацию остальных ролей, указав для параметра `SeccompEnabled` значение `True`.
