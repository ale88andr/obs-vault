---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "2762629259"
reuse_id: "12087312139"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2762629259"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Microsoft SQL Server 2008—2019: настройка актива / Создание учетной записи Microsoft SQL Server с помощью запроса"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи Microsoft SQL Server с помощью запроса

> [!info] Раздел: System.Collections.Hashtable[@{Id=2762629259; ReuseId=12087312139; Title=Создание учетной записи Microsoft SQL Server с помощью запроса; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Microsoft SQL Server 2008—2019: настройка актива / Создание учетной записи Microsoft SQL Server с помощью запроса; Segments=System.Object[]; Index=450}.Id])
> @{Id=2762629259; ReuseId=12087312139; Title=Создание учетной записи Microsoft SQL Server с помощью запроса; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Microsoft SQL Server 2008—2019: настройка актива / Создание учетной записи Microsoft SQL Server с помощью запроса; Segments=System.Object[]; Index=450}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2762629259)

---

**Задача.** Чтобы создать учетную запись СУБД на основе учетной записи пользователя Windows с помощью запроса:

1. Запустите Microsoft SQL Server Management Studio.
   → Откроется окно **Connect to Server**.
2. В раскрывающемся списке **Server name** выберите сервер и экземпляр СУБД.
3. Введите данные учетной записи администратора СУБД и нажмите кнопку **Connect**.
   → Откроется окно Microsoft SQL Server Management Studio.
4. В панели **Object Explorer** в контекстном меню узла **<Имя экземпляра СУБД>** выберите **New Query**.
   → Откроется панель нового запроса.
5. Введите запрос:
   ```
   use [master];
   create login [<Домен>\<Логин>] from windows;
   declare @db_user varchar(300)
   select @db_user =
   'USE [?]
   create user [<Домен>\<Логин>] for login [<Домен>\<Логин>]'
   exec sp_MSforeachdb @db_user
   declare @db_priv varchar(600)
   select @db_priv =
   'USE [?]
   grant select on sys.database_permissions to [<Домен>\<Логин>]
   grant select on sys.database_principals to [<Домен>\<Логин>]
   grant select on sys.database_files to [<Домен>\<Логин>]
   grant select on sys.database_role_members to [<Домен>\<Логин>]
   grant select on sys.all_objects to [<Домен>\<Логин>]
   grant select on sys.triggers to [<Домен>\<Логин>]
   grant view definition to [<Домен>\<Логин>]'
   exec sp_MSforeachdb @db_priv
   grant select on information_schema.tables to [<Домен>\<Логин>]
   grant select on sys.databases to [<Домен>\<Логин>]
   grant select on sys.server_permissions to [<Домен>\<Логин>]
   grant select on sys.sql_logins to [<Домен>\<Логин>]
   grant select on sys.server_principals to [<Домен>\<Логин>]
   grant select on dbo.syscharsets to [<Домен>\<Логин>]
   grant select on sys.database_files to [<Домен>\<Логин>]
   grant select on sys.database_mirroring to [<Домен>\<Логин>]
   grant select on sys.configurations to [<Домен>\<Логин>]
   grant select on sys.servers to [<Домен>\<Логин>]
   grant select on sys.assemblies to [<Домен>\<Логин>]
   grant select on sys.server_role_members to [<Домен>\<Логин>]
   grant select on sys.dm_os_loaded_modules to [<Домен>\<Логин>]
   grant select on dbo.syscharsets to [<Домен>\<Логин>]
   grant view server state to [<Домен>\<Логин>]
   --Выдача прав на просмотр определений объектов БД
   grant view any definition to [<Домен>\<Логин>]
   --Выдача прав на сбор данных о связанных серверах и трассировках
   alter server role setupadmin add member [<Домен>\<Логин>]
   grant alter trace to [<Домен>\<Логин>]
   --Если требуется, выдача прав на процедуру xp_loginconfig
   grant execute on xp_loginconfig to [<Домен>\<Логин>]
   ```
6. В панели инструментов нажмите кнопку **Execute**.

> Учетная запись СУБД создана.
