---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7370322571"
reuse_id: "7392433931"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7370322571"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Redis 6.2 и выше: настройка актива / Установка пароля для пользователя default"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Установка пароля для пользователя default

> [!info] Раздел: System.Collections.Hashtable[@{Id=7370322571; ReuseId=7392433931; Title=Установка пароля для пользователя default; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Redis 6.2 и выше: настройка актива / Установка пароля для пользователя default; Segments=System.Object[]; Index=470}.Id])
> @{Id=7370322571; ReuseId=7392433931; Title=Установка пароля для пользователя default; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Redis 6.2 и выше: настройка актива / Установка пароля для пользователя default; Segments=System.Object[]; Index=470}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7370322571)

---

**Задача.** Чтобы установить пароль для пользователя default:

1. Подключитесь к Redis от имени учетной записи с правами администратора.
2. Выполните команду:
   ```
   ACL SETUSER default ><Пароль>
   ```
3. Сохраните изменения:
   ```
   ACL SAVE
   ```
