---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "4409293451"
reuse_id: "4415078795"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/4409293451"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Другие активы / Atlassian Confluence 7.13 и выше: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Atlassian Confluence 7.13 и выше: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=4409293451; ReuseId=4415078795; Title=Atlassian Confluence 7.13 и выше: настройка актива; Depth=3; Path=Справочник по настройке активов / Другие активы / Atlassian Confluence 7.13 и выше: настройка актива; Segments=System.Object[]; Index=500}.Id])
> @{Id=4409293451; ReuseId=4415078795; Title=Atlassian Confluence 7.13 и выше: настройка актива; Depth=3; Path=Справочник по настройке активов / Другие активы / Atlassian Confluence 7.13 и выше: настройка актива; Segments=System.Object[]; Index=500}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/4409293451)

---

Настройку актива нужно выполнять от имени учетной записи администратора устройства.

> [!warning] Внимание
> При использовании в IT-инфраструктуре организации межсетевого экрана или других средств для контроля сетевого трафика требуется настроить в них правила, разрешающие трафик в обоих направлениях между узлом актива и узлом MP 10 Collector. Для проведения аудита актива по протоколу SSH используется порт TCP 22.

Для настройки актива необходимо настроить удаленный доступ к СУБД и создать учетную запись СУБД с правом на чтение базы данных актива.

## Настройка СУБД MySQL для сканирования

Данные актива сохраняются в базу данных (по умолчанию confluence) под управлением СУБД MySQL.

**Задача.** Чтобы настроить СУБД MySQL для сканирования:

1. Настройте [[Настройка удаленного доступа в MySQL|удаленный доступ к СУБД]].
2. В интерфейсе терминала запустите консоль MySQL с правами суперпользователя (root):
   ```
   mysql -u root -p
   ```
3. Переключитесь на базу данных confluence:
   ```
   USE confluence;
   ```
4. Создайте учетную запись пользователя с правами удаленного доступа к базе данных:
   ```
   CREATE USER 'ptsiem'@'%' IDENTIFIED BY 'P@ssw0rd';
   ```
   > [!note] Примечание
   > Логин и пароль созданной учетной записи необходимо будет указать при добавлении учетной записи в MaxPatrol VM.
5. Предоставьте учетной записи права на чтение таблиц, в которых хранятся данные об активе:
   ```
   GRANT SELECT ON confluence.BANDANA TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.CONTENT TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.CONTENT_PERM TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.CONTENT_PERM_SET TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.cwd_directory TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.cwd_directory_attribute TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.cwd_directory_operation TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.cwd_group TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.cwd_membership TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.cwd_user TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.SPACES TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.SPACEPERMISSIONS TO 'ptsiem'@'%';
   GRANT SELECT ON confluence.user_mapping TO 'ptsiem'@'%';
   ```
6. Примените внесенные изменения:
   ```
   FLUSH PRIVILEGES;
   ```
7. Завершите сеанс работы с консолью MySQL:
   ```
   EXIT;
   ```

> СУБД MySQL настроена для сканирования.
