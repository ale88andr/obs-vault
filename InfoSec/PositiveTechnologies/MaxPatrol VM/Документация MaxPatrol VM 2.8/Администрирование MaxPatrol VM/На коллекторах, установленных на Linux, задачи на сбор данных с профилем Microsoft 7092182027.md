---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7092182027"
reuse_id: "7091418379"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7092182027"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / На коллекторах, установленных на Linux, задачи на сбор данных с профилем Microsoft Active Directory завершаются ошибкой"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# На коллекторах, установленных на Linux, задачи на сбор данных с профилем Microsoft Active Directory завершаются ошибкой

> [!info] Раздел: System.Collections.Hashtable[@{Id=7092182027; ReuseId=7091418379; Title=На коллекторах, установленных на Linux, задачи на сбор данных с профилем Microsoft Active Directory завершаются ошибкой; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / На коллекторах, установленных на Linux, задачи на сбор данных с профилем Microsoft Active Directory завершаются ошибкой; Segments=System.Object[]; Index=109}.Id])
> @{Id=7092182027; ReuseId=7091418379; Title=На коллекторах, установленных на Linux, задачи на сбор данных с профилем Microsoft Active Directory завершаются ошибкой; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / На коллекторах, установленных на Linux, задачи на сбор данных с профилем Microsoft Active Directory завершаются ошибкой; Segments=System.Object[]; Index=109}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7092182027)

---

## Проблема

Задача на сбор данных с Windows XP или Windows Server 2003 с профилем Microsoft Active Directory Audit для коллектора, установленного на Linux, завершается ошибкой.

## Возможные причины

Для аутентификации на активе с помощью протокола Kerberos используется библиотека MIT Kerberos версии 1.18 или выше, которая содержит ошибку, приводящую к невозможности аутентификации.

## Решение

Необходимо установить пакет libkrb5-3 с библиотекой MIT Kerberos версии ниже 1.18 или пакет libkrb5-26-heimdal с библиотекой Heimdal Kerberos, которые вы можете скачать на сайте [debian.org](https://www.debian.org/).

**Задача.** Чтобы решить проблему:

1. Удалите пакет с библиотекой Kerberos, установленный в системе.
   > [!warning] Внимание
   > При удалении пакета система предложит удалить коллектор. Необходимо отклонить это действие.
2. Установите пакет libkrb5-3 или libkrb5-26-heimdal.
3. Откройте на редактирование файл `/etc/krb5.conf`.
4. В блоке параметров `libdefaults` замените строки, которые ограничивают допустимые алгоритмы шифрования и подписи:
   - Если вы установили библиотеку MIT Kerberos:
   ```
   default_tgs_enctypes = aes256-cts-hmac-sha1-96 aes128-cts-hmac-sha1-96 aes128-cts-hmac-sha256-128 rc4-hmac
   default_tkt_enctypes = aes256-cts-hmac-sha1-96 aes128-cts-hmac-sha1-96 aes128-cts-hmac-sha256-128 rc4-hmac
   permitted_enctypes = aes256-cts-hmac-sha1-96 aes128-cts-hmac-sha1-96 aes128-cts-hmac-sha256-128 rc4-hmac
   ```
   - Если вы установили библиотеку Heimdal Kerberos:
   ```
   default_etypes = aes256-cts-hmac-sha1-96 aes128-cts-hmac-sha1-96 aes128-cts-hmac-sha256-128 rc4-hmac
   default_tgs_etypes = aes256-cts-hmac-sha1-96 aes128-cts-hmac-sha1-96 aes128-cts-hmac-sha256-128 rc4-hmac
   default_as_etypes = aes256-cts-hmac-sha1-96 aes128-cts-hmac-sha1-96 aes128-cts-hmac-sha256-128 rc4-hmac
   ```
5. Сохраните изменения в файле `krb5.conf`.
