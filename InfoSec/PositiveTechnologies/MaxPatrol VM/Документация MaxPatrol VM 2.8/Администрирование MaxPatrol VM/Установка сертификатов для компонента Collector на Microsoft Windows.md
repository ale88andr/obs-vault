---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "10963555083"
reuse_id: "11088625675"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10963555083"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Установка сертификатов для компонента Collector на Microsoft Windows"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Установка сертификатов для компонента Collector на Microsoft Windows

> [!info] Раздел: System.Collections.Hashtable[@{Id=10963555083; ReuseId=11088625675; Title=Установка сертификатов для компонента Collector на Microsoft Windows; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Установка сертификатов для компонента Collector на Microsoft Windows; Segments=System.Object[]; Index=129}.Id])
> @{Id=10963555083; ReuseId=11088625675; Title=Установка сертификатов для компонента Collector на Microsoft Windows; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Установка сертификатов для компонента Collector на Microsoft Windows; Segments=System.Object[]; Index=129}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10963555083)

---

## Выпуск сертификатов

**Задача.** Чтобы установить сертификаты:

1. На сервере с установленной ролью Deployer разместите [утилиту для генерации сертификатов](https://storage.ptsecurity.com/f/2e67ec57d9684587ac1a/).
2. Создайте сертификат и закрытый ключ для компонента Collector:
   ```
   ./generate_remote_signed_cert.sh --common-name agent --cert ~/RmqClientAgent.crt --key ~/RmqClientAgent.key
   ```

Внутри указанных каталогов будут созданы файлы сертификата и закрытого ключа.

## Установка сертификатов на сервер Microsoft Windows

> [!note] Примечание
> Вы можете воспользоваться этой инструкцией, чтобы перенести на сервер пользовательские сертификаты.

**Задача.** Чтобы перенести сертификаты и закрытые ключи для компонента MP 10 Collector на Microsoft Windows:

1. Разместите файлы сертификата ЦС (расположен на сервере с ролью Deployer в файле `/opt/deployer/pki/rootCA.crt`), пользовательского сертификата и закрытого ключа на сервере Microsoft Windows в постоянной папке, к которой есть доступ у пользователя Network Service.
2. Откройте командную строку от имени администратора.
3. Установите сертификат для MP 10 Collector, выполнив следующую команду:
   ```
   coreagentcfg set -p RMQ_SSL_CA_CERTIFICATE <Полный путь к файлу сертификата ЦС> RMQ_SSL_CERTIFICATE <Полный путь к файлу пользовательского сертификата> RMQ_SSL_KEY <Полный путь к файлу закрытого ключа>
   ```
   → Например:
   ```
   coreagentcfg set -p RMQ_SSL_CA_CERTIFICATE "C:\ProgramData\Positive Technologies\MaxPatrol 10 Agent\cert\rootCA.crt" RMQ_SSL_CERTIFICATE "C:\ProgramData\Positive Technologies\MaxPatrol 10 Agent\cert\RmqClientAgent.crt" RMQ_SSL_KEY "C:\ProgramData\Positive Technologies\MaxPatrol 10 Agent\cert\RmqClientAgent.key"
   ```
4. Проверьте установку сертификатов:
   ```
   coreagentcfg get
   ```
   → Значения параметров `RMQ_SSL_CA_CERTIFICATE`, `RMQ_SSL_CERTIFICATE` и `RMQ_SSL_KEY` должны содержать пути к установленным сертификатам и закрытому ключу.

Если при установке сертификатов возникли проблемы, вы можете ознакомиться с [[Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows|часто возникающими ошибками]].
