---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "626215691"
reuse_id: "3664778251"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/626215691"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена пароля служебной учетной записи в PostgreSQL"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Смена пароля служебной учетной записи в PostgreSQL

> [!info] Раздел: System.Collections.Hashtable[@{Id=626215691; ReuseId=3664778251; Title=Смена пароля служебной учетной записи в PostgreSQL; Depth=3; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена пароля служебной учетной записи в PostgreSQL; Segments=System.Object[]; Index=76}.Id])
> @{Id=626215691; ReuseId=3664778251; Title=Смена пароля служебной учетной записи в PostgreSQL; Depth=3; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена пароля служебной учетной записи в PostgreSQL; Segments=System.Object[]; Index=76}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/626215691)

---

При развертывании MaxPatrol VM в PostgreSQL создается служебная учетная запись с правами администратора. По умолчанию логин служебной учетной записи — `pt_system`, пароль — `P@ssw0rdP@ssw0rd`.

> [!warning] Внимание
> Не рекомендуется использовать в паролях служебных учетных записей следующие символы: `# \ / ' " ; $`.

**Задача.** Чтобы сменить пароль служебной учетной записи в PostgreSQL на Linux:

1. На сервере с установленной ролью SqlStorage выполните команды:
   ```bash
   docker exec -it $(docker ps | awk '/storage-postgres/ && !/EDR/ {print $NF}') psql -U pt_system -d postgres
   ALTER USER pt_system WITH PASSWORD '<Новый пароль>';
   ```
2. На сервере с установленной ролью Deployer выполните команду:
   ```bash
   deployer instance configure -type SqlStorage
   ```
3. [[Изменение конфигурации роли|Измените конфигурацию]] роли SqlStorage:
   ```
   PgPassword: <Новый пароль>
   ```
4. [[Изменение конфигурации роли|Измените конфигурации]] ролей LogConnector, Observability, Management and Configuration и Core.
   → При изменении конфигурации зависимых ролей пароль, заданный в конфигурации роли SqlStorage, обновится автоматически.
