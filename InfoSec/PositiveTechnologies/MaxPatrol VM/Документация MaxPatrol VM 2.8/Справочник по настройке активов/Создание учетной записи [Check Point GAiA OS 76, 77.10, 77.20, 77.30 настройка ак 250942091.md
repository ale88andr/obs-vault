---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "250942091"
reuse_id: "3549807371"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/250942091"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 76, 77.10, 77.20, 77.30: настройка актива / Создание учетной записи"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи

> [!info] Раздел: System.Collections.Hashtable[@{Id=250942091; ReuseId=3549807371; Title=Создание учетной записи; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 76, 77.10, 77.20, 77.30: настройка актива / Создание учетной записи; Segments=System.Object[]; Index=339}.Id])
> @{Id=250942091; ReuseId=3549807371; Title=Создание учетной записи; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 76, 77.10, 77.20, 77.30: настройка актива / Создание учетной записи; Segments=System.Object[]; Index=339}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/250942091)

---

**Задача.** Чтобы создать учетную запись администратора сервера управления:

1. Запустите SmartConsole.
2. В открывшемся окне введите данные учетной записи и IP-адрес для подключения к серверу управления. Нажмите кнопку **Login**.
3. Выберите вкладку **Users and Administrators**.
4. В контекстном меню узла **Administrators** выберите **New Administrator**.
5. В открывшемся окне **Administrator Properties** в поле **User Name** введите имя учетной записи.
   ![[133163531.png]]

   *Настройка учетной записи*
6. Справа от поля **Permission Profile** нажмите кнопку **New**.
7. В открывшемся окне **Permission Profile Properties** укажите параметры профиля.
   ![[133165451.png]]

   *Настройка профиля учетной записи*
8. В левой части окна **Administrator Properties** выберите узел **Authentication**.
9. В раскрывающемся списке **Authentication Scheme** выберите способ проверки **Check Point Password**.
10. В поле **Password** введите пароль для входа, в поле **Confirm Password** подтвердите его.
    ![[133167371.png]]

    *Настройка способа проверки учетной записи*
11. Нажмите кнопку **ОК**.

> Учетная запись администратора создана.
