---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "8029874315"
reuse_id: "8490549131"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8029874315"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы виртуализации / VMware vSphere Hypervisor (ESXi) 6.5—7.0: настройка актива / Настройка IP-адреса и FQDN на активе"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка IP-адреса и FQDN на активе

> [!info] Раздел: System.Collections.Hashtable[@{Id=8029874315; ReuseId=8490549131; Title=Настройка IP-адреса и FQDN на активе; Depth=4; Path=Справочник по настройке активов / Системы виртуализации / VMware vSphere Hypervisor (ESXi) 6.5—7.0: настройка актива / Настройка IP-адреса и FQDN на активе; Segments=System.Object[]; Index=417}.Id])
> @{Id=8029874315; ReuseId=8490549131; Title=Настройка IP-адреса и FQDN на активе; Depth=4; Path=Справочник по настройке активов / Системы виртуализации / VMware vSphere Hypervisor (ESXi) 6.5—7.0: настройка актива / Настройка IP-адреса и FQDN на активе; Segments=System.Object[]; Index=417}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8029874315)

---

> [!warning] Внимание
> Если VMware vSphere Hypervisor (ESXi) уже входит в кластер, изменение его FQDN может привести к проблемам в работе кластера.

**Задача.** Чтобы настроить IP-адрес и FQDN на активе:

1. На узле, с которого производится настройка, запустите терминальный клиент, поддерживающий протокол SSH.
2. Пройдите аутентификацию на активе.
3. Откройте файл `/etc/hosts`.
4. Добавьте в файл строки:
   ```xml
   <IP-адрес сервера VMware vSphere Hypervisor (ESXi)> <FQDN сервера VMware vSphere Hypervisor (ESXi)>
   127.0.0.1 <FQDN сервера VMware vSphere Hypervisor (ESXi)>
   ::1 <FQDN сервера VMware vSphere Hypervisor (ESXi)>
   ```
5. Сохраните файл.
6. Перезагрузите актив.
