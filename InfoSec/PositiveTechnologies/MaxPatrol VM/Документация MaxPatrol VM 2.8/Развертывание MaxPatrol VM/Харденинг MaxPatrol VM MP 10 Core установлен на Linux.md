---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "3720894475"
reuse_id: "6192382091"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3720894475"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Харденинг MaxPatrol VM: MP 10 Core установлен на Linux"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Харденинг MaxPatrol VM: MP 10 Core установлен на Linux

> [!info] Раздел: System.Collections.Hashtable[@{Id=3720894475; ReuseId=6192382091; Title=Харденинг MaxPatrol VM: MP 10 Core установлен на Linux; Depth=2; Path=Развертывание MaxPatrol VM / Харденинг MaxPatrol VM: MP 10 Core установлен на Linux; Segments=System.Object[]; Index=39}.Id])
> @{Id=3720894475; ReuseId=6192382091; Title=Харденинг MaxPatrol VM: MP 10 Core установлен на Linux; Depth=2; Path=Развертывание MaxPatrol VM / Харденинг MaxPatrol VM: MP 10 Core установлен на Linux; Segments=System.Object[]; Index=39}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3720894475)

---

**Задача.** Чтобы настроить MaxPatrol VM:

1. На серверах под управлением Linux разрешите удаленный доступ по протоколу SSH только с рабочих станций администраторов:
   ```
   iptables -A INPUT -i <Название внешнего сетевого интерфейса> -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
   iptables -A INPUT -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций администраторов> -p tcp -m tcp --dport 22 -m conntrack --ctstate NEW -m comment --comment "SSH admin access" -j ACCEPT
   ```
   > [!note] Примечание
   > IP-адреса рабочих станций необходимо перечислять через запятую. Также вы можете указать маску подсети, где находятся рабочие станции, в формате CIDR. Например, `-s 198.51.100.0,198.51.100.1,192.0.2.0/24`.
2. На сервере MP 10 Core разрешите доступ к веб-интерфейсу системы с рабочих станций пользователей:
   ```
   iptables -F DOCKER-USER
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций пользователей> -p tcp -m conntrack --ctstate NEW --ctorigdstport 80 -m comment --comment "Web user access" -j ACCEPT
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций пользователей> -p tcp -m conntrack --ctstate NEW --ctorigdstport 443 -m comment --comment "Web user access" -j ACCEPT
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций пользователей> -p tcp --dport 3334 -m conntrack --ctstate NEW -m comment --comment "Web user access" -j ACCEPT
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций пользователей> -p tcp -m conntrack --ctstate NEW --ctorigdstport 8091 -m comment --comment "Web user access" -j ACCEPT
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций пользователей> -p tcp -m conntrack --ctstate NEW --ctorigdstport 8190 -m comment --comment "Web user access" -j ACCEPT
   ```
3. На серверах с установленной ролью - разрешите удаленный доступ к веб-интерфейсу Grafana:
   ```
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций администраторов> -p tcp -m conntrack --ctstate NEW --ctorigdstport 9002 -m comment --comment "Grafana access" -j ACCEPT
   ```
4. На сервере c установленной ролью Deployer разрешите входящие соединения от коллекторов:
   ```
   iptables -A INPUT -i <Название внешнего сетевого интерфейса> -s <IP-адрес сервера MP 10 Collector> -p tcp -m multiport --dports 4505,4506,9035,4318 -m conntrack --ctstate NEW -m comment --comment "From MP 10 Collector to Deployer" -j ACCEPT
   ```
5. В файл `custom.env`, расположенный в каталоге `/var/lib/deployed-roles/<Идентификатор приложения Management and Configuration>/<Название экземпляра роли SqlStorage>/images/storage-pgadmin/config/`, добавьте параметры:
   ```
   PGADMIN_CONFIG_MASTER_PASSWORD_REQUIRED=True
   PGADMIN_DEFAULT_PASSWORD=<Пароль PGAdmin>
   ```
6. Перезапустите службу pgAdmin:
   ```bash
   cd /var/lib/deployed-roles/<Идентификатор приложения Management and Configuration>/<Название экземпляра роли SqlStorage>/images/storage-pgadmin
   docker-compose up -d
   ```
7. Разрешите доступ к панели управления pgAdmin с рабочих станций администраторов:
   ```
   iptables -I DOCKER-USER 1 -i <Название внешнего сетевого интерфейса> -s <IP-адреса рабочих станций администраторов> -p tcp -m conntrack --ctstate NEW --ctorigdstport 9001 -j ACCEPT -m comment --comment "pgAdmin access"
   ```
