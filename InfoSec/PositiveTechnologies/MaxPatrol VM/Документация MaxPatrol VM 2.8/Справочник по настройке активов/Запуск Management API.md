---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "3000601483"
reuse_id: "6152907787"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3000601483"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 80.10 и выше: настройка актива / Запуск Management API"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Запуск Management API

> [!info] Раздел: System.Collections.Hashtable[@{Id=3000601483; ReuseId=6152907787; Title=Запуск Management API; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 80.10 и выше: настройка актива / Запуск Management API; Segments=System.Object[]; Index=343}.Id])
> @{Id=3000601483; ReuseId=6152907787; Title=Запуск Management API; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 80.10 и выше: настройка актива / Запуск Management API; Segments=System.Object[]; Index=343}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3000601483)

---

Для проведения аудита требуется компонент Management API, который входит в состав сервера управления Check Point GAiA.

**Задача.** Чтобы запустить компонент Management API на сервере управления:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Проверьте статус Management API:
   ```
   api status
   ```
4. Если ответ на экране содержит `Overall API Status: The API Server Is Not Running!`, запустите Management API:
   ```
   api start
   ```

> Management API запущен.
