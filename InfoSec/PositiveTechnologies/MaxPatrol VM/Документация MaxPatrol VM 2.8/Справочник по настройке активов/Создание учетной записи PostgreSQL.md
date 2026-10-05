---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5921893131"
reuse_id: "5924583435"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921893131"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / PostgreSQL 9—15: настройка актива / Создание учетной записи PostgreSQL"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи PostgreSQL

> [!info] Раздел: System.Collections.Hashtable[@{Id=5921893131; ReuseId=5924583435; Title=Создание учетной записи PostgreSQL; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / PostgreSQL 9—15: настройка актива / Создание учетной записи PostgreSQL; Segments=System.Object[]; Index=464}.Id])
> @{Id=5921893131; ReuseId=5924583435; Title=Создание учетной записи PostgreSQL; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / PostgreSQL 9—15: настройка актива / Создание учетной записи PostgreSQL; Segments=System.Object[]; Index=464}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921893131)

---

**Задача.** Чтобы создать учетную запись пользователя СУБД:

1. Откройте интерфейс командной строки актива.
2. Создайте учетную запись для доступа к активу:
   ```
   CREATE USER auditor WITH LOGIN PASSWORD '<Пароль>';
   ```
3. Предоставьте учетной записи права на просмотр таблиц, в которых хранятся данные об активе:
   ```
   GRANT SELECT ON information_schema.table_privileges TO auditor;
   GRANT SELECT ON pg_catalog.pg_authid TO auditor;
   GRANT SELECT ON pg_catalog.pg_tablespace TO auditor;
   GRANT SELECT ON pg_catalog.pg_namespace TO auditor;
   GRANT SELECT ON pg_catalog.pg_roles TO auditor;
   GRANT SELECT ON pg_catalog.pg_auth_members TO auditor;
   GRANT SELECT ON pg_catalog.pg_settings TO auditor;
   GRANT SELECT ON pg_catalog.pg_ident_file_mappings TO auditor;
   GRANT SELECT ON pg_catalog.pg_hba_file_rules TO auditor;
   GRANT SELECT ON pg_catalog.pg_extension TO auditor;
   GRANT SELECT ON pg_catalog.pg_policies TO auditor;
   GRANT pg_read_all_settings TO auditor;
   GRANT EXECUTE ON FUNCTION pg_catalog.pg_hba_file_rules TO auditor;
   ```
