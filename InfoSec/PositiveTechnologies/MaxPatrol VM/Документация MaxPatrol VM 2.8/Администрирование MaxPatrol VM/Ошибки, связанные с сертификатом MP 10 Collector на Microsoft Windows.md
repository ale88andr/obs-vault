---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "11021333515"
reuse_id: "11240247947"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11021333515"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows

> [!info] Раздел: System.Collections.Hashtable[@{Id=11021333515; ReuseId=11240247947; Title=Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows; Segments=System.Object[]; Index=113}.Id])
> @{Id=11021333515; ReuseId=11240247947; Title=Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Ошибки, связанные с сертификатом MP 10 Collector на Microsoft Windows; Segments=System.Object[]; Index=113}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11021333515)

---

Если после установки сертификатов для MP 10 Collector на Microsoft Windows сервис недоступен, необходимо проанализировать журналы компонента. Файлы журналов хранятся в папке `<Диск установки>:\ProgramData\Positive Technologies\MaxPatrol 10 Agent\log`.

## Ошибка доступа к сертификату и ключу

Вид сообщения об ошибке: `librmq: Failed to connect to broker [:5671/siem [SSL ON]]. Description: cert path check FAILED`.

**Возможные причины**

Не были указаны полные пути к файлам сертификата и закрытого ключа. Также возможно, что файлы сертификата и закрытого ключа находятся в папке, к которой нет доступа у пользователя Network Service.

**Решение**

**Задача.** Чтобы решить проблему:

1. Проверьте правильность путей к файлам сертификата и закрытого ключа.
2. Убедитесь, что у пользователя Network Service есть доступ к папке с файлами сертификата и закрытого ключа.
3. Установите параметры сертификата повторно:
   ```
   coreagentcfg set -p RMQ_SSL_CA_CERTIFICATE <Путь к файлу сертификата ЦС> RMQ_SSL_CERTIFICATE <Путь к файлу сертификата Collector> RMQ_SSL_KEY <Путь к файлу закрытого ключа Collector>
   ```

## Указан неверный файл сертификата

Вид сообщения об ошибке: `librmq: Error connect to amqp server: amqp_ssl_socket_set_key FAILED`.

**Возможные причины**

В качестве файла ключа RMQ_SSL_KEY указан неподходящий файл.

**Решение**

**Задача.** Чтобы исправить проблему,

1. Проверьте соответствие путей параметрам.
   - RMQ_SSL_CA_CERTIFICATE — Путь к файлу сертификата ЦС;
   - RMQ_SSL_CERTIFICATE — Путь к файлу сертификата MP 10 Collector;
   - RMQ_SSL_KEY — Путь к файлу закрытого ключа MP 10 Collector.
