---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "7106832523"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7106832523"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Подготовка к развертыванию MaxPatrol VM / Настройка ядра Astra Linux типа hardened"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка ядра Astra Linux типа hardened

> [!info] Раздел: System.Collections.Hashtable[@{Id=7106832523; ReuseId=; Title=Настройка ядра Astra Linux типа hardened; Depth=3; Path=Развертывание MaxPatrol VM / Подготовка к развертыванию MaxPatrol VM / Настройка ядра Astra Linux типа hardened; Segments=System.Object[]; Index=6}.Id])
> @{Id=7106832523; ReuseId=; Title=Настройка ядра Astra Linux типа hardened; Depth=3; Path=Развертывание MaxPatrol VM / Подготовка к развертыванию MaxPatrol VM / Настройка ядра Astra Linux типа hardened; Segments=System.Object[]; Index=6}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7106832523)

---

Для корректной работы Docker-контейнеров с операционной системой Astra Linux с типом ядра hardened необходимо, чтобы в Docker использовались компоненты cgroups версии 1. Для этого до установки MaxPatrol VM необходимо настроить ядра типа hardened на всех серверах с Astra Linux.

**Задача.** Чтобы настроить ядро Astra Linux:

1. Откройте конфигурационный файл `/etc/default/grub`.
2. В строку `GRUB_CMDLINE_LINUX_HARDENED` добавьте параметр `systemd.unified_cgroup_hierarchy` со значением 0.
   → Например:
   ```
   GRUB_CMDLINE_LINUX_HARDENED="slub_debug=P page_poison=1 slab_nomerge user.max_user_namespaces=0 kernel.kptr_restrict=1 vsyscall=none systemd.unified_cgroup_hierarchy=0"
   ```
3. Сохраните файл.
4. Выполните команду:
   ```
   update-grub
   ```
5. Перезагрузите сервер.

Вы можете проверить, применились ли изменения, выполнив команду `cat /proc/cmdline`: в параметрах ядра появится добавленная строка. Если на сервере уже установлен Docker, узнать используемую версию контрольной группы можно с помощью команды `docker info`: если изменения применились, параметр `Cgroup Driver` будет иметь значение `systemd`, а параметр `Cgroup Version` — `1`.
