---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "2761735819"
reuse_id: "12087311371"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2761735819"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Microsoft SQL Server 2008—2019: настройка актива / Создание учетной записи Microsoft SQL Server"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи Microsoft SQL Server

> [!info] Раздел: System.Collections.Hashtable[@{Id=2761735819; ReuseId=12087311371; Title=Создание учетной записи Microsoft SQL Server; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Microsoft SQL Server 2008—2019: настройка актива / Создание учетной записи Microsoft SQL Server; Segments=System.Object[]; Index=449}.Id])
> @{Id=2761735819; ReuseId=12087311371; Title=Создание учетной записи Microsoft SQL Server; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Microsoft SQL Server 2008—2019: настройка актива / Создание учетной записи Microsoft SQL Server; Segments=System.Object[]; Index=449}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2761735819)

---

При создании учетной записи СУБД для проведения аудита требуется на основе учетной записи пользователя Windows создать учетную запись с правом на чтение БД master, model, msdb, tempdb и в каждой из этих БД выдать учетной записи право на просмотр определений объектов.

## Создание учетной записи

**Задача.** Чтобы создать учетную запись СУБД на основе учетной записи пользователя Windows:

1. Запустите Microsoft SQL Server Management Studio.
   → Откроется окно **Connect to Server**.
2. В раскрывающемся списке **Server name** выберите сервер и экземпляр СУБД.
3. Введите данные учетной записи администратора СУБД и нажмите кнопку **Connect**.
   → Откроется окно Microsoft SQL Server Management Studio.
4. В панели **Object Explorer** в контекстном меню узла **<Имя экземпляра СУБД>** → **Security** → **Logins** выберите **New Login**.
   → Откроется окно **Login — New**.
5. Выберите **Windows authentication** и нажмите кнопку **Search**.
   → Откроется окно **Select User or Group**.
6. Нажмите кнопку **Locations**.
7. В открывшемся окне выберите:
   - если используется локальная учетная запись — имя узла;
   - если используется доменная учетная запись — имя домена.
8. Нажмите кнопку **ОК**.
9. В поле **Enter the object name to select** введите логин учетной записи Windows и нажмите кнопку **Check Names**.
10. Нажмите кнопку **ОК**.
11. В окне **Login — New** в раскрывающемся списке **Default database** выберите **master**.
12. В панели **Select a page** выберите **User Mapping**.
13. В списке **User mapped to this login** установите флажки в строках баз данных **master**, **model**, **msdb**, **tempdb**.
14. В списке **Database role membership** установите флажки для ролей **db_datareader** и **public**.
15. В панели **Select a page** выберите **Securables**.
16. Если список в панели **Securables** не содержит имени сервера СУБД, нажмите кнопку **Search**, в открывшемся окне выберете **The server <Имя сервера>** и нажмите кнопку **ОК**.
17. В нижней части окна выберите вкладку **Explicit**.
18. В колонке **Grant** установите флажки в строках **Connect SQL**, **View server state** и **View any definition**.
    > [!note] Примечание
    > При установке флажка **View any definition** учетной записи предоставляется доступ к определениям всех объектов сервера СУБД из таблиц sys.server_permissions, sys.server_principals и sys.sql_logins.
19. Нажмите кнопку **ОК**.

> Учетная запись СУБД создана.

## Выдача прав на просмотр определений объектов БД

Инструкцию требуется выполнить для каждой БД с данными для аудита.

**Задача.** Чтобы выдать учетной записи право на просмотр определений объектов БД:

1. Запустите Microsoft SQL Server Management Studio.
   → Откроется окно **Connect to Server**.
2. В раскрывающемся списке **Server name** выберите сервер и экземпляр СУБД.
3. Введите данные учетной записи администратора СУБД и нажмите кнопку **Connect**.
   → Откроется окно Microsoft SQL Server Management Studio.
4. В панели **Object Explorer** в контекстном меню узла **<Имя экземпляра СУБД>** → **Databases** → **System Databases** → **<Имя БД>** выберите **Properties**.
   → Откроется окно **Databases Properties — <Имя БД>**.
5. В панели **Select a page** выберите **Permissions**.
6. В списке **Users or roles** выберите созданную ранее учетную запись.
7. В нижней части окна выберите вкладку **Explicit**.
8. В колонке **Grant** установите флажок в строке **View definition**.
9. Нажмите кнопку **ОК**.

> Право на просмотр определений объектов БД выдано учетной записи.

## Выдача дополнительных прав

Требуется выдать учетной записи дополнительные права на сбор данных о связанных серверах и трассировках.

**Задача.** Чтобы выдать учетной записи дополнительные права:

1. Запустите Microsoft SQL Server Management Studio.
   → Откроется окно **Connect to Server**.
2. В раскрывающемся списке **Server name** выберите сервер и экземпляр СУБД.
3. Введите данные учетной записи администратора СУБД и нажмите кнопку **Connect**.
   → Откроется окно Microsoft SQL Server Management Studio.
4. В панели **Object Explorer** в контекстном меню узла **<Имя экземпляра СУБД>** → **Security** → **Logins** выберите созданную ранее учетную запись и в контекстном меню выберите **Properties**.
   → Откроется окно **Login Properties — <Имя учетной записи>**.
5. В панели **Select a page** выберите **Server Roles**.
6. В списке **Server roles** установите флажок **setupadmin**.
7. В панели **Select a page** выберите **Securables**.
8. Если список в панели **Securables** не содержит имени сервера СУБД, нажмите **Search**, в открывшемся окне выберите **The server <Имя сервера>** и нажмите **OК**.
9. Выберите вкладку **Explicit**.
10. В колонке **Grant** установите флажок **Alter trace**.
11. Нажмите кнопку **ОК**.

> Учетной записи выданы дополнительные права.

Кроме того, если требуется, вы можете выдать разрешение **Execute** на процедуру xp_loginconfig в свойствах процедуры (панель **Object Explorer** → **master** → **Programmability** → **Stored Procedures** → **Extended Stored Procedures** → **xp_loginconfig** → **Properties** → **Permissions**.
