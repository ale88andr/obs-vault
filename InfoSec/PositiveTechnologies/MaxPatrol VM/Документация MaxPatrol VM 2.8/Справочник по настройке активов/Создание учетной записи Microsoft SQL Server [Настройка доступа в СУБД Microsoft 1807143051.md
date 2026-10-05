---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "1807143051"
reuse_id: "1837581579"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1807143051"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Настройка доступа в СУБД Microsoft SQL Server / Создание учетной записи Microsoft SQL Server"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи Microsoft SQL Server

> [!info] Раздел: System.Collections.Hashtable[@{Id=1807143051; ReuseId=1837581579; Title=Создание учетной записи Microsoft SQL Server; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Настройка доступа в СУБД Microsoft SQL Server / Создание учетной записи Microsoft SQL Server; Segments=System.Object[]; Index=566}.Id])
> @{Id=1807143051; ReuseId=1837581579; Title=Создание учетной записи Microsoft SQL Server; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Настройка доступа в СУБД Microsoft SQL Server / Создание учетной записи Microsoft SQL Server; Segments=System.Object[]; Index=566}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1807143051)

---

**Задача.** Чтобы создать учетную запись пользователя СУБД на основе учетной записи Windows:

1. Запустите Microsoft SQL Server Management Studio.
   → Откроется окно **Connect to Server**.
2. В раскрывающемся списке **Server name** выберите сервер и экземпляр СУБД.
3. Введите данные учетной записи администратора СУБД и нажмите кнопку **Connect**.
   → Откроется окно Microsoft SQL Server Management Studio.
4. В панели **Object Explorer** в контекстном меню узла **<Имя экземпляра СУБД>** → **Security** выберите **New** → **Login**.
5. Выберите **Windows authentication** и нажмите кнопку **Search**.
   → Откроется окно **Select User or Group**.
6. Нажмите **Locations**.
7. В открывшемся окне выберите:
   - если используется локальная учетная запись — имя узла;
   - если используется доменная учетная запись — имя домена.
8. Нажмите **ОК**.
9. В поле **Enter the object name to select** введите логин созданной ранее учетной записи Windows и нажмите кнопку **Check Names**.
10. Нажмите **ОК**.
11. В окне **Login — New** в раскрывающемся списке **Default database** выберите название БД источника.
    > [!note] Примечание
    > Если вы планируете использовать для аутентификации учетную запись SQL Server, нужно выбрать **SQL Server authentication**, ввести и подтвердить пароль, а также снять флажки **Enforce password expiration** и **User must change password at next login**.
12. В левой части окна выберите **User Mapping**.
13. В списке **User mapped to this login** установите флажок в строке с названием БД источника.
14. В списке **Database role membership** установите флажки для ролей **db_datareader** и **public**.
15. Нажмите **ОК**.
