---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7434454923"
reuse_id: "7475429259"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7434454923"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Другие активы / JFrog Artifactory 7 и выше: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# JFrog Artifactory 7 и выше: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=7434454923; ReuseId=7475429259; Title=JFrog Artifactory 7 и выше: настройка актива; Depth=3; Path=Справочник по настройке активов / Другие активы / JFrog Artifactory 7 и выше: настройка актива; Segments=System.Object[]; Index=522}.Id])
> @{Id=7434454923; ReuseId=7475429259; Title=JFrog Artifactory 7 и выше: настройка актива; Depth=3; Path=Справочник по настройке активов / Другие активы / JFrog Artifactory 7 и выше: настройка актива; Segments=System.Object[]; Index=522}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7434454923)

---

Для доступа MP 10 Collector к активу используется токен доступа.

**Задача.** Чтобы выпустить токен доступа:

1. Войдите в веб-интерфейс актива от имени учетной записи с правами администратора.
2. Нажмите **Administration** → **User Management** → **Access Tokens**.
3. Нажмите **Generate Token**.
4. В раскрывающемся списке **Token scope** выберите **Admin**.
5. В поле **User name** введите имя пользователя, которое будет использоваться для MP 10 Collector.
6. В раскрывающемся списке **Expiration time** выберите срок действия токена доступа.
7. Нажмите **Generate**.
8. Скопируйте токен доступа и сохраните в текстовый файл.

Токен доступа понадобится при добавлении учетной записи в MaxPatrol VM.
