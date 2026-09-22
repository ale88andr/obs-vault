# admin

**admin certificate create** - Издать или перевыпустить сертификат локального администратора admin.

**admin certificate delete** - Удалить сертификат локального администратора admin.

**admin clear-auth-warning** - Сбросить счётчик попыток подбора пароля.

**admin escape** - Выйти в системную командную оболочку shell. Внимание! Команда предназначена для использования опытными администраторами в целях отладки. ИнфоТеКС не гарантирует нормальную работу ViPNet Coordinator HW в случае некорректных действий администратора в системной командной оболочке.

**admin passwd** - Изменить пароль локального администратора admin.

**admin remove keys** - Удалить ключи и справочники ViPNet Coordinator HW.

**admin show check integrity status** - Просмотреть информацию о последней проверке целостности файлов:  время последней проверки; -  результаты проверки.

**admin ssh reset-key {host АДРЕС | id ИДЕНТИФИКАТОР}** - Удалить отпечатки SSH-ключей, хранящиеся локально на ViPNet Coordinator HW.

**admin ssh reset-key local** - Перевыпустить ключи локального SSH-сервера ViPNet Coordinator HW.

**admin ssh show-key {host АДРЕС | id ИДЕНТИФИКАТОР | local}** - Просмотреть информацию об SSH-ключах, хранящихся локально на ViPNet Coordinator HW.

**admin upgrade software** - Обновить ПО ViPNet Coordinator HW вручную с USB-носителя или CD-диска.

# alg

**alg module ПРИКЛАДНОЙ_ПРОТОКОЛ direct-media {on | off}** - Настроить передачу медиапотоков между клиентами при обработке прикладных протоколов H.323 или SIP.

**alg module h323 direct-h245 {on | off}** - Настроить согласование параметров связи между клиентами при обработке сигнального протокола H.245.

**alg module ПРИКЛАДНОЙ_ПРОТОКОЛ process off** - Выключить обработку прикладного протокола DNS , FTP , H.323 , SCCP или SIP.

**alg module ПРИКЛАДНОЙ_ПРОТОКОЛ process {tcp | udp} ПОРТЫ on** - Включить обработку прикладного протокола DNS , FTP , H.323 , SCCP , SIP , или изменить параметры обработки этого протокола.

**alg show** - Просмотреть текущие параметры обработки прикладных протоколов DNS , FTP , H.323 , SCCP , SIP.

# failover

**failover config edit** - Редактировать конфигурационный файл системы защиты от сбоев failover.ini.

**failover config mode {single | cluster}** - Задать режим работы системы защиты от сбоев.

**failover show active-mac-address** - Просмотреть доступность активного узла кластера со стороны пассивного узла.

**failover show config** - Просмотреть конфигурационный файл системы защиты от сбоев failover.ini.

**failover show info** - Просмотреть текущее состояние системы защиты от сбоев.

**failover show sync-connections** - Просмотреть количество сетевых соединений в кеше узла кластера.

**failover start [{active | passive}]** - Запустить службу системы защиты от сбоев failoverd.

**failover stop** - Завершить работу службы системы защиты от сбоев failoverd.

**failover view НАЧАЛО КОНЕЦ** - Просмотреть журнал переключений кластера за заданный период времени.

# firewall

**firewall ТИП add [НОМЕР] [rule ИМЯ] src АДРЕС_ОТПРАВИТЕЛЯ dst АДРЕС_ПОЛУЧАТЕЛЯ [ТРАНСПОРТНЫЙ_ПРОТОКОЛ] [dpiapp ПРИЛОЖЕНИЕ] [dpiprotocol ПРИКЛАДНОЙ_ПРОТОКОЛ] [dpigroup ГРУППА_ПРИЛОЖЕНИЙ] [dnuser ПОЛЬЗОВАТЕЛЬ] [РАСПИСАНИЕ] [log {on | off}] ДЕЙСТВИЕ** - Создать сетевой фильтр или правило трансляции адресов.

**firewall ТИП add name @ИМЯ СОСТАВ [exclude ИСКЛЮЧЕНИЯ]** - Создать группу объектов заданного типа.

**firewall ТИП change append [НОМЕР] [rule ИМЯ] src АДРЕС_ОТПРАВИТЕЛЯ dst АДРЕС_ПОЛУЧАТЕЛЯ [ТРАНСПОРТНЫЙ_ПРОТОКОЛ] [dpiapp ПРИЛОЖЕНИЕ] [dpiprotocol ПРИКЛАДНОЙ_ПРОТОКОЛ] [dpigroup ГРУППА_ПРИЛОЖЕНИЙ] [dnuser ПОЛЬЗОВАТЕЛЬ] [РАСПИСАНИЕ] log {on | off}** - Добавить адрес отправителя, адрес получателя, протокол или расписание в сетевой фильтр или правило трансляции адресов.

**firewall ТИП delete ПАРАМЕТРЫ** - Удалить сетевой фильтр или правило трансляции адресов.

**firewall ТИП move rule ТЕКУЩИЙ_НОМЕР to НОВЫЙ_НОМЕР** - Изменить порядковый номер (приоритет) сетевого фильтра или правила трансляции адресов в таблице.

**firewall object delete @ИМЯ** - Удалить группу объектов с заданным именем.

**firewall object show** - Просмотреть все группы объектов.

**firewall rules log-blocked** - Включить регистрацию IP-пакетов при срабатывании всех настраиваемых фильтров с действиями блокировать ( drop ) и отклонить ( reject ).

**firewall rules show [{pass | drop | reject}]** - Просмотреть все сетевые фильтры и правила трансляции адресов, заданные в ViPNet Coordinator HW.

**firewall settings ПАРАМЕТР ЗНАЧЕНИЕ** - Задать параметры межсетевого экрана ViPNet Coordinator HW.

**firewall settings show** - Просмотреть параметры межсетевого экрана ViPNet Coordinator HW.

**firewall ТИП show [ПАРАМЕТРЫ]** - Просмотреть конкретные группы объектов, сетевые фильтры заданного типа, а также правила трансляции адресов.

# inet

**inet bgp {on | off}** - Включить или выключить BGP-маршрутизацию.

**inet bgp as-path-filter add NAME** - Создать AS-path-фильтр.

**inet bgp as-path-filter NAME clear seq NUM** - Удалить регулярное выражение из AS-path-фильтра.

**inet bgp as-path-filter delete NAME** - Удалить AS-path-фильтр.

**inet bgp as-path-filter NAME [seq NUM] {permit | deny}** - Добавить или изменить регулярное выражение в AS-path-фильтре.

**inet bgp clear {all | IP-ADDRESS}** - Перезапустить BGP-сессии со всеми соседями или с выбранным соседом.

**inet bgp community-list add NAME** - Создать комьюнити-лист.

**inet bgp community-list NAME clear seq NUM** - Удалить правило из комьюнити-листа.

**inet bgp community-list delete NAME** - Удалить комьюнити-лист.

**inet community-list NAME [seq NUM] {permit | deny} COMMUNITY-LINE** - Добавить или изменить правило в комьюнити-листе.

**inet bgp neighbor add IP-ADDRESS remote-as AS-NUMBER [port PORTNUMBER]** - Добавить соседа.

**inet bgp neighbor IP-ADDRESS delete** - Удалить соседа.

**inet bgp neighbor IP-ADDRESS remove advertise-map** - Выключить условное анонсирование для выбранного соседа.

**inet bgp neighbor IP-АДРЕС remove AS-path-filter {in | out}** - Отключить AS-path-фильтр для соседа.

**inet bgp neighbor IP-ADDRESS remove password** - Выключить аутентификацию соседа и удалить пароль.

**inet bgp neighbor IP-АДРЕС remove prefix-list {in | out}** - Отключить префикс-лист для соседа.

**inet bgp neighbor IP-АДРЕС remove route-map {in | out}** - Отключить карту маршрутов для соседа.

**inet bgp neighbor IP-ADDRESS set advertise-map AD_NAME {exist-map | non-exist-map} MAP_NAME** - Включить условное анонсирование для выбранного соседа.

**inet bgp neighbor IP-АДРЕС set AS-path-filter NAME {in | out}** - Применить AS-path-фильтр к входящим или исходящим маршрутам, которыми обмениваются маршрутизатор и сосед.

**inet bgp neighbor IP-ADDRESS set default-originate {on | off}** - Включить или выключить анонсирование маршрута по умолчанию для соседа.

**inet bgp neighbor IP-ADDRESS set ebgp-multihop {TTL | off}** - Настроить eBGP-multihop для соседа.

**inet bgp neighbor IP-ADDRESS set next-hop-self {on | off}** - Включить или выключить замену next-hop для соседа на собственный адрес.

**inet bgp neighbor IP-ADDRESS set password** - Включить аутентификацию с соседом и задать пароль.

**inet bgp neighbor IP-ADDRESS set port PORTNUMBER** - Задать номера TCP-порта соседа.

**inet bgp neighbor IP-АДРЕС set prefix-list NAME {in | out}** - Назначить префикс-лист для фильтрации входящих или исходящих маршрутов, которыми обмениваются маршрутизатор и сосед.

**inet bgp neighbor IP-ADDRESS set remote-as AS-NUMBER** - Изменить номер автономной системы соседа.

**inet bgp neighbor IP-ADDRESS set route-map NAME {in|out}** - Применить карту маршрутов к входящим или исходящим маршрутам, которыми обмениваются маршрутизатор и сосед.

**inet bgp neighbor IP-ADDRESS set ttl-security {HOPS | off}** - Настроить TTL Security для соседа.

**inet bgp network add SUBNET netmask NETMASK** - Добавить анонсируемую подсеть.

**inet bgp network delete SUBNET netmask NETMASK** - Удалить анонсируемую посеть.

**inet bgp network redistribute {connected | static} {on | off}** - Включить или выключить перераспределение маршрутов.

