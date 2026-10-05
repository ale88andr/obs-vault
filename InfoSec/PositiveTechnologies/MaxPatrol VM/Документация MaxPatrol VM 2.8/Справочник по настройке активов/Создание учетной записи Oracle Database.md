---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5921890315"
reuse_id: "5924368395"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921890315"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Oracle Database 11, 12, 18, 19, 21, 23: настройка актива / Создание учетной записи Oracle Database"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи Oracle Database

> [!info] Раздел: System.Collections.Hashtable[@{Id=5921890315; ReuseId=5924368395; Title=Создание учетной записи Oracle Database; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle Database 11, 12, 18, 19, 21, 23: настройка актива / Создание учетной записи Oracle Database; Segments=System.Object[]; Index=456}.Id])
> @{Id=5921890315; ReuseId=5924368395; Title=Создание учетной записи Oracle Database; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle Database 11, 12, 18, 19, 21, 23: настройка актива / Создание учетной записи Oracle Database; Segments=System.Object[]; Index=456}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921890315)

---

**Задача.** Чтобы создать учетную запись пользователя СУБД:

1. Откройте интерфейс командной строки актива.
2. Создайте учетную запись для доступа к активу:
   ```
   create user auditor identified by "<Пароль>" default tablespace system quota 0 on system;
   create role auditrole;
   grant connect, auditrole to auditor;
   ```
3. Предоставьте учетной записи права на просмотр таблиц, в которых хранятся данные об активе:
   ```
   grant select on SYS.AUD$ to auditrole;
   grant select on SYS.DEFROLE$ to auditrole;
   grant select on SYS.FGA_LOG$ to auditrole;
   grant select on SYS.JOB$ to auditrole;
   grant select on SYS.LIBRARY$ to auditrole;
   grant select on SYS.LINK$ to auditrole;
   grant select on SYS.OBJ$ to auditrole;
   grant select on SYS.OBJAUTH$ to auditrole;
   grant select on SYS.SCHEDULER$_JOB to auditrole;
   grant select on SYS.SOURCE$ to auditrole;
   grant select on SYS.SYSAUTH$ to auditrole;
   grant select on SYS.USER$ to auditrole;
   ```
4. Если используется Oracle Database версии 12, предоставьте учетной записи права на чтение таблиц AUDIT_UNIFIED_POLICIES и AUDIT_UNIFIED_ENABLED_POLICIES:
   ```
   grant select on SYS.AUDIT_UNIFIED_POLICIES to auditrole;
   grant select on SYS.AUDIT_UNIFIED_ENABLED_POLICIES to auditrole;
   ```
5. Предоставьте учетной записи права на просмотр представлений БД:
   ```
   grant select on sys.all_def_audit_opts to auditrole;
   grant select on sys.all_objects to auditrole;
   grant select on sys.all_source to auditrole;
   grant select on sys.all_tab_privs to auditrole;
   grant select on sys.all_tab_privs_made to auditrole;
   grant select on sys.all_tables to auditrole;
   grant select on sys.audit_actions to auditrole;
   grant select on sys.database_properties to auditrole;
   grant select on sys.dba_audit_policies to auditrole;
   grant select on sys.dba_audit_trail to auditrole;
   grant select on sys.dba_data_files to auditrole;
   grant select on sys.dba_db_links to auditrole;
   grant select on sys.dba_feature_usage_statistics to auditrole;
   grant select on sys.dba_fga_audit_trail to auditrole;
   grant select on sys.dba_free_space to auditrole;
   grant select on sys.dba_jobs to auditrole;
   grant select on sys.dba_hist_active_sess_history to auditrole;
   grant select on sys.dba_libraries to auditrole;
   grant select on sys.dba_obj_audit_opts to auditrole;
   grant select on sys.dba_objects to auditrole;
   grant select on sys.dba_priv_audit_opts to auditrole;
   grant select on sys.dba_profiles to auditrole;
   grant select on sys.dba_proxies to auditrole;
   grant select on sys.dba_registry to auditrole;
   grant select on sys.dba_role_privs to auditrole;
   grant select on sys.dba_roles to auditrole;
   grant select on sys.dba_scheduler_jobs to auditrole;
   grant select on sys.dba_segments to auditrole;
   grant select on sys.dba_source to auditrole;
   grant select on sys.dba_stmt_audit_opts to auditrole;
   grant select on sys.dba_sys_privs to auditrole;
   grant select on sys.dba_tab_privs to auditrole;
   grant select on sys.dba_tablespaces to auditrole;
   grant select on sys.dba_temp_files to auditrole;
   grant select on sys.dba_ts_quotas to auditrole;
   grant select on sys.dba_users to auditrole;
   grant select on sys.dba_users_with_defpwd to auditrole;
   grant select on sys.dba_views to auditrole;
   grant select on sys.product_component_version to auditrole;
   grant select on sys.system_privilege_map to auditrole;
   grant select on sys.user_astatus_map to auditrole;
   grant select on sys.v_$database to auditrole;
   grant select on sys.v_$datafile to auditrole;
   grant select on sys.v_$datafile_header to auditrole;
   grant select on sys.v_$instance to auditrole;
   grant select on sys.v_$license to auditrole;
   grant select on sys.v_$logfile to auditrole;
   grant select on sys.v_$option to auditrole;
   grant select on sys.v_$parameter to auditrole;
   grant select on sys.v_$pwfile_users to auditrole;
   grant select on sys.v_$session to auditrole;
   grant select on sys.v_$sesstat to auditrole;
   grant select on sys.v_$spparameter to auditrole;
   grant select on sys.v_$statname to auditrole;
   grant select on sys.v_$tablespace to auditrole;
   grant select on sys.v_$temp_space_header to auditrole;
   grant select on sys.v_$tempfile to auditrole;
   grant select on sys.v_$version to auditrole;
   grant select on sys.wrh$_active_session_history to auditrole;
   ```
6. Предоставьте учетной записи право на подсчет хеш-сумм:
   ```
   grant execute on sys.dbms_utility to auditrole;
   ```
7. Предоставьте учетной записи право на определение пути к домашнему каталогу Oracle:
   ```
   grant execute on sys.dbms_system to auditrole;
   ```
8. Предоставьте учетной записи право на создание таблиц в БД:
   ```
   grant create table to auditrole;
   ```

> Учетная запись СУБД создана.
