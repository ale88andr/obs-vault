---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "3518673547"
reuse_id: "3521757835"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3518673547"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Службы каталогов / Microsoft Active Directory в Windows Server 2003—2022: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Microsoft Active Directory в Windows Server 2003—2022: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=3518673547; ReuseId=3521757835; Title=Microsoft Active Directory в Windows Server 2003—2022: настройка актива; Depth=3; Path=Справочник по настройке активов / Службы каталогов / Microsoft Active Directory в Windows Server 2003—2022: настройка актива; Segments=System.Object[]; Index=487}.Id])
> @{Id=3518673547; ReuseId=3521757835; Title=Microsoft Active Directory в Windows Server 2003—2022: настройка актива; Depth=3; Path=Справочник по настройке активов / Службы каталогов / Microsoft Active Directory в Windows Server 2003—2022: настройка актива; Segments=System.Object[]; Index=487}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3518673547)

---

Для проведения аудита Active Directory нужно создать учетную запись, имеющую права на чтение домена Active Directory. Данные этой учетной записи нужно указать при добавлении учетной записи в MaxPatrol VM.

> [!warning] Внимание
> При использовании в IT-инфраструктуре организации межсетевого экрана или других средств для контроля сетевого трафика требуется настроить в них правила, разрешающие трафик в обоих направлениях между узлом источника и узлом MP 10 Collector. При подключении по протоколу LDAP используется TCP-порт 389 и UDP-порт 389, при подключении по протоколу LDAPS (если контроллер домена является глобальным каталогом) — TCP-порт 636 и UDP-порт 636 или TCP-порт 3269 и UDP-порт 3269. Для подключения по протоколу LDAPS необходима предварительная настройка (инструкцию см. на сайте [learn.microsoft.com](https://learn.microsoft.com/) в разделе «Включение протокола LDAP через протокол SSL с использованием стороннего центра сертификации»).

> [!note] Примечание
> Если с момента последнего входа учетной записи компьютера в домен Active Directory прошло более 90 суток, то для этого компьютера не будет добавлен актив. Для определения даты последнего входа в домен используется атрибут `LastLogonTimestamp`.

> [!note] Примечание
> Вы можете детально настроить учетную запись, предоставив ей права на выполнение [[Команды, выполняемые при аудите активов|команд для проведения аудита]].
