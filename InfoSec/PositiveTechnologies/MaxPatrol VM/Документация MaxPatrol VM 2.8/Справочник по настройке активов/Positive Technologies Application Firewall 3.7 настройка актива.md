---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "10493638667"
reuse_id: "10570703115"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10493638667"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы защиты сети / Positive Technologies Application Firewall 3.7: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Positive Technologies Application Firewall 3.7: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=10493638667; ReuseId=10570703115; Title=Positive Technologies Application Firewall 3.7: настройка актива; Depth=3; Path=Справочник по настройке активов / Системы защиты сети / Positive Technologies Application Firewall 3.7: настройка актива; Segments=System.Object[]; Index=429}.Id])
> @{Id=10493638667; ReuseId=10570703115; Title=Positive Technologies Application Firewall 3.7: настройка актива; Depth=3; Path=Справочник по настройке активов / Системы защиты сети / Positive Technologies Application Firewall 3.7: настройка актива; Segments=System.Object[]; Index=429}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10493638667)

---

> [!warning] Внимание
> При использовании в IT-инфраструктуре организации межсетевого экрана или других средств для контроля сетевого трафика требуется настроить в них правила, разрешающие трафик в обоих направлениях между узлом актива и узлом MP 10 Collector. Для проведения аудита актива по протоколу SSH используется порт TCP 22013.

Для аудита актива нужно:

1. [[Создание учетной записи в ОС семейства Unix|Создать учетную запись ОС для доступа к активу]].
2. [[Аудит с помощью учетной записи с sudo-привилегиями|Добавить для учетной записи sudo-привилегии]].
3. Разрешить учетной записи доступ к активу по протоколу SSH.

**Задача.** Чтобы разрешить учетной записи доступ к активу по протоколу SSH:

1. Откройте файл конфигурации `/etc/ssh/sshd_config`.
2. В директиве AllowUsers добавьте учетную запись, созданную на шаге 1.
3. Сохраните изменения и закройте файл.
4. Выполните команду:
   ```bash
   systemctl reload sshd
   ```