**inet bgp network redistribute {connected | static} route-map {name NAME | clear}** - Назначить или удалить карту маршрутов, применяемую к распространяемым маршрутам.

**inet bgp router as AS-NUMBER** - Задать или изменить номер автономной системы маршрутизатора.

**inet bgp router bestpath AS-path-relax {on | off}** - Включить или выключить проверку AS-path при встраивании эквивалентных маршрутов (AS-path relax).

**inet bgp router bestpath bandwith {ignore | skip | min | default}** - Задать режим обработки эквивалентных (multipath) маршрутов с расширенным комьюнити Link bandwidth.

**inet bgp router conditional-advertisement-timer ПЕРИОД** - Задать период опроса BGP-таблицы для условного анонсирования.

**inet bgp router id {A.B.C.D | auto}** - Задать идентификатор маршрутизатора (router id) вручную или назначить его автоматически.

**inet bgp router reset** - Сбросить настройки маршрутизатора, в том числе номер автономной системы, соседей и анонсированные подсети.

**inet bgp route-reflector client add IP-ADDRESS** - Добавить iBGP-соседа в список RR-клиентов.

**inet bgp route-reflector client delete IP-АДРЕС** - Удалить iBGP-соседа из списка RR-клиентов.

**inet bgp route-reflector cluster-id {A.B.C.D | auto}** - Задать идентификатор RR-кластера вручную или назначить его автоматически.

**inet bgp route-reflector outbound-policy {on | off}** - Разрешить или запретить применение исходящих карт маршрутов к отраженным маршрутам.

**inet bgp show [ПОДСЕТЬ]** - Просмотреть всю BGP-таблицу или маршруты, через которые доступна выбранная подсеть.

**inet bgp show AS-path-filter [NAME]** - Просмотреть список AS-path-фильтров или состав выбранного AS-path-фильтра.

**inet bgp show bestpath** - Просмотреть настройки выбора лучшего маршрута.

**inet bgp show community-list [NAME]** - Просмотреть список комьюнити-листов или состав выбранного комьюнити-листа.

**inet bgp show neighbors [IP-АДРЕС [{advertised | learned}]]** - Просмотреть сведения о всех соседях или выбранном соседе.

**inet bgp show neighbors filters [IP-АДРЕС]** - Просмотреть список AS-Path-фильтров, префикс-листов и карт маршрутов всех соседей или только выбранного соседа.

**inet bgp show neighbors list** - Просмотреть список соседей и атрибутов доступа к ним.

**inet bgp show network** - Просмотреть настройки анонсирования подсетей и перераспределения маршрутов.

**inet bgp show route-reflector** - Просмотреть настройки отражения маршрутов (Route Reflector).

**inet bgp show summary** - Просмотреть список сессий и состояние каждой из них.

**inet bonding add НОМЕР mode РЕЖИМ slaves ИНТЕРФЕЙС_1 [ИНТЕРФЕЙС_2] ... [ИНТЕРФЕЙС_N]** - Создать агрегированный интерфейс.

**inet bonding delete НОМЕР** - Удалить агрегированный интерфейс.

**inet clear mac-address-table** - Очистить ARP-таблицу (таблицу преобразования IP-адресов в MAC-адреса).

**inet dgd configuration default** - Сбросить параметры службы DGD на значения по умолчанию:  interval-time - частота проверки состояния шлюза; -  response-time - время ожидания ответа от тестового IP-адреса шлюза; -  retries-count - число неудачных проверок шлюза, после которого шлюз считается нерабочим; -  syslog-level - максимальный уровень событий DGD, записываемых в системный журнал.

**inet dgd configuration interval-time ИНТЕРВАЛ** - Задать частоту проверки соединения с тестовым IP-адресом шлюза (см. inet dgd next-hop add).

**inet dgd configuration response-time ВРЕМЯ** - Задать время ожидания ответа от тестового IP-адреса шлюза (см. inet dgd next-hop add).

**inet dgd configuration retries-count ЧИСЛО_ПРОВЕРОК** - Задать число проверок IP-адреса шлюза, при достижении которого шлюз считается нерабочим.

**inet dgd configuration syslog-level УРОВЕНЬ_ВАЖНОСТИ** - Изменить уровень важности событий службы DGD, записываемых в системный журнал.

**inet dgd next-hop add ИМЯ_ШЛЮЗА {address АДРЕС_ШЛЮЗА | interface ИНТЕРФЕЙС} [test-address ТЕСТОВЫЙ_АДРЕС] {check | no-check} {icmp | tcp80 | tcp443}** - Добавить проверку состояния шлюза с помощью отправки запросов на тестовый IP-адрес или IP-адрес шлюза (если тестовый IP-адрес не задан).

**inet dgd next-hop delete ИМЯ_ШЛЮЗА** - Удалить ранее заданную проверку состояния шлюза (см. inet dgd next-hop add).

**inet dgd mode {on | off}** - Включить или выключить автоматический запуск службы DGD при загрузке ViPNet Coordinator HW.

**inet dgd rule add ИМЯ_ПРАВИЛА action-priority ПРИОРИТЕТ service {log command ТЕКСТ_СООБЩЕНИЯ | route command policy active {ИМЯ_ПОЛИТИКИ | default}}** - Задать действие, которое будет выполнено при совпадении всех состояний шлюзов, заданных командой inet dgd rule add match-next-hop.

**inet dgd rule add ИМЯ_ПРАВИЛА match-next-hop ИМЯ_ШЛЮЗА {up | down}** - Добавить правило проверки состояния шлюзов, заданных командой inet dgd next-hop add.

**inet dgd rule clear ИМЯ_ПРАВИЛА** - Удалить все проверки и действия из ранее заданных правил шлюзов.

**inet dgd rule delete ИМЯ_ПРАВИЛА action-priority ПРИОРИТЕТ** - Удалить действие, ранее заданное командой inet dgd rule add action-priority.

**inet dgd rule delete ИМЯ_ПРАВИЛА match-next-hop ИМЯ_ШЛЮЗА {up | down}** - Удалить шлюз из правила проверки состояния шлюзов, заданного командой inet dgd next-hop add.

**inet dhcp client route-default-metric 1-255** - Изменить значение метрики по умолчанию для маршрутов, поступающих от DHCP-сервера. Эта метрика будет присваиваться маршрутам DHCP-сервера, если для сетевого интерфейса, на который они поступили, не задана специфичная метрика.

**inet dhcp client route-distance АДМИНИСТРАТИВНАЯ_ДИСТАНЦИЯ [default-route АДМИНИСТРАТИВНАЯ_ДИСТАНЦИЯ]** - Задать административную дистанцию маршрутам, поступающим от DHCP-сервера (с использованием DHCP-протокола).

**inet dhcp relay [НОМЕР_КОПИИ] add backup-interface ИНТЕРФЕЙС server АДРЕС** - Добавить сетевой интерфейс, через который служба DHCP-relay будет связываться с запасным DHCP-сервером.

**inet dhcp relay [НОМЕР_КОПИИ] add external-interface ИНТЕРФЕЙС server АДРЕС** - Добавить сетевой интерфейс, через который служба DHCP-relay будет связываться с внешним DHCP-сервером.

**inet dhcp relay [НОМЕР_КОПИИ] add listen-interface ИНТЕРФЕЙС** - Добавить сетевой интерфейс в список интерфейсов, принимающих запросы от DHCP-клиентов для их последующей ретрансляции на внешний DHCP-сервер.

**inet dhcp relay [НОМЕР_КОПИИ] delete backup-interface ИНТЕРФЕЙС server АДРЕС** - Удалить сетевой интерфейс, через который служба DHCP-relay связывается с запасным DHCP-сервером.

**inet dhcp relay [НОМЕР_КОПИИ] delete external-interface ИНТЕРФЕЙС server АДРЕС** - Удалить сетевой интерфейс, через который служба DHCP-relay связывается с внешним DHCP-сервером.

**inet dhcp relay [НОМЕР_КОПИИ] delete listen-interface ИНТЕРФЕЙС** - Удалить сетевой интерфейс из списка интерфейсов, принимающих запросы от DHCP-клиентов для их последующей ретрансляции на внешний DHCP-сервер.

**inet dhcp relay mode {on | off}** - Включить или выключить автоматический запуск службы DHCP-relay при загрузке ViPNet Coordinator HW.

**inet dhcp relay [НОМЕР_КОПИИ] reset** - Сбросить настройки службы DHCP-relay, включая настройки автоматического запуска при загрузке ViPNet Coordinator HW, и завершить её работу.

**inet dhcp relay [НОМЕР_КОПИИ] start** - Запустить службу DHCP-relay.

**inet dhcp relay [НОМЕР_КОПИИ] stop** - Завершить работу службы DHCP-relay.

**inet dhcp server add default-lease-time ВРЕМЯ [interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА]** - Задать значение по умолчанию времени аренды (лизинга) IP-адресов, выделяемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay). Это значение используется клиентами DHCP-сервера, которые не запрашивают определенное время аренды IP-адресов.

