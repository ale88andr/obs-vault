---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5907809803"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5907809803"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / MongoDB 3.6 и выше: настройка актива / Операции MongoDB"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Операции MongoDB

> [!info] Раздел: System.Collections.Hashtable[@{Id=5907809803; ReuseId=; Title=Операции MongoDB; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / MongoDB 3.6 и выше: настройка актива / Операции MongoDB; Segments=System.Object[]; Index=452}.Id])
> @{Id=5907809803; ReuseId=; Title=Операции MongoDB; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / MongoDB 3.6 и выше: настройка актива / Операции MongoDB; Segments=System.Object[]; Index=452}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5907809803)

---

Для успешного сканирования необходимо предоставить учетной записи привилегии на выполнение операций, указанных в таблице ниже.

**Операции MongoDB**

| Операция | Необходимые привилегии | Описание |
| --- | --- | --- |
| buildInfo | — | Получение информации о версии MongoDB |
| getCmdLineOpts | clusterMonitor | Получение информации о пути установки MongoDB, файле конфигурации `mongod.cfg`, журнале аудита и событиях входа пользователей в систему |
| getParameter | clusterMonitor | Получение информации о параметрах enableLocalhostAuthBypass и tlsMode |
| getDBNames | clusterMonitor | Получение информации о базах MongoDB |
| getUsers | viewUser | Получение информации о пользователях MongoDB |
| getRoles | viewRole | Получение информации о ролях MongoDB |