8. Заблокируйте доступ к панели управления pgAdmin для всех новых соединений:
   ```
   iptables -I DOCKER-USER 2 -i <Название внешнего сетевого интерфейса> -p tcp -m conntrack --ctstate NEW --ctorigdstport 9001 -j DROP
   ```
9. Разрешите входящие соединения от коллекторов:
   ```
   iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -s <IP-адрес сервера MP 10 Collector> -p tcp -m tcp --dport 5671 -m conntrack --ctstate NEW -m comment --comment "From MP 10 Collector to MP 10 Core" -j ACCEPT
   ```
10. На серверах под управлением Linux заблокируйте все входящие соединения, кроме разрешенных:
    ```
    iptables -A INPUT -i <Название внешнего сетевого интерфейса> -j DROP
    iptables -A DOCKER-USER -i <Название внешнего сетевого интерфейса> -j REJECT
    ```
11. Для сохранения созданных правил на серверах под управлением Debian установите пакет iptables-persistent:
    ```bash
    apt-get install iptables-persistent
    ```
    > [!note] Примечание
    > Порядок сохранения правил на серверах под управлением Astra Linux описан в [справочном центре производителя операционной системы](https://wiki.astralinux.ru).
12. Сохраните правила межсетевого экрана:
    ```
    netfilter-persistent save
    ```
13. На серверах коллекторов под управлением Microsoft Windows удалите все правила удаленного доступа по протоколу RDP:
    ```
    netsh advfirewall firewall delete rule name=all protocol=tcp localport=3389
    netsh advfirewall firewall delete rule name=all protocol=udp localport=3389
    ```
14. Разрешите удаленный доступ по протоколу RDP только с рабочих станций администраторов:
    ```
    netsh advfirewall firewall add rule name="Allow RDP TCP in" dir=in action=allow protocol=tcp localport=3389 remoteip=<IP-адреса рабочих станций администраторов>
    netsh advfirewall firewall add rule name="Allow RDP UDP in" dir=in action=allow protocol=udp localport=3389 remoteip=<IP-адреса рабочих станций администраторов>
    ```
    > [!note] Примечание
    > IP-адреса рабочих станций необходимо перечислять через запятую. Также вы можете указать маску подсети, где находятся рабочие станции, в формате CIDR. Например, `remoteip=198.51.100.0,198.51.100.1,192.0.2.0/24`.
15. Cмените пароли [служебных учетных записей](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/626208779) в MaxPatrol VM.
16. На серверах под управлением Microsoft Windows убедитесь, что пароли для входа в операционную систему соответствуют требованиям к сложности, установленным в организации.
17. [[Установка доверенного сертификата для сайта MaxPatrol VM|Установите собственные доверенные сертификаты]] для MP 10 Core, PT MC и RabbitMQ.
18. На серверах под управлением Linux для каждого администратора MaxPatrol VM создайте отдельную учетную запись:
    ```
    adduser <Логин администратора>
    ```
19. На рабочих станциях администраторов MaxPatrol VM сгенерируйте ключевую пару.
    > [!note] Примечание
    > Для генерации ключевой пары на Linux вы можете использовать утилиту ssh-keygen, на Microsoft Windows — PuTTygen.
20. На серверах под управлением Linux добавьте открытый ключ в файл `/home/<Логин администратора>/.ssh/authorized_keys`.
21. В файле `/etc/ssh/sshd_config` раскомментируйте и измените значения параметров (разрешите вход только с помощью SSH-ключей):
    ```
    PubkeyAuthentication yes
    RhostsRSAAuthentication no
    HostbasedAuthentication no
    PermitEmptyPasswords no
    PasswordAuthentication no
    ```
22. В файле `/etc/sudoers` измените значение параметра:
    ```xml
    <Логин администратора> ALL=(ALL) ALL
    ```
23. Для каждого пользователя MaxPatrol VM создайте отдельную учетную запись.
24. Смените пароль учетной записи Administrator.

## Подключение к локальному серверу обновлений

Если локальный сервер обновлений установлен на сервере, отличном от сервера PT MC, на локальном сервере обновлений необходимо разрешить входящие соединения от PT MC.

**Задача.** Чтобы настроить подключение к локальному серверу обновлений,

1. на локальном сервере обновлений выполните команду:
   ```
   iptables -A INPUT -i <Название внешнего сетевого интерфейса> -s <IP-адрес сервера PT MC> -p tcp -m multiport --dports 8553,8743 -m conntrack --ctstate NEW -m comment --comment "From PT MC to LUS" -j ACCEPT
   ```
