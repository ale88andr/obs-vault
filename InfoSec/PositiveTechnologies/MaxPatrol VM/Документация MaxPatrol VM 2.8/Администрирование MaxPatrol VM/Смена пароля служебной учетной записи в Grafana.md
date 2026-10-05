---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "9797706763"
reuse_id: "9800097035"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/9797706763"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена пароля служебной учетной записи в Grafana"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Смена пароля служебной учетной записи в Grafana

> [!info] Раздел: System.Collections.Hashtable[@{Id=9797706763; ReuseId=9800097035; Title=Смена пароля служебной учетной записи в Grafana; Depth=3; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена пароля служебной учетной записи в Grafana; Segments=System.Object[]; Index=80}.Id])
> @{Id=9797706763; ReuseId=9800097035; Title=Смена пароля служебной учетной записи в Grafana; Depth=3; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена пароля служебной учетной записи в Grafana; Segments=System.Object[]; Index=80}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/9797706763)

---

При развертывании MaxPatrol VM в Grafana создается служебная учетная запись с правами администратора. По умолчанию логин служебной учетной записи — `admin`, пароль — `P@ssw0rd`.

**Задача.** Чтобы сменить пароль служебной учетной записи в Grafana:

1. [[Изменение конфигурации роли|Измените конфигурацию]] роли Observability, указав в качестве значения параметра `GrafanaAdminPassword` новый пароль учетной записи.
2. В веб-интерфейсе Grafana выберите **admin** → **Preferences**.
3. Выберите **Change Password**.
4. В поле **Old password** введите старый пароль учетной записи.
5. В поле **New password** введите новый пароль учетной записи, указанный в параметре `GrafanaAdminPassword`.
6. В поле **Confirm password** повторно введите новый пароль и нажмите **Change password**.
   → Появится сообщение **User password changed**.
