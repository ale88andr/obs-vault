---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7738485131"
reuse_id: "8032772747"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7738485131"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы электронной почты / Почта VK WorkSpace 1.20 и выше: настройка актива / Настройка доступа к API"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка доступа к API

> [!info] Раздел: System.Collections.Hashtable[@{Id=7738485131; ReuseId=8032772747; Title=Настройка доступа к API; Depth=4; Path=Справочник по настройке активов / Системы электронной почты / Почта VK WorkSpace 1.20 и выше: настройка актива / Настройка доступа к API; Segments=System.Object[]; Index=483}.Id])
> @{Id=7738485131; ReuseId=8032772747; Title=Настройка доступа к API; Depth=4; Path=Справочник по настройке активов / Системы электронной почты / Почта VK WorkSpace 1.20 и выше: настройка актива / Настройка доступа к API; Segments=System.Object[]; Index=483}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7738485131)

---

Необходимо предоставить MP 10 Collector доступ к API с использованием идентификатора (client_id). MP 10 Collector будет использовать этот идентификатор для авторизации по протоколу OAuth2.

Инструкцию необходимо выполнять от имени учетной записи операционной системы, где установлен актив, с правами sudo.

> [!warning] Внимание
> Если узлы объединены в кластер, настройку нужно выполнять на главном узле.

> [!warning] Внимание
> Настраивать доступ к API необходимо после каждого перезапуска контейнеров.

**Задача.** Чтобы настроить доступ к API:

1. На узле, с которого производится настройка, запустите терминальный клиент, поддерживающий протокол SSH.
2. Пройдите аутентификацию на активе.
3. Выполните команду:
   ```bash
   docker exec -i swadb1 mysql <<< "use oauth; update client set scope=concat(scope,',mail.biz') where name='mail-ios';"
   ```
4. Перезапустите сервисы swa* и cube*:
   ```bash
   systemctl restart onpremise-container-cube1
   systemctl restart onpremise-container-swa1
   ```
