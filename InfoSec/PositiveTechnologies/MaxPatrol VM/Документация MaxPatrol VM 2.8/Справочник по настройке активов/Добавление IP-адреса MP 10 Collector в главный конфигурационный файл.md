---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7340770955"
reuse_id: "7392431627"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7340770955"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Redis 6.2 и выше: настройка актива / Добавление IP-адреса MP 10 Collector в главный конфигурационный файл"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Добавление IP-адреса MP 10 Collector в главный конфигурационный файл

> [!info] Раздел: System.Collections.Hashtable[@{Id=7340770955; ReuseId=7392431627; Title=Добавление IP-адреса MP 10 Collector в главный конфигурационный файл; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Redis 6.2 и выше: настройка актива / Добавление IP-адреса MP 10 Collector в главный конфигурационный файл; Segments=System.Object[]; Index=467}.Id])
> @{Id=7340770955; ReuseId=7392431627; Title=Добавление IP-адреса MP 10 Collector в главный конфигурационный файл; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Redis 6.2 и выше: настройка актива / Добавление IP-адреса MP 10 Collector в главный конфигурационный файл; Segments=System.Object[]; Index=467}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7340770955)

---

**Задача.** Чтобы добавить IP-адрес MP 10 Collector в главный конфигурационный файл:

1. Откройте главный конфигурационный файл:
   ```bash
   sudo nano /etc/redis/redis.conf
   ```
2. В секции `NETWORK` в параметре `bind` укажите IP-адрес MP 10 Collector.
3. Сохраните изменения.
4. Перезапустите актив:
   ```bash
   sudo systemctl restart redis-server
   ```
