---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-параметрам-модулей"
doc_id: "2478046475"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2478046475"
section: "Справочник по параметрам модулей"
breadcrumb: "Справочник по параметрам модулей / Модули для сбора информации об активах / Модуль Pentest / Сканирование UDP-служб"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Сканирование UDP-служб

> [!info] Раздел: System.Collections.Hashtable[@{Id=2478046475; ReuseId=; Title=Сканирование UDP-служб; Depth=4; Path=Справочник по параметрам модулей / Модули для сбора информации об активах / Модуль Pentest / Сканирование UDP-служб; Segments=System.Object[]; Index=593}.Id])
> @{Id=2478046475; ReuseId=; Title=Сканирование UDP-служб; Depth=4; Path=Справочник по параметрам модулей / Модули для сбора информации об активах / Модуль Pentest / Сканирование UDP-служб; Segments=System.Object[]; Index=593}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2478046475)

---

Секция содержит параметры для настройки обнаружения служб, которые используют UDP-порты. Это может замедлять процесс сканирования, особенно если в сети запрещены ICMP-пакеты «Порт недоступен». Ответ службы ожидается в течение времени, указанного в параметре **Тайм-аут ответа UDP-портов**. Доступны следующие параметры для включения обнаружения служб на указанных портах:

- **CA BrightStor ARCServe Backup (порт 41524)**;
- **Character Generator Protocol (порт 19)**;
- **Daytime (порт 13)**;
- **DB2 DAS (порт 523)**;
- **DHCP (порта 67)**;
- **DNS (порт 53)**;
- **ECHO (порт 7)**;
- **GTP (порты 2123, 2152, 3386)**;
- **ICQ (порт 4000)**;
- **IKE (порт 500)**;
- **IKE NAT-T (порт 4500)**;
- **IPMI (порт 623)**;
- **LLMNR (порт 5355)**;
- **mDNS (порт 5353)**;
- **Microsoft Remote Desktop Gateway**;
- **Порт RDG** — поле для ввода номера порта службы Microsoft Remote Desktop Gateway (по умолчанию 3391/UDP).
- **Microsoft RPC Port Mapper (порт 135)**;
- **Microsoft SQL Server (порт 1434)**;
- **NetBIOS Name (порт 137)**;
- **NTP (порт 123)**;
- **ONC RPC portmap (порт 111)**;
- **OpenVPN (порт 1194)**;
- **pcAnyWhere (порт 5632)**;
- **Quota (порт 17)**;
- **SIP (порт 5060)**;
- **SLP (порт 427)**;
- **SNMP (порт 161)**;
- **Teredo (порт 3544)**;
- **TFTP (порт 69)**;
- **Unreal Tournament (порт 7777)**;
- **UPNP (порт 1900)**;
- **XDMCP (порт 177)**;
- **Мemcached (порт 11211)**.
