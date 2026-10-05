---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "2786965003"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2786965003"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы мониторинга сети / Microsoft System Center Configuration Manager (SCCM) 2012—2019: настройка актива / Создание учетной записи Microsoft SQL Server с помощью запроса"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи Microsoft SQL Server с помощью запроса

> [!info] Раздел: System.Collections.Hashtable[@{Id=2786965003; ReuseId=; Title=Создание учетной записи Microsoft SQL Server с помощью запроса; Depth=4; Path=Справочник по настройке активов / Системы мониторинга сети / Microsoft System Center Configuration Manager (SCCM) 2012—2019: настройка актива / Создание учетной записи Microsoft SQL Server с помощью запроса; Segments=System.Object[]; Index=441}.Id])
> @{Id=2786965003; ReuseId=; Title=Создание учетной записи Microsoft SQL Server с помощью запроса; Depth=4; Path=Справочник по настройке активов / Системы мониторинга сети / Microsoft System Center Configuration Manager (SCCM) 2012—2019: настройка актива / Создание учетной записи Microsoft SQL Server с помощью запроса; Segments=System.Object[]; Index=441}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2786965003)

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
   create user [<Домен>\<Логин>] for login [<Домен>\<Логин>]
   grant select on information_schema.tables to [<Домен>\<Логин>]
   grant select on sys.databases to [<Домен>\<Логин>]
   grant select on sys.database_files to [<Домен>\<Логин>]
   grant view server state to [<Домен>\<Логин>]
   grant view definition to [<Домен>\<Логин>]
   use [SCCM];
   create user [<Домен>\<Логин>] for login [<Домен>\<Логин>]
   grant select on SCCM.sys.database_files to [<Домен>\<Логин>]
   grant select on SCCM.dbo.v_R_System_Valid to [<Домен>\<Логин>]
   grant select on SCCM.SCCM_Ext.vex_GS_PROCESSOR to [<Домен>\<Логин>]
   grant select on SCCM.dbo.v_GS_LOGICAL_DISK to [<Домен>\<Логин>]
   grant select on SCCM.dbo.v_GS_COMPUTER_SYSTEM to [<Домен>\<Логин>]
   grant select on SCCM.SCCM_Ext.vex_R_System to [<Домен>\<Логин>]
   grant select on SCCM.dbo.System_DATA to [<Домен>\<Логин>]
   grant select on SCCM.dbo.v_GS_OPERATING_SYSTEM to [<Домен>\<Логин>]
   grant select on SCCM.dbo.v_GS_ADD_REMOVE_PROGRAMS to [<Домен>\<Логин>]
   grant select on SCCM.dbo.v_GS_ADD_REMOVE_PROGRAMS_64 to [<Домен>\<Логин>]
   grant select on SCCM.SCCM_Ext.vex_GS_NETWORK_ADAPTER to [<Домен>\<Логин>]
   --Строка для таблицы Ext.vex_GS_NETWORK_ADAPTER_CONFIGUR
   grant select on SCCM.SCCM_Ext.vex_GS_NETWORK_ADAPTER_CONFIGUR to [<Домен>\<Логин>]
   --Строка для выдачи прав на просмотр определений объектов БД
   grant view definition to [<Домен>\<Логин>]
   ```
6. Если вместо таблицы `Ext.vex_GS_NETWORK_ADAPTER_CONFIGUR` используется `Ext.vex_GS_NETWORK_ADAPTER_CONFIGURATION`, замените в запросе строку:
   ```
   grant select on SCCM.SCCM_Ext.vex_GS_NETWORK_ADAPTER_CONFIGUR to [<Домен>\<Логин>]
   ```
   → на строку:
   ```
   grant select on SCCM.SCCM_Ext.vex_GS_NETWORK_ADAPTER_CONFIGURATION to [<Домен>\<Логин>]
   ```
7. В панели инструментов нажмите кнопку **Execute**.

> Учетная запись СУБД создана.
