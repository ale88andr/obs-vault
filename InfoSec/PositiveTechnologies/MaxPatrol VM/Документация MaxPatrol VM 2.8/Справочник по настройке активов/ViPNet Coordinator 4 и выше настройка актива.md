---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7461879947"
reuse_id: "8045720587"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7461879947"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / ViPNet Coordinator 4 и выше: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# ViPNet Coordinator 4 и выше: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=7461879947; ReuseId=8045720587; Title=ViPNet Coordinator 4 и выше: настройка актива; Depth=3; Path=Справочник по настройке активов / Сетевые устройства / ViPNet Coordinator 4 и выше: настройка актива; Segments=System.Object[]; Index=395}.Id])
> @{Id=7461879947; ReuseId=8045720587; Title=ViPNet Coordinator 4 и выше: настройка актива; Depth=3; Path=Справочник по настройке активов / Сетевые устройства / ViPNet Coordinator 4 и выше: настройка актива; Segments=System.Object[]; Index=395}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7461879947)

---

Настройку актива нужно выполнять от имени учетной записи администратора защищенной сети ViPNet.

Для аудита на активе нужно создать учетную запись с помощью программного обеспечения «ViPNet Центр управления сетью» или ViPNet Prime для доступа MP 10 Collector к активу по протоколу SSH.

> [!warning] Внимание
> Максимальная длина имени объекта в правилах фильтрации — 51 символ.

Для подключения MP 10 Collector к активу по протоколу SSH необходимо добавить правило доступа в локальный фильтр firewall.

**Задача.** Чтобы добавить правило доступа в локальный фильтр firewall,

1. выполните команду:
   ```
   firewall local add <Номер правила доступа> rule "<Название правила доступа>" src <IP-адрес MP 10 Collector> dst @local tcp dport 22 pass
   ```