**inet dhcp server add dns IP-АДРЕС [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Задать IP-адрес DNS-сервера для передачи DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add domain ИМЯ_ДОМЕНА [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Задать имя домена для передачи DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add host ИМЯ_УЗЛА hardware MAC-АДРЕС address IP-АДРЕС {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА}** - Зарезервировать в DHCP-сервере IP-адрес сетевого узла с заданными доменным именем и MAC-адресом. Информацию об этом DHCP-сервер передает своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add interface ИНТЕРФЕЙС** - Задать рабочий интерфейс DHCP-сервера.

**inet dhcp server add max-lease-time ВРЕМЯ [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Задать время аренды (лизинга) IP-адресов, выделяемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add ntp IP-АДРЕС [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Задать IP-адрес NTP-сервера для передачи DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add option НОМЕР {ip | ascii | hex} ЗНАЧЕНИЕ [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Добавить опцию DHCP-сервера в соответствии с RFC 2132.

**inet dhcp server add range НАЧАЛО_ДИАПАЗОНА КОНЕЦ_ДИАПАЗОНА {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА}** - Задать диапазон IP-адресов, выделяемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add relay-interface ИНТЕРФЕЙС remote subnet ПОДСЕТЬ mask МАСКА** - Задать сетевой интерфейс DHCP-сервера, на котором он будет обрабатывать запросы от агента DHCP-relay, работающего в удаленной подсети.

**inet dhcp server add router IP-АДРЕС {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}** - Задать IP-адрес шлюза по умолчанию для передачи DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add subnet-mask МАСКА_ПОДСЕТИ {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}** - Задать маску подсети для передачи DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add tftp {ИМЯ_СЕРВЕРА | АДРЕС_СЕРВЕРА} [file ПУТЬ_К_ФАЙЛУ] [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Задать IP-адрес или имя TFTP-сервера, а также имя передаваемого файла. Данная информация передается DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add voip АДРЕС_СЕРВЕРА [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Задать IP-адрес TFTP-сервера Cisco VoIP. Данная информация передается DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server add wins АДРЕС_СЕРВЕРА [interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА]** - Добавить адрес WINS-сервера в список адресов, передаваемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete default-lease-time {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}** - Удалить значение по умолчанию времени аренды (лизинга) IP-адресов, выделяемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete dns IP-АДРЕС [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить IP-адрес DNS-сервера, передаваемый DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete domain [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить доменное имя, передаваемое DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete host ИМЯ_УЗЛА hardware MAC-АДРЕС address IP-АДРЕС {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА}** - Удалить зарезервированный DHCP-сервером IP-адрес сетевого узла с заданными доменным именем и MAC-адресом.

**inet dhcp server delete interface ИНТЕРФЕЙС** - Удалить рабочий интерфейс DHCP-сервера.

**inet dhcp server delete max-lease-time [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить ранее заданное время аренды (лизинга) IP-адресов, выделяемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete ntp IP-АДРЕС [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить IP-адрес NTP-сервера, передаваемый DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete option НОМЕР [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить опцию DHCP-сервера в соответствии с RFC 2132.

**inet dhcp server delete range НАЧАЛО_ДИАПАЗОНА КОНЕЦ_ДИАПАЗОНА {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА}** - Удалить диапазон IP-адресов, выделяемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete relay-interface ИНТЕРФЕЙС remote subnet ПОДСЕТЬ mask МАСКА** - Удалить сетевой интерфейс DHCP-сервера, на котором обрабатываются запросы от агента DHCP-relay, работающего в удаленной подсети.

**inet dhcp server delete router АДРЕС {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}** - Удалить IP-адрес шлюза по умолчанию, передаваемый DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete subnet-mask {interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}** - Удалить маску подсети, передаваемую DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete tftp [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить настройки TFTP-сервера, передаваемые DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete voip АДРЕС_СЕРВЕРА [{interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА}]** - Удалить IP-адрес TFTP-сервера Cisco VoIP. Данная информация передается DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server delete wins АДРЕС_СЕРВЕРА [interface ИНТЕРФЕЙС | remote subnet ПОДСЕТЬ mask МАСКА | host ИМЯ_УЗЛА]** - Удалить адрес WINS-сервера из списка адресов, передаваемых DHCP-сервером своим клиентам (либо клиентам удаленной подсети при использовании стороннего DHCP relay).

**inet dhcp server mode {on | off}** - Включить или выключить автоматический запуск DHCP-сервера при загрузке ViPNet Coordinator HW.

**inet dhcp server reset** - Сбросить настройки DHCP-сервера, включая настройки автоматического запуска при загрузке ViPNet Coordinator HW, и завершить работу DHCP-сервера.

**inet dhcp server start** - Запустить DHCP-сервер.

**inet dhcp server stop** - Завершить работу DHCP-сервера.

**inet dns clients add {АДРЕС[/ДЛИНА_МАСКИ] | any}** - Добавить адрес или подсеть в список клиентов DNS-сервера, развернутого на ViPNet Coordinator HW.

**inet dns clients delete {АДРЕС[/ДЛИНА_МАСКИ] | any}** - Удалить адрес или подсеть из списка клиентов DNS-сервера, развернутого на ViPNet Coordinator HW.

**inet dns clients list** - Просмотреть список DNS-клиентов, которым разрешена передача запросов DNS-серверу.

**inet dns forwarders add АДРЕС [for-zone ИМЯ_ЗОНЫ]** - Добавить сервер в список DNS-серверов перенаправления (forwarder), которые передают запросы DNS-серверу внешней сети. Дополнительно можно указать имя зоны, для которой осуществляется перенаправление запросов.

**inet dns forwarders delete АДРЕС [for-zone ИМЯ_ЗОНЫ]** - Удалить сервер из списка DNS-серверов перенаправления (forwarder), которые передают запросы DNS-серверу внешней сети.

**inet dns forwarders list** - Просмотреть список DNS-серверов перенаправления (forwarder).

**inet dns mode {on | off}** - Включить или выключить автоматический запуск DNS-сервера при загрузке ViPNet Coordinator HW.

**inet dns querylog {on | off}** - Включить или выключить ведение журнала DNS-запросов.

**inet dns querylog show [filtered СТРОКА]** - Просмотреть журнал DNS-запросов.

**inet dns querylog size РАЗМЕР** - Задать размер журнала DNS-запросов.

**inet dns start** - Запустить DNS-сервер.

**inet dns stop** - Завершить работу DNS-сервера.

**inet ifconfig ИНТЕРФЕЙС address IP-АДРЕС netmask МАСКА** - Настроить параметры сетевого интерфейса.

**inet ifconfig ИНТЕРФЕЙС address add IP-АДРЕС netmask МАСКА** - Добавить дополнительный IP-адрес на сетевой интерфейс.

**inet ifconfig ИНТЕРФЕЙС address delete IP-АДРЕС netmask МАСКА** - Удалить дополнительный IP-адрес с сетевого интерфейса.

**inet ifconfig АГРЕГИРОВАННЫЙ_ИНТЕРФЕЙС bonding add ПОДЧИНЕННЫЙ_ИНТЕРФЕЙС** - Добавить подчиненный физический интерфейс в агрегированный интерфейс.

**inet ifconfig ИНТЕРФЕЙС bonding ad-select РЕЖИМ** - Задать режим выбора активного агрегатора на агрегированном интерфейсе, работающем в режиме 802.3ad.

**inet ifconfig АГРЕГИРОВАННЫЙ_ИНТЕРФЕЙС bonding delete ПОДЧИНЕННЫЙ_ИНТЕРФЕЙС** - Удалить подчиненный интерфейс из агрегированного.

**inet ifconfig ИНТЕРФЕЙС bonding lacp-rate {slow | fast}** - Задать частоту обмена пакетами по протоколу LACP для агрегированных интерфейсов, работающих в режиме 802.3ad.

**inet ifconfig ИНТЕРФЕЙС bonding miimon ИНТЕРВАЛ** - Задать частоту проверки соединения на подчиненных физических интерфейсах.

**inet ifconfig АГРЕГИРОВАННЫЙ_ИНТЕРФЕЙС bonding primary {ПОДЧИНЕННЫЙ_ИНТЕРФЕЙС | none}** - Настроить агрегированный интерфейс, работающий в режиме balance-tlb или active-backup. Команда используется для принудительного выбора одного из подчиненных физических интерфейсов в качестве основного.

**inet ifconfig ИНТЕРФЕЙС bonding xmit-hash-policy LAYER2_|_LAYER2+3_|_LAYER3+4** - Настроить агрегированный интерфейс, работающий в режиме balance-xor или 802.3ad. Команда задает алгоритм вычисления хэш-функции, используемой при выборе подчиненного интерфейса, через который будет отправляться исходящий пакет.

**inet ifconfig ИНТЕРФЕЙС class {access | trunk | slave}** - Выбрать класс для сетевого интерфейса.

**inet ifconfig ИНТЕРФЕЙС disable** - Запретить прохождения IP-трафика через сетевой интерфейс.

**inet ifconfig ИНТЕРФЕЙС dhcp [НАСТРОЙКА {on | off}]** - Установить режим DHCP на сетевом интерфейсе.

**inet ifconfig ИНТЕРФЕЙС dhcp route-metric {МЕТРИКА | none}** - Задать специфичную метрику маршрутам, поступающим от DHCP-сервера, на сетевом интерфейсе ViPNet Coordinator HW.

**inet ifconfig ИНТЕРФЕЙС down** - Выключить сетевой интерфейс.

**inet ifconfig ИНТЕРФЕЙС enable** - Разрешить прохождения IP-трафика через сетевой интерфейс.

**inet ifconfig ИНТЕРФЕЙС mtu {ЗНАЧЕНИЕ_MTU | auto}** - Изменить значение MTU для сетевого интерфейса.

**inet ifconfig {ИНТЕРФЕЙС | all} reset** - Сбросить настройки одного сетевого интерфейса либо всех интерфейсов.

**inet ifconfig ИНТЕРФЕЙС speed СКОРОСТЬ duplex {half | full} autoneg {on | off}** - Задать параметры скорости сетевого интерфейса.

**inet ifconfig ИНТЕРФЕЙС speed auto** - Установить режим автоматического определения параметров скорости на сетевом интерфейсе.

**inet ifconfig ИНТЕРФЕЙС up** - Включить сетевой интерфейс.

**inet ifconfig ИНТЕРФЕЙС vlan add НОМЕР** - Создать интерфейс для виртуальной сети с заданным номером.

**inet ifconfig ИНТЕРФЕЙС vlan delete НОМЕР** - Удалить виртуальный интерфейс.

**inet ntp add {server | peer} {IP-АДРЕС | ДОМЕННОЕ_ИМЯ}** - Добавить сервер в список NTP-серверов, используемых для синхронизации времени.

**inet ntp delete {server | peer} {IP-АДРЕС | ДОМЕННОЕ_ИМЯ}** - Удалить сервер из списка NTP-серверов, используемых для синхронизации времени.

**inet ntp list** - Просмотреть список NTP-серверов, используемых для синхронизации времени.

**inet ntp mode {on | off}** - Включить или выключить автоматический запуск NTP-сервера при загрузке ViPNet Coordinator HW.

**inet ntp orphan {on STRATUM | off}** - Включить или выключить переход локального NTP-сервера в изолированный (orphan) режим при потере соединения с внешними NTP-серверами.

**inet ntp start** - Запустить NTP-сервер.

**inet ntp stop** - Завершить работу NTP-сервера.

**inet ospf area 0-4294967295 auth {on {pswd | md5} | off}** - Включить или выключить аутентификацию в протоколе OSPF.

**inet ospf interface ИНТЕРФЕЙС keys add KEYID** - Добавить ключ для аутентификации MD5 HMAC.

**inet ospf interface ИНТЕРФЕЙС keys remove {all | KEYID}** - Удалить ключ для аутентификации MD5 HMAC.

**inet ospf interface ИНТЕРФЕЙС password [remove]** - Установить или удалить пароль для аутентификации на выбранном интерфейсе.

**inet ospf mode {on | off}** - Включить или выключить использование протокола OSPF.

**inet ospf network add IP-АДРЕС_НАЗНАЧЕНИЯ netmask МАСКА_СЕТИ area 0-4294967295** - Добавить сеть, в которой должна выполняться маршрутизация по протоколу OSPF.

**inet ospf network delete IP-АДРЕС_НАЗНАЧЕНИЯ netmask МАСКА_СЕТИ area 0-4294967295** - Удалить сеть, которая была указана как маршрутизируемая по протоколу OSPF.

**inet ospf redistribute add {static | dhcp}** - Включить перераспределение статических маршрутов или маршрутов DHCP-сервера, которое позволяет выполнять протокол OSPF.

**inet ospf redistribute delete {static | dhcp}** - Выключить перераспределение статических маршрутов или маршрутов DHCP-сервера, которое позволяет выполнять протокол OSPF.

**inet ospf priority ПРИОРИТЕТ [interface ИНТЕРФЕЙС]** - Задать приоритет ViPNet Coordinator HW.

**inet ospf router-id {ИДЕНТИФИКАТОР | auto}** - Задать идентификатор ViPNet Coordinator HW в формате адреса протокола IPv4.

**inet ospf show configuration** - Просмотреть настройки протокола OSPF.

**inet ospf show database** - Просмотреть информацию о состоянии каналов связи между всеми OSPF-маршрутизаторами в базе данных (link state database).

**inet ospf show neighbour** - Просмотреть сведения о соседних OSPF-маршрутизаторах, работающих в вашей сети по протоколу OSPF.

**inet ping АДРЕС [count VALUE] [iface NAME] [size VALUE]** - Проверить соединение с сетевым узлом.

**inet policy active {ИМЯ_ПОЛИТИКИ | default}** - Задать действующую политику маршрутизации.

**inet policy rule add ИМЯ_ПОЛИТИКИ ПРИОРИТЕТ match {address {from | to} IP-АДРЕС/МАСКА | {inbound-interface | outbound-interface} ИНТЕРФЕЙС | dscp ЗНАЧЕНИЕ_МЕТКИ_DSCP}** - Задать условие применения правила политики маршрутизации.

**inet policy rule add ИМЯ_ПОЛИТИКИ ПРИОРИТЕТ {table {НОМЕР_ТАБЛИЦЫ | name ИМЯ_ТАБЛИЦЫ | default} | block}** - Задать действие правила политики маршрутизации.

**inet policy rule clear ИМЯ_ПОЛИТИКИ [priority ПРИОРИТЕТ]** - Удалить все правила политики маршрутизации или правила заданного приоритета.

**inet policy rule delete ИМЯ_ПОЛИТИКИ ПРИОРИТЕТ match {address {from | to} IP-АДРЕС/МАСКА | {inbound-interface | outbound-interface} ИНТЕРФЕЙС | dscp ЗНАЧЕНИЕ_МЕТКИ_DSCP}** - Удалить условие применения правила политики маршрутизации.

**inet policy rule delete ИМЯ_ПОЛИТИКИ ПРИОРИТЕТ {table {НОМЕР_ТАБЛИЦЫ | name ИМЯ_ТАБЛИЦЫ | default} | block}** - Удалить действие правила политики маршрутизации.

**inet prefix-list add NAME** - Создать префикс-лист.

**inet prefix-list NAME clear seq NUM** - Удалить правило из префикс-листа.

**inet prefix-list delete NAME** - Удалить префикс-лист.

**inet prefix-list NAME [seq NUM] {permit|deny} {SUBNET [le LEN][ge LEN] | any}** - Добавить или изменить правило в префикс-листе.

**inet prefix-list show [NAME]** - Просмотреть список префикс-листов или состав выбранного префикс-листа.

**inet route add {IP-АДРЕС_НАЗНАЧЕНИЯ [netmask МАСКА] | default} next-hop IP-АДРЕС_ШЛЮЗА [table НОМЕР_ТАБЛИЦЫ [name ИМЯ_ТАБЛИЦЫ]] [distance 1-255 [weight 1-255]]** - Добавить статический маршрут, в том числе в пользовательские таблицы маршрутизации.

**inet route clear [table {НОМЕР_ТАБЛИЦЫ | name ИМЯ_ТАБЛИЦЫ}]** - Удалить все маршруты, в том числе маршрут по умолчанию.

**inet route delete {IP-АДРЕС_НАЗНАЧЕНИЯ [netmask МАСКА] | default} [next-hop IP-АДРЕС_ШЛЮЗА] [table {НОМЕР_ТАБЛИЦЫ | name ИМЯ_ТАБЛИЦЫ}]** - Удалить маршрут.

**inet route-map add NAME** - Создать карту маршрутов.

**inet route-map NAME clear SEQ** - Удалить блок из карты маршрутов.

**inet route-map delete NAME** - Удалить карту маршрутов.

**inet route-map NAME SEQ match as-path AS-PATH-FILTER-NAME** - Добавить или заменить в блоке карты маршрутов проверку совпадения с AS-path-фильтром.

**inet route-map NAME SEQ match community COMMUNITY-LIST-NAME [exact-match]** - Добавить или заменить в блоке карты маршрутов проверку совпадения с комьюнити-листом.

**inet route-map NAME SEQ match prefix-list PREFIX-LIST-NAME** - Добавить или заменить в блоке карты маршрутов проверку совпадения с префикс-листом.

**inet route-map NAME SEQ on-match {next|goto NUMBER}** - Добавить или заменить в блоке карты маршрутов директиву перехода в другой блок при совпадении условий.

**inet route-map NAME {permit|deny} SEQ** - Изменить действие блока в карте маршрутов.

**inet route-map NAME SEQ set as-path-prepend STRING** - Добавить или заменить в блоке карты маршрутов установку дополнения атрибута AS-path.

**inet route-map NAME SEQ set community {[additive] COMMUNITY-LINE| none}** - Добавить или заменить в блоке карты маршрутов установку комьюнити маршрута.

**inet route-map NAME SEQ set local-preference LOCAL-PREFERENCE** - Добавить или заменить в блоке карты маршрутов установку значения атрибута local-preference.

**inet route-map NAME SEQ set metric METRIC** - Добавить или заменить в блоке карты маршрутов установку значения атрибута MED.

**inet route-map NAME SEQ set next-hop IP-ADDRESS** - Добавить или заменить в блоке карты маршрутов установку next-hop маршрута.

**inet route-map NAME SEQ set weight WEIGHT** - Добавить или заменить в блоке карты маршрутов установку веса маршрута.

**inet route-map show [NAME [usage]]** - Просмотреть список карт маршрутов, а также их состав и использование.

**inet show dgd configuration** - Просмотреть параметры службы DGD.

**inet show dgd next-hop [ИМЯ_ШЛЮЗА]** - Просмотреть настройки проверки шлюзов и их текущее состояние.

**inet show dgd rule [ИМЯ_ПРАВИЛА]** - Просмотреть параметры правил службы DGD.

**inet show dhcp client** - Просмотреть настройки DHCP на сетевых интерфейсах (настройки DHCP-клиента).

**inet show dhcp server** - Просмотреть настройки DHCP-сервера и его текущее состояние.

**inet show dhcp server lease [{last | all | full}]** - Просмотреть список клиентов DHCP-сервера.

**inet show dhcp relay [НОМЕР_КОПИИ]** - Просмотреть настройки службы DHCP-relay и ее текущее состояние.

**inet show dns** - Просмотреть информацию о состоянии DNS-сервера.

**inet show interface [ИМЯ_ИНТЕРФЕЙСА | ИМЯ_ИНТЕРФЕЙСА:НОМЕР]** - Просмотреть параметры и состояние сетевого интерфейса.

**inet show interface state [ИНТЕРФЕЙС]** - Просмотреть статус сетевого интерфейса.

**inet show mac-address-table [{interface ИНТЕРФЕЙС | address IP-АДРЕС | hwaddress MAC-АДРЕС | vlan ИНТЕРФЕЙС_VLAN}]** - Просмотреть ARP-таблицу (таблицу, содержащую записи о преобразованиях IP-адресов в MAC-адреса). Примечание. Если узел недоступен, время жизни записей в ARP-таблице от 5 до 10 минут.

**inet show ntp** - Просмотреть настройки и состояние NTP-сервера.

**inet show policy rule {active | all | ИМЯ_ПОЛИТИКИ}** - Просмотреть правила политик маршрутизации.

**inet show routing [{static [table {НОМЕР_ТАБЛИЦЫ | name ИМЯ_ТАБЛИЦЫ | default}] | dhcp | ospf [ФИЛЬТР] | bgp}]** - Просмотреть таблицу маршрутизации по умолчанию, списки маршрутов от конкретного источника (Static, DHCP/PPP, OSPF, BGP) или пользовательскую таблицу маршрутизации.

**inet show traffic [interface ИНТЕРФЕЙС][interval ИНТЕРВАЛ_ОБНОВЛЕНИЯ]** - Просмотреть статистику передачи данных по интерфейсам: текущее значение скорости и объёма трафика.

**inet show usb-modem** - Просмотреть информацию о модеме и настройках подключения к сети текущего прератора.

**inet show usb-modem chatscript** - Просмотреть скрипт подключения к сети текущего оператора.

**inet show usb-modem config** - Просмотреть конфигурацию текущего оператора.

**inet show usb-modem providers** - Просмотреть список операторов.

**inet show vlan** - Просмотреть список виртуальных интерфейсов.

**inet show wifi** - Примечание. Команда доступна только на аппаратных платформах со встроенным адаптером Wi-Fi. Просмотреть настройки адаптера Wi-Fi.

**inet snmp autostart {on | off}** - Включить или выключить автоматический запуск SNMP-агента при загрузке ViPNet Coordinator HW.

**inet snmp cluster node IP-АДРЕС community** - Задать community string, который используется для мониторинга узла кластера по протоколам SNMPv1 и SNMPv2c.

**inet snmp cluster node IP-АДРЕС context КОНТЕКСТ** - Изменить контекст, назначенный узлу кластера.

**inet snmp cluster show [community]** - Просмотреть конфигурацию мониторинга кластера по протоколу SNMP.

**inet snmp cluster v2 {on | off}** - Разрешить или запретить чтение SNMP-параметров узлов кластера по протоколам SNMPv1 и SNMPv2c.

**inet snmp community add** - Добавить community string (пароль) для чтения SNMP-параметров ViPNet Coordinator HW.

**inet snmp community change** - Изменить community string для чтения SNMP-параметров ViPNet Coordinator HW.

**inet snmp community delete** - Удалить community string, используемый для чтения SNMP-параметров ViPNet Coordinator HW.

**inet snmp community list** - Просмотреть список community string для чтения SNMP-параметров ViPNet Coordinator HW.

**inet snmp logging УРОВЕНЬ_ВАЖНОСТИ** - Изменить уровень важности событий SNMP-агента, которые будут записываться в системный журнал ViPNet Coordinator HW.

**inet snmp port ПОРТ** - Задать UDP-порт, на котором SNMP-агент будет принимать запросы.

**inet snmp reset-engineid** - Сгенерировать новый идентификатор engineID SNMP-агента ViPNet Coordinator HW.

**inet snmp show** - Просмотреть информацию о текущем состоянии и настройках SNMP-агента.

**inet snmp start** - Запустить встроенный SNMP-агент ViPNet Coordinator HW.

**inet snmp stop** - Завершить работу встроенного SNMP-агента ViPNet Coordinator HW.

**inet snmp system contact** - Задать параметр mib-2.system.sysContact («Контактное лицо») SNMP-агента ViPNet Coordinator HW.

**inet snmp system location** - Задать параметр mib-2.system.sysLocation («Местоположение») SNMP-агента ViPNet Coordinator HW.

**inet snmp system name** - Задать параметр mib-2.system.sysName («Имя устройства») SNMP-агента ViPNet Coordinator HW.

**inet snmp trapsink add АДРЕС [port НОМЕР] [{v1 | inform}]** - Добавить адрес сетевого узла, на который SNMP-агент ViPNet Coordinator HW будет отправлять оповещения.

**inet snmp trapsink delete АДРЕС [port НОМЕР]** - Удалить адрес сетевого узла, на который SNMP-агент ViPNet Coordinator HW отправляет оповещения.

**inet snmp trapsink list [secure]** - Просмотреть список сетевых узлов, на которые SNMP-агент отправляет оповещения.

**inet snmp user add ИМЯ_ПОЛЬЗОВАТЕЛЯ [{md5 | sha}]** - Добавить пользователя SNMP-агента ViPNet Coordinator HW.

**inet snmp user delete ИМЯ_ПОЛЬЗОВАТЕЛЯ** - Удалить пользователя SNMP-агента ViPNet Coordinator HW.

**inet snmp user list** - Просмотреть список пользователей SNMP-агента ViPNet Coordinator HW.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ key [off]** - Создать, изменить или удалить ключ шифрования пользователя SNMP-агента ViPNet Coordinator HW.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ name НОВОЕ_ИМЯ_ПОЛЬЗОВАТЕЛЯ** - Изменить имя пользователя SNMP-агента ViPNet Coordinator HW.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ passwd [{md5 | sha}]** - Изменить пароль пользователя SNMP-агента ViPNet Coordinator HW.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ read {on | off}** - Разрешить или запретить чтение SNMP-параметров (OID) для пользователя SNMP-агента ViPNet Coordinator HW.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ trapsess {on | off}** - Разрешить или запретить отправку SNMP-оповещений для пользователя.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ trapsess add АДРЕС [port НОМЕР] [inform]** - Добавить адрес сетевого узла, на который SNMP-агент ViPNet Coordinator HW будет отправлять оповещения для пользователя.

**inet snmp user set ИМЯ_ПОЛЬЗОВАТЕЛЯ trapsess delete АДРЕС [port НОМЕР]** - Удалить адрес сетевого узла, на который SNMP-агент ViPNet Coordinator HW отправляет оповещения для пользователя.

**inet snmp v2 {ro | traps} {on | off}** - Разрешить или запретить чтение SNMP-параметров (OID) и отправку SNMP-оповещений по протоколам SNMPv1 и SNMPv2c.

**inet snmp v3 {ro | traps} {on | off}** - Разрешить или запретить чтение SNMP-параметров (OID) и отправку SNMP-оповещений по протоколу SNMPv3.

**inet ssh {host АДРЕС | id ИДЕНТИФИКАТОР} [user ПОЛЬЗОВАТЕЛЬ] [port ПОРТ]** - Подключиться к удаленному узлу по протоколу SSH.

**inet usb-modem add provider ОПЕРАТОР** - Добавить нового оператора в список доступных операторов.

**inet usb-modem delete provider ОПЕРАТОР** - Удалить оператора из списка доступных операторов.

**inet usb-modem mode {on | off}** - Включить или выключить модем.

**inet usb-modem modify chatscript** - Изменить скрипт подключения к сети текущего оператора.

**inet usb-modem modify config** - Изменить конфигурацию текущего оператора.

**inet usb-modem reset pin** - Удалить ПИН SIM-карты текущего оператора.

**inet usb-modem set connection address {IP-АДРЕС | DNS-ИМЯ}** - Задать адрес сервера доступа для подключения к сети текущего оператора.

**inet usb-modem set dns {on | off}** - Разрешить или запретить получение адреса DNS-сервера оператора.

**inet usb-modem set password ПАРОЛЬ** - Задать пароль пользователя, используемый при аутентификации во время подключения к сети текущего оператора.

**inet usb-modem set phone НОМЕР_ДОСТУПА** - Задать номер доступа текущего оператора.

**inet usb-modem set pin ПИН** - Задать ПИН SIM-карты текущего оператора.

**inet usb-modem set provider ОПЕРАТОР** - Выбрать текущего оператора из списка доступных.

**inet usb-modem set route {on | off}** - Разрешить или запретить получение маршрута по умолчанию от оператора при подключении к его сети.

**inet usb-modem set route-metric {1-255 | none}** - Задать или удалить специфичную метрику маршрута по умолчанию, получаемого от оператора при подключении к его сети.

**inet usb-modem set user ИМЯ** - Задать имя пользователя, используемое при аутентификации во время подключения к сети текущего оператора.

**inet vlan НОМЕР comment add КОММЕНТАРИЙ** - Добавить комментарий к виртуальной сети.

**inet vlan НОМЕР comment delete** - Удалить комментарий к виртуальной сети.

**inet wifi access-point channel НОМЕР** - Задать номер канала Wi-Fi при работе ViPNet Coordinator HW в режиме точки доступа.

**inet wifi access-point hwmode {b | g}** - Выбрать стандарт сети Wi-Fi при работе ViPNet Coordinator HW в режиме точки доступа.

**inet wifi access-point show** - Просмотреть список подключенных клиентов Wi-Fi к ViPNet Coordinator HW, при его работе в режиме точки доступа.

**inet wifi {client | access-point} authentication {open | wpa-psk | wpa2-psk}** - Указать тип защиты сети Wi-Fi - при работе ViPNet Coordinator HW в режиме клиента Wi-Fi. -  Задать тип защиты сети Wi-Fi - при работе ViPNet Coordinator HW в режиме точки доступа Wi-Fi.

**inet wifi mode** - Включить или выключить интерфейс Wi-Fi. Синтаксис inet wifi mode {on | off}

**inet wifi role {access-point | client}** - Выбрать режим работы ViPNet Coordinator HW в сети Wi-Fi.

**inet wifi scan** - Просмотреть доступные сети Wi-Fi.

# iplir

**iplir adapter add ИНТЕРФЕЙС [traffic {on | off}]** - Добавить новый сетевой интерфейс.

**iplir adapter delete ИНТЕРФЕЙС** - Удалить сетевой интерфейс.

**iplir adapter traffic ИНТЕРФЕЙС {on | off}** - Включить или выключить прохождение IP-трафика через сетевой интерфейс.

**iplir config [{ИНТЕРФЕЙС | ГРУППА_ИНТЕРФЕЙСОВ}]** - Редактировать один из файлов конфигурации: основной файл конфигурации, файл конфигурации заданного интерфейса или группы интерфейсов.

**iplir info [{ИНТЕРФЕЙС | ГРУППА_ИНТЕРФЕЙСОВ}]** - Просмотреть информацию о своем узле и количестве туннельных соединений, а также статистику фильтрации IP-пакетов по заданному интерфейсу или группе интерфейсов.

**iplir option be-default-gateway {on | off | auto}** - Включить или выключить обнаружение ViPNet Coordinator HW другими координаторами с версией ПО 5.3.0 и выше в качестве шлюза по умолчанию.

**iplir option connection-server {ID_УЗЛА | default}** - Задать или удалить сервер соединений.

**iplir option interface-timeout ПЕРИОД** - Задать период опроса сетевых интерфейсов.

**iplir option ip-forwarding {on | off | system}** - Включить или выключить маршрутизацию транзитных IP-пакетов при запуске управляющей службы iplircfg.

**iplir option keepalive-timeout ПЕРИОД** - Задать период отправки IP-пакетов серверу соединений для поддержания активности соединения и пропуска входящего трафика через межсетевой экран.

**iplir option maxtimediff ИНТЕРВАЛ** - Задать допустимый интервал времени между отправкой и приемом IP-пакетов.

**iplir option mode {dynamic ID_УЗЛА [always-use-server] | static}** - Задать режим подключения ViPNet Coordinator HW к VPN-сети.

**iplir option mss-decrease КОЛИЧЕСТВО** - Задать количество байт, на которое будет уменьшен максимальный размер TCP-сегмента (MSS).

**iplir option ping-timeout ПЕРИОД** - Задать период опроса состояния ViPNet-клиентов со стороны ViPNet Coordinator HW для которых он - сервер IP-адресов.

**iplir option show** - Просмотреть текущие параметры работы управляющей службы iplircfg.

**iplir option sync-time {on | off}** - Включить или выключить синхронизацию времени с сервером IP-адресов.

**iplir option syslog-level УРОВЕНЬ** - Задать уровень важности событий, регистрируемых в системном журнале или отключить ведение системного журнала.

**iplir option udp-ports-count КОЛИЧЕСТВО_ПОРТОВ** - Задать диапазон портов UDP.

**iplir ping ИДЕНТИФИКАТОР** - Проверить соединение с сетевым узлом ViPNet.

**iplir set l2overip interface ИНТЕРФЕЙС** - Задать рабочий интерфейс L2OverIP.

**iplir set l2overip local-port ПОРТ IP-АДРЕС** - Добавить параметры локального сегмента сети в настройках L2OverIP.

**iplir set l2overip mac-ttl ВРЕМЯ** - Задать время жизни MAC-адреса в таблице MAC-адресов виртуального коммутатора при отсутствии трафика, поступающего от этого адреса.

**iplir set l2overip mode {switch | none}** - Включить или выключить L2OverIP.

**iplir set l2overip remote-port ПОРТ IP-АДРЕС** - Добавить параметры удаленного сегмента сети в настройках L2OverIP.

**iplir set l2overip remote-port ПОРТ delete** - Удалить порт с заданным номером из настроек L2OverIP.

**iplir set l2overip unsolicited-frames {drop | broadcast | smart-broadcast}** - Задать режим обработки одноадресных Ethernet-кадров с неизвестным MAC-адресом получателя.

**iplir node ID_УЗЛА access-point {add | delete} IP-АДРЕС [port ПОРТ] [metric МЕТРИКА]** - Добавить или удалить IP-адрес доступа к координатору.

**iplir node ID_УЗЛА blockforward {on | off}** - Включить или выключить блокирование транзитных IP-пакетов, передаваемых через ViPNet Coordinator HW связанному с ним сетевому узлу ViPNet.

**iplir node ID_УЗЛА {add | delete} domain-name {ИМЯ | all}** - Добавить или удалить доменное имя узла ViPNet.

**iplir node list [{clients | gateways}] [filter ФИЛЬТР]** - Просмотреть сведения об узлах ViPNet, связанных с ViPNet Coordinator HW.

**iplir node ID_УЗЛА show** - Просмотреть сведения об узле ViPNet.

**iplir node ID_УЗЛА show domain-names** - Просмотреть список доменных имен сетевого узла ViPNet.

**iplir node ID_УЗЛА update domain-name cache** - Обновить DNS-кеш узла ViPNet.

**iplir set performance-mode {single|multi}** - Выбрать профиль производительности обработки трафика сетевых соединений.

**iplir show adapter ИНТЕРФЕЙС** - Просмотреть разрешение на прохождение IP-трафика через сетевой интерфейс.

**iplir show adapters** - Просмотреть все активные статические и динамические сетевые интерфейсы ViPNet Coordinator HW. При просмотре для каждого интерфейса в списке указан параметр allowtraffic , который показывает, разрешено или заблокировано прохождения IP-трафика через интерфейс.

**iplir show adapters groups** - Просмотреть активные динамические сетевые интерфейсы ViPNet Coordinator HW.

**iplir show authentication-type** - Просмотреть способ аутентификации пользователей.

**iplir show ciphertype [NODEID]** - Просмотреть информацию о текущем режиме шифрования для сети или отдельного узла ViPNet.

**iplir show config [{ИНТЕРФЕЙС | ГРУППА_ИНТЕРФЕЙСОВ}]** - Просмотреть один из файлов конфигурации: основной файл конфигурации или файл конфигурации заданного интерфейса.

**iplir show exchange-keys [{warning | peer PEER}]** - Просмотреть сведения о ключах обмена.

**iplir show firewall status** - Просмотреть статистику работы межсетевого экрана.

**iplir show key-info** - Получить информацию о ключе защиты узла ViPNet Coordinator HW.

**iplir show l2overip ОПЦИЯ** - Просмотреть состояние или настройки L2OverIP.

**iplir show performance-mode** - Просмотреть текущий профиль производительности обработки трафика сетевых соединений.

**iplir show tcptunnel-info** - Просмотреть настройки TCP-туннеля на ViPNet Coordinator HW.

**iplir start** - Запустить управляющую службу iplircfg.

**iplir stop** - Завершить работу управляющей службы iplircfg.

**iplir warning-threshold ПЕРИОД_ОПОВЕЩЕНИЯ** - Задать период оповещения о скором истечении срока действия:  ключей обмена; -  ключа защиты узла; -  сертификата пользователя.

**iplir tcptunnel server {on | off}** - Включить или выключить сервер TCP-туннелей.

# machine

**machine backup {on | off}** - Включить или выключить резервное копирование индивидуальной конфигурации ViPNet Coordinator HW по расписанию.

**machine backup export {server | usb}** - Экспортировать индивидуальную конфигурацию ViPNet Coordinator HW на сервер ViPNet Prime или USB-носитель вручную.

**machine backup schedule НАЧАЛО_ИНТЕРВАЛА[-КОНЕЦ_ИНТЕРВАЛА]** - Задать расписание резервного копирования индивидуальной конфигурации ViPNet Coordinator HW на сервер ViPNet Prime.

**machine config export usb** - Экспортировать универсальную конфигурацию ViPNet Coordinator HW на USB-носитель.

**machine config import** - Импортировать настройки универсальной конфигурации или индивидуальную конфигурацию ViPNet Coordinator HW с USB-носителя.

**machine halt** - Завершить работу ViPNet Coordinator HW.

**machine hosts add IP-АДРЕС ДОМЕННОЕ_ИМЯ** - Добавить запись о соответствии IP-адреса доменному имени в файл hosts ViPNet Coordinator HW.

**machine hosts remove {IP-АДРЕС | ДОМЕННОЕ_ИМЯ}** - Удалить запись о соответствии IP-адреса доменному имени в файле hosts ViPNet Coordinator HW.

**machine hosts show** - Просмотреть файл hosts ViPNet Coordinator HW.

**machine logs clear dns** - Очистить журнал DNS-запросов.

**machine logs export usb** - Экспортировать архив файлов журналов на USB-носитель.

**machine logs export-and-clear usb** - Экспортировать архив файлов журналов на USB-носитель с последующим удалением файлов журналов на ViPNet Coordinator HW.

**machine logs export network-traffic usb ИМЯ** - Экспортировать журнал IP-пакетов на USB-носитель.

**machine logs settings {user-audit | crypto-audit} size РАЗМЕР** - Задать максимальный размер журнала аудита или журнала СКЗИ.

**machine logs settings cef ИНТЕРФЕЙС {include | exclude} {СОБЫТИЯ | none}** - Задать для сетевого интерфейса исключения из списка событий журнала IP-пакетов, экспортируемых в формате CEF.

**machine logs settings cef event-type {all | blocked} [ИНТЕРФЕЙС]** - Задать тип событий журнала IP-пакетов, экспортируемых в формате CEF.

**machine logs settings network-traffic ИНТЕРФЕЙС maxsize РАЗМЕР** - Задать максимальный размер журнала IP-пакетов для выбранного сетевого интерфейса.

**machine logs settings network-traffic ИНТЕРФЕЙС omit-client-port {on | off}** - Включить или выключить регистрацию порта TCP-соединения на сетевом интерфейсе.

**machine logs settings network-traffic ИНТЕРФЕЙС register {passed | broadcast | service} {on | off}** - Включить или выключить регистрацию IP-пакетов определенного типа, проходящих через сетевой интерфейс, в журнале IP-пакетов.

**machine logs settings network-traffic ИНТЕРФЕЙС timediff ИНТЕРВАЛ** - Задать интервал времени, в течение которого связанные события регистрации IP-пакетов, проходящих через сетевой интерфейс, объединяются в одну запись журнала IP-пакетов.

**machine logs settings show** - Просмотреть размер и процент заполнения журналов аудита и СКЗИ, а также настройки экспорта журнала IP-пакетов в формате CEF.

**machine logs show crypto-audit [reversed] [result {success | failed}] [from ВРЕМЯ] [to ВРЕМЯ] [user ИМЯ] [text СТРОКА]** - Просмотреть журнал СКЗИ.

**machine logs show mftp** - Просмотреть журнал MFTP.

**machine logs show network-traffic** - Просмотреть журнал IP-пакетов. Синтаксис machine logs show network-traffic

**machine logs show syslog [reversed] [{since ВРЕМЯ | filtered {СЛУЖБА | string СТРОКА} }]** - Просмотреть системный журнал.

**machine logs show user-audit [reversed] [result {success | failed}] [from ВРЕМЯ] [to ВРЕМЯ] [user ИМЯ] [text СТРОКА]** - Просмотреть журнал аудита.

**machine reboot** - Перезагрузить ViPNet Coordinator HW.

**machine reboot-schedule {on | off}** - Включить или выключить перезагрузку ViPNet Coordinator HW по расписанию.

**machine reboot-schedule show** - Просмотреть настройку перезагрузки ViPNet Coordinator HW по расписанию.

**machine reboot-schedule time ВРЕМЯ [day ДЕНЬ]** - Задать расписание перезагрузки ViPNet Coordinator HW.

**machine self-test** - Запустить регламентное тестирование ViPNet Coordinator HW.

**machine session-inactivity-timeout set ВРЕМЯ** - Установить допустимое время неактивности сессий пользователей.

**machine session-inactivity-timeout show** - Просмотреть текущее допустимое время неактивности сессий пользователей.

**machine set date ДАТА ВРЕМЯ** - Изменить дату и время.

**machine set hostname ИМЯ** - Изменить имя компьютера.

**machine set loghost {АДРЕС | local | null}** - Задать место хранения системного журнала. С помощью этой команды также можно выключить запись событий в журнал.

**machine set log invalid-packet {on | off}** - Включить и отключить запись нарушений параметров таблицы соединений в системный журнал.

**machine set log queue {on | off}** - Включить или выключить запись в системный журнал событий о входящих IP-пакетах, обработка которых была отклонена в рамках приоритетной обработки трафика.

**machine set timezone ВРЕМЕННАЯ_ЗОНА** - Задать временную зону (часовой пояс).

**machine show backup** - Просмотреть состояние резервного копирования индивидуальной конфигурации ViPNet Coordinator HW.

**machine show date** - Просмотреть дату и время, установленные на ViPNet Coordinator HW.

**machine show hostname** - Просмотреть имя ViPNet Coordinator HW.

**machine show loghost** - Просмотреть настройки хранения системного журнала.

**machine show log invalid-packet** - Просмотреть настройку записи нарушений параметров таблицы соединений в системный журнал (см. machine set log invalid-packet).

**machine show log queue** - Просмотреть настройку записи событий об отклоненных IP-пакетах в системный журнал (см. machine set log queue).

**machine show memory** - Просмотреть информацию об использовании оперативной памяти и дискового пространства.

**machine show timezone** - Просмотреть информацию о временной зоне (часовом поясе), настроенной на ViPNet Coordinator HW.

**machine show uptime** - Просмотреть время работы ViPNet Coordinator HW после загрузки, а также среднее число процессов в очереди за ближайшее время.

**machine update-queue** - Просмотреть очередь отложенных обновлений компонентов и настроек ViPNet Coordinator HW, отправленных из ViPNet Prime.

# mftp

**mftp config** - Редактировать конфигурационный файл службы mftpd.

**mftp info** - Просмотреть очередь исходящих транспортных конвертов MFTP.

**mftp show config** - Просмотреть конфигурационный файл транспортного сервера MFTP.

**mftp start** - Запустить службу mftpd (транспортный сервер MFTP).

**mftp stop Режимы командного интерпретатора Режим настройки.** - Завершить работу службы mftpd (транспортный сервер MFTP).

# service

**service cert delete {cert | crl | private} PEM** - Удалить сертификат, список аннулированных сертификатов (CRL) или закрытый ключ.

**service cert import** - Импортировать в локальное хранилище ViPNet Coordinator HW закрытый ключ сертификата, сертификат или список аннулированных сертификатов (CRL) с USB-накопителя.

**service cert list** - Просмотреть установленные закрытые ключи, сертификаты и списки аннулированных сертификатов (CRL).

**service cert request create name ИМЯ bits ДЛИНА_КЛЮЧА digest {sha256 | sha384 | sha512} [subj SUBJ]** - Создать закрытый ключ и запрос на сертификат в формате PKCS#12.

**service cert request delete ЗАПРОС** - Удалить запрос на сертификат.

**service cert request export ЗАПРОС** - Экспортировать запрос на сертификат на USB-накопитель.

**service cert request list** - Просмотреть список созданных запросов на сертификат.

**service cert request show ЗАПРОС** - Просмотреть содержимое запроса на сертификат с заданным именем или всех запросов на сертификаты, созданных в ViPNet Coordinator HW.

**service cert show cert СЕРТИФИКАТ** - Просмотреть содержимое сертификата с заданным именем или всех сертификатов, установленных в локальное хранилище ViPNet Coordinator HW.

**service cert show crl CRL** - Просмотреть содержимое списка аннулированных сертификатов (CRL) с заданным именем или всех списков аннулированных сертификатов, установленных в локальное хранилище ViPNet Coordinator HW.

**service dpi mode {on | off}** - Включить или выключить автозапуск подсистемы DPI при перезагрузке ViPNet Coordinator HW.

**service dpi show status** - Просмотреть текущие параметры подсистемы DPI. Синтаксис service dpi show status

**service dpi start** - Запустить подсистему DPI.

**service dpi stop** - Остановить работу подсистемы DPI.

**service dpi update usb** - Обновить подсистему DPI с USB-носителя.

**service http-proxy antivirus bypass {on | off}** - Включить или выключить блокировку трафика, проходящего через прокси-сервер при недоступности ICAP-сервера внешнего антивируса.

**service http-proxy antivirus mode {on | off}** - Включить или выключить антивирусную проверку трафика, проходящего через прокси-сервер.

**service http-proxy antivirus server-url add {reqmod | respmod} URL** - Задать адрес и метод подключения к ICAP-серверу внешнего антивируса.

**service http-proxy antivirus server-url delete {reqmod | respmod}** - Удалить параметры подключения к ICAP-серверу внешнего антивируса.

**service http-proxy antivirus server-url list** - Просмотреть текущие настройки доступа к ICAP-серверу антивируса.

**service http-proxy antivirus show-status** - Просмотреть текущие настройки и состояние антивирусной защиты.

**service http-proxy cache РАЗМЕР** - Задать размер кеша прокси-сервера.

**service http-proxy content-filter add [num НОМЕР] [rule ИМЯ] src АДРЕС_ОТПРАВИТЕЛЯ dst АДРЕС_ПОЛУЧАТЕЛЯ {command HTTP-МЕТОД | mime-type MIME-ТИП} ДЕЙСТВИЕ** - Добавить правило контент-фильтрации.

**service http-proxy content-filter change num НОМЕР_ПРАВИЛА [rule ИМЯ_ПРАВИЛА] src АДРЕС_ОТПРАВИТЕЛЯ dst АДРЕС_ПОЛУЧАТЕЛЯ {command HTTP-МЕТОД | mime-type MIME-ТИП} ДЕЙСТВИЕ** - Редактировать правило контент-фильтрации.

**service http-proxy content-filter default-reply-action ДЕЙСТВИЕ** - Задать действие по умолчанию для ответов от HTTP-ресурсов, которые не подошли под условия других правил контент-фильтрации.

**service http-proxy content-filter default-request-action ДЕЙСТВИЕ** - Задать действие по умолчанию для запросов к HTTP-ресурсам, которые не подошли под условия других правил контент-фильтрации.

**service http-proxy content-filter delete {num НОМЕР | rule ИМЯ}** - Удалить правило контент-фильтрации.

**service http-proxy content-filter list** - Просмотреть список правил контент-фильтрации.

**service http-proxy content-filter mode {on | off}** - Включить или выключить контент-фильтрацию HTTP-трафика.

**service http-proxy content-filter move {num НОМЕР | rule ИМЯ} to НОВЫЙ_НОМЕР** - Изменить порядковый номер правила контент-фильтрации.

**service http-proxy content-filter show-status** - Просмотреть информацию о статусе контент-фильтрации трафика.

**service http-proxy external-address set ИНТЕРФЕЙС** - Задать внешний IP-адрес прокси-сервера.

**service http-proxy external-address show** - Просмотреть внешний IP-адрес прокси-сервера.

**service http-proxy fw-rules apply** - Сгенерировать сетевые фильтры и правила трансляции адресов, соответствующие текущим настройкам прокси-сервера.

**service http-proxy fw-rules delete** - Удалить сетевые фильтры и правила трансляции адресов, необходимые для работы прокси-сервера.

**service http-proxy fw-rules show** - Просмотреть существующие сетевые фильтры и правила трансляции адресов, необходимые для работы прокси-сервера.

**service http-proxy listen-address add ИНТЕРФЕЙС ПОРТ** - Добавить интерфейс и порт, через которые будут приниматься запросы от клиентов прокси-сервера.

**service http-proxy listen-address delete ИНТЕРФЕЙС** - Удалить интерфейс из списка слушающих интерфейсов прокси-сервера.

**service http-proxy listen-address list** - Просмотреть текущий список адресов интерфейсов и портов, через которые принимаются запросы от клиентов прокси-сервера.

**service http-proxy mode {on | off}** - Включить или выключить автоматический запуск прокси-сервера при загрузке ViPNet Coordinator HW.

**service http-proxy reset** - Сбросить текущие настройки прокси-сервера.

**service http-proxy show** - Просмотреть состояние и настройки прокси-сервера.

**service http-proxy start** - Запустить прокси-сервер.

**service http-proxy stop** - Завершить работу прокси-сервера.

**service http-proxy transparent-mode {on | off}** - Включить или выключить «прозрачный» режим работы прокси-сервера.

**service ips start** - Включить предотвращение вторжений (IPS).

**service ips stop** - Выключить предотвращение вторжений (IPS).

**service ips mode {on | off}** - Включить или выключить автоматический запуск предотвращения вторжений (IPS) при загрузке ViPNet Coordinator HW.

**service ips rule restore-default** - Восстановить базу правил IPS до версии, поставляемой в составе дистрибутива ViPNet Coordinator HW.

**service ips rule update {on | off}** - Включить или выключить автоматическое обновление базы правил предотвращения вторжения (правил IPS) по расписанию.

**service ips rule update fetch** - Обновить вручную базу правил предотвращения вторжений (правил IPS) с сервера обновлений.

**service ips rule update server proxy address {АДРЕС_ПРОКСИ-СЕРВЕРА | none}** - Задать адрес прокси-сервера, используемого при подключении к серверу обновлений базы правил IPS.

**service ips rule update server proxy port ПОРТ** - Задать TCP-порт прокси-сервера, используемого при подключении к серверу обновлений базы правил IPS.

**service ips rule update schedule {daily at ВРЕМЯ | weekly on ДЕНЬ at ВРЕМЯ}** - Настроить расписание автоматического обновления базы правил предотвращения вторжений (правил IPS).

**service ips rule update server address АДРЕС_СЕРВЕРА** - Задать адрес сервера обновлений базы правил IPS.

**service ips rule update server login ИМЯ_ПОЛЬЗОВАТЕЛЯ** - Задать имя пользователя для доступа к серверу обновлений базы правил предотвращения вторжений (правил IPS).

**service ips rule update server password** - Задать пароль пользователя для доступа к серверу обновлений базы правил предотвращения вторжений (правил IPS).

**hostname# service ips rule update usb** - Обновить базу правил IPS c USB-носителя.

**service ips show status** - Просмотреть параметры системы предотвращения вторжений IPS.

**service ips show update-settings** - Просмотр текущих параметров обновления базы правил IPS.

**service ips syslog-level УРОВЕНЬ_ВАЖНОСТИ** - Задать уровень важности событий предотвращения вторжений (IPS), записываемых в системный журнал ViPNet Coordinator HW.

**service user-control {start | stop}** - Запустить или остановить работу службы управления пользователями ( uc ).

**service user-control active-users** - Просмотреть информацию о текущих сессиях пользователей Active Directory и Captive portal.

**service user-control ad reset [controller АДРЕС_КОНТРОЛЛЕРА_ДОМЕНА]** - Удалить параметры соединения ViPNet Coordinator HW с контроллером домена Active Directory.

**service user-control ad show [controller АДРЕС]** - Просмотреть информацию о параметрах аутентификации с помощью Active Directory.

**service user-control ad set controller АДРЕС_КОНТРОЛЛЕРА_ДОМЕНА user ИМЯ_ПОЛЬЗОВАТЕЛЯ** - Настроить соединение с контроллером домена Active Directory.

**service user-control ad set connection-timeout ВРЕМЯ** - Задать допустимое время отсутствия связи с контроллером домена Active Directory.

**service user-control ad set controller sync-delay ПЕРИОД** - Задать период получения журнала контроллера домена Active Directory.

**service user-control cp reset** - Сбросить настройки Captive portal.

**service user-control cp set connection-secure РЕЖИМ_СОЕДИНЕНИЯ** - Задать режим соединения Captive portal с LDAP-сервером.

**service user-control cp set connection-timeout ПОЛНОЕ_ВРЕМЯ_ЖИЗНИ_СЕССИИ_ПОЛЬЗОВАТЕЛЯ** - Задать время жизни сессии пользователя Captive portal, по истечении которого сессия будет принудительно сброшена.

**service user-control cp set custom-login-form ТЕКСТОВОЕ_СООБЩЕНИЕ** - Задать произвольное текстовое сообщение на странице аутентификации пользователей Captive portal.

**service user-control cp set hostcert ИМЯ_ФАЙЛА_СЕРТИФИКАТА hostkey ИМЯ_ФАЙЛА_ЗАКРЫТОГО_КЛЮЧА** - Выбрать сертификат веб-сервера Captive portal из локального хранилища сертификатов ViPNet Coordinator HW.

**service user-control cp set idle-timeout ВРЕМЯ_БЕЗДЕЙСТВИЯ_ПОЛЬЗОВАТЕЛЯ** - Задать время бездействия (отсутствия передачи данных) пользователя Captive portal, по истечении которого сессия будет сброшена.

**service user-control cp set ldap АДРЕС_LDAP-СЕРВЕРА identity ИМЯ_АДМИНИСТРАТОРА basedn БАЗА_ПОИСКА** - Настроить соединение Captive portal с LDAP-сервером.

**service user-control cp set ldap cacert ИМЯ_ФАЙЛА_СЕРТИФИКАТА** - Выбрать корневой сертификат LDAP-сервера из локального хранилища сертификатов ViPNet Coordinator HW.

**service user-control cp show** - Просмотреть настройки Captive portal.

**service user-control fw-rules apply** - Создать служебные сетевые фильтры после изменения параметров соединения с сервером Active Directory или Captive portal.

**service user-control fw-rules delete** - Удалить разрешающие сетевые фильтры, созданные командой service user-control fw-rules apply.

**service user-control fw-rules show** - Просмотреть сетевые фильтры, разрешающие соединение ViPNet Coordinator HW с сервером Active Directory и Captive portal.

**service user-control mode {on | off}** - Включить или выключить автозапуск службы управления пользователями ( uc ) при загрузке ViPNet Coordinator HW.

**service user-control show** - Просмотреть информацию о состоянии работы службы управления пользователями ( uc ).

**service user-control syslog-level УРОВЕНЬ_ВАЖНОСТИ** - Задать уровень важности событий службы управления пользователями ( uc ), которые будут попадать в системный журнал.

**service vpn mode {on | off}** - Включить или выключить автозапуск подсистемы VPN при перезагрузке ViPNet Coordinator HW.

**service vpn show status** - Просмотреть текущие параметры подсистемы VPN.

**service vpn start** - Запустить подсистему VPN.

**service vpn stop** - Завершить работу подсистемы VPN.

# ups

**ups set driver ДРАЙВЕР** - Выбрать драйвер UPS.

**ups set mode {master | slave IP-АДРЕС_МАСТЕРА}** - Настроить режим взаимодействия ViPNet Coordinator HW с ИБП.

**ups set monitoring {on | off}** - Включить или выключить мониторинг состояния ИБП.

**ups show config** - Просмотреть текущие настройки взаимодействия ViPNet Coordinator HW с ИБП.

**ups show status [extended]** - Просмотреть информацию о состоянии ИБП.

**ups start** - Запустить службы пакета NUT , обеспечивающие взаимодействие ViPNet Coordinator HW с ИБП.

**ups stop** - Завершить работу служб пакета NUT , обеспечивающих взаимодействие ViPNet Coordinator HW с ИБП.

# vpn

**vpn config delete ИМЯ [ВЕРСИЯ]** - Удалить копию конфигурации VPN.

**vpn config list** - Просмотреть список сохраненных копий конфигурации VPN.

**vpn config load ИМЯ [ВЕРСИЯ]** - Загрузить настройки ViPNet Coordinator HW из копии конфигурации VPN.

**vpn config save ИМЯ** - Сохранить копию текущей конфигурации VPN.

**vpn start** - Запустить службы шифрования IP-пакетов ( itcscrpt и itcshub ) и прозрачного соединения сегментов сети на уровне L2 ( l2overip ).

**vpn stop** - Остановить службы шифрования IP-пакетов ( itcscrpt и itcshub ) и прозрачного соединения сегментов сети на уровне L2 ( l2overip ).

# webui

**webui https-cert СЕРТИФИКАТ** - Установить сертификат для доступа к ViPNet Coordinator HW по протоколу HTTPS.

**webui info** - Просмотреть состояние службы WebUI.

**webui port ПОРТ** - Задать TCP-порт для доступа к ViPNet Coordinator HW с помощью веб-интерфейса.

**webui protocol {http | https}** - Выбрать протокол, используемый для доступа к ViPNet Coordinator HW с помощью веб-интерфейса.

**webui recreate-cert [bits ДЛИНА_КЛЮЧА digest {sha256 | sha384 | sha512} span СРОК_ДЕЙСТВИЯ [subj SUBJECT]]** - Перевыпустить самоподписанный сертификат, используемый для подключения к веб-интерфейсу по протоколу HTTPS.

**webui restart** - Перезапустить службу WebUI.

# прочие

**debug off [ИСТОЧНИК УРОВЕНЬ_ВАЖНОСТИ]** - Выключить вывод сообщений о событиях.

**debug on [ИСТОЧНИК УРОВЕНЬ_ВАЖНОСТИ]** - Включить вывод сообщений о событиях.

**enable** - Перейти в режим настройки.

**exit** - Выйти из текущего режима командного интерпретатора.

**license** - Просмотреть лицензии ViPNet Coordinator HW.

**serial СЕРИЙНЫЙ_НОМЕР** - Установить серийный номер ViPNet Coordinator HW.

**user certificate create** - Издать или перевыпустить сертификат локального аудитора user.

**user certificate delete** - Удалить сертификат локального аудитора user.

**user passwd** - Изменить пароль локального аудитора user.

**user reset passwd** - Сбросить пароль локального аудитора user.

**version [full]** - Просмотреть сведения о ViPNet Coordinator HW.

**version features list** - Просмотреть список функциональных модулей, входящих в состав текущей версии ViPNet Coordinator HW.

**who** - Просмотреть информацию об активных сессиях пользователей.

