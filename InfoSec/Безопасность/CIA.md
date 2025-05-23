# Триада CIA: Конфиденциальность, Целостность, Доступность

При подключении устройства к сети и Интернету, организации открывают двери для хакеров, которые могут проникнуть в их сеть и нанести ущерб. Есть много организаций, которые используют только брандмауэр в своей сети и поэтому думают, что и их внутренняя сеть, и пользователи защищены от угроз в Интернете. [Брандмауэр](https://wiki.merionet.ru/seti/64/chto-takoe-firewall/), как единственное сетевое устройство, развернутое между внутренней сетью и Интернетом, — это просто первый уровень безопасности для всей организации. Но достаточно ли брандмауэра для фильтрации вредоносного входящего и исходящего трафика? Ответ на этот вопрос уже не является однозначным «да» просто потому, что существует много типов трафика, которые используют небезопасные сетевые протоколы для обмена сообщениями между источником и получателем.

Ниже приведены лишь некоторые из многих вопросов, которые должен задать специалист по кибербезопасности:

- Осуществляет ли организация активный мониторинг сообщений [Domain Name System](https://wiki.merionet.ru/seti/48/dns-server-chto-eto-i-kak-rabotaet/) (**DNS**) на предмет угроз?
- Есть ли в организации какие-либо решения для обеспечения безопасности, защищающие входящие и исходящие сообщения электронной почты компании?
- Если в сети происходит кибератака, существуют ли системы для упреждающего блокирования и оповещения?
- Есть ли в организации специальная группа или человек по безопасности для управления общей безопасностью всего предприятия?
- Существуют ли какие-либо политики безопасности и технические средства контроля для защиты внутренней сети?

Не все решения для защиты конечных точек или антивирусы, защищают от угроз, связанных, например с электронной почтой. Проще говоря, организация не может полагаться только на один подход для защиты своих активов, ей нужен многоуровневый подход, известный как Глубокая защита (**Defense in Depth - DiD**).

Стратегия **DiD** подразумевает, что один уровень безопасности не должен использоваться в качестве единственной меры противодействия кибератакам. Если этот единственный уровень не сможет защитить сеть, тогда все (активы) будут открыты для взлома хакерами. В **DiD** реализован многоуровневый подход для защиты всех активов от различных типов кибератак, где, если один уровень не может защитить актив, то есть другой уровень для обеспечения безопасности.

Концепция **Defense in Depth** делит организацию защиты инфраструктуры на три контролируемые части:

- **Физическая**: сюда относятся все меры по ограничению физического доступа к ИТ-инфраструктуре неавторизованных лиц. Например, охранник офиса, системы СКУД, камеры видеонаблюдения, сигнализация, телекоммуникационные шкафы с замками и так далее.
- **Техническая**: сюда относятся все хардварные и софтовые средства защиты информации, призванные контролировать сетевой доступ к объектам информационной системы, межсетевой экран, средства антивирусной защиты рабочих станций, прокси-серверы, системы аутентификации и авторизации.
- **Административная**: сюда относятся все политики и процедуры информационной безопасности, принятые в организации. Данные документы призваны регулировать управление защитой, распределение и обработку критичной информации, использование программных и технических средств в компании, а также взаимодействие сотрудников с информационной системой, сторонними организациями и другими внешними субъектами.

Чтобы лучше понять важность DiD, давайте углубимся в изучение трех основных принципов информационной безопасности, так называемой триады CIA:

- **Конфиденциальность** (Confidentiality)
- **Целостность** (Integrity)
- **Доступность** (Availability)
### Конфиденциальность

Каждый день организации генерируют новые данные, отправляя и получая сообщения между устройствами. Представьте себе организацию, которая использует электронную почту в качестве единственной платформы обмена сообщениями. Каждый человек создает сообщение электронной почты, которое является данными, и эти данные используют некоторое пространство для хранения в локальной системе. Когда адресат получает электронное письмо, оно сохраняется на компьютере получателя. Важно, чтобы сообщение получили только те, кому оно адресовано, посторонние лица его увидеть не должны.

Когда эти данные передаются по сети, их также можно перехватить. Поэтому специалист по безопасности также должен принимать во внимание следующие вопросы: является ли соединение безопасным? Является ли протокол связи безопасным? Является ли сеть безопасной?

**Конфиденциальность** (Confidentiality) гарантирует, что сообщения и другие данные будут храниться в тайне от посторонних лиц или устройств. В области информационных технологий конфиденциальность реализуется в виде **шифрования** данных. Люди используют устройства для выполнения задач, будь то отправка электронной почты, загрузка файла или даже отправка сообщения в мессенджере со смартфона. Важно всегда защищать эти сообщения.

Данные обычно существуют в следующих состояниях:
- **Данные в состоянии покоя** — это данные, которые не используются ни приложением, ни системой. В данный момент они хранятся на носителях, таких как жесткий диск (HDD) в локальной или удаленной системе. Когда данные находятся в состоянии покоя, они уязвимы для злоумышленников, пытающихся либо украсть, либо изменить их. Специалисты по безопасности реализуют как методы аутентификации, так и алгоритмы шифрования для защиты любых данных в состоянии покоя. Примером может служить использование _BitLocker_ в операционной системе Microsoft Windows 10, которая позволяет администратору создавать зашифрованный контейнер, а затем пользователь может поместить файлы в эту специальную область памяти и заблокировать ее.
- **Данные в движении (транзит)** - определяются как данные, которые передаются между источником и пунктом назначения. Представьте, что есть сотрудники, которые работают дистанционно или работают в удаленном месте вдали от офиса. Этим людям может потребоваться часто подключаться к корпоративной сети для доступа к сетевым ресурсам, например, при доступе или работе с документами, находящимися на файловом сервере. Как специалисты в области кибербезопасности, мы должны быть осведомлены о том, какие типы защиты или механизмы безопасности существуют для защиты данных, передаваемых между устройством пользователя и файловым сервером. Кроме того, устройства отправляют и получают сообщения почти каждую секунду, и некоторые из этих сообщений обмениваются с использованием небезопасных протоколов, что позволяет злоумышленнику перехватывать сообщения (данные) по мере их передачи по сети. Если данные передаются по сети в незашифрованном формате, злоумышленник может увидеть все содержимое в виде открытого текста и собрать конфиденциальную информацию, такую как логины и пароли.
	если организация имеет подключение к Интернету и брандмауэры находятся в каждом офисе, специалист по безопасности может настроить VPN между двумя устройствами брандмауэра. Этот тип VPN известен как **site-to-site** VPN. 
	![[Pasted image 20240527092943.png]]
	**site-to-site** VPN устанавливает безопасное зашифрованное соединение между головным офисом и удаленными филиалами через Интернет. Следовательно, любые сообщения, которые перемещаются между офисами, шифруются и отправляются через туннель VPN, защищая сообщения от неавторизованных пользователей. (в случае с **VIPNet сетью - это туннелирование открытых узлов между координаторами**)

	Существует также другой тип VPN - с удаленным доступом (**remote access VPN**). Этот тип позволяет пользователю установить VPN-туннель между конечным устройством, таким как ноутбук, и брандмауэром организации. Этот тип VPN позволяет сотрудникам, работающим дома или вне организации, безопасно подключаться к сети организации и получать доступ к сетевым ресурсам. Имейте в виду, что на устройстве сотрудника должен быть установлен VPN-клиент, который используется для установления безопасного соединения между компьютером и корпоративным брандмауэром.
	![[Pasted image 20240527101351.png]]
	(в случае с **VIPNet сетью - это связь ViPNet Client - Coordinator**)

- **Используемые данные** - это данные, к которым в данный момент обращается или использует приложение. В этом состоянии данные наиболее уязвимы. В качестве примера используемых данных представьте, что вы открываете PDF-документ с помощью приложения для чтения PDF-файлов. Прежде чем приложение сможет успешно открыть файл PDF, документ необходимо расшифровать, если файл защищен паролем. После ввода правильного пароля документ будет представлен пользователю в незашифрованном виде. Важно обеспечить постоянную безопасность системы и приложений, которые обращаются к данным и/или используют их.

Таким образом, **конфиденциальность** — это защита ваших активов от посторонних лиц или устройств.

### Целостность

**Целостность** (Integrity) - это невозможность модификации файла извне (неавторизованными лицами). В сети очень важно обеспечить, чтобы данные или сообщения не изменялись в процессе передачи между источником и получателем.

В кибербезопасности используются специальные алгоритмы хеширования, чтобы помочь пользователям и устройствам проверить, было ли сообщение изменено или нет при его передаче. Алгоритмы хеширования создают односторонний криптографический **хэш** (дайджест, он же контрольная сумма), который является **математическим представлением сообщения**. Это означает, что только это сообщение может выдавать одно и то же хэш-значение. Алгоритмы хеширования создают одностороннюю функцию, которая делает практически невозможным изменение и определение содержимого самого сообщения.

На следующем рисунке показан процесс хеширования сообщения:

![[Pasted image 20240527103224.png]]

Когда пользователь или устройство хочет отправить сообщение адресату, и сообщение, и его хеш-значение упаковываются вместе и отправляются по сети. Когда получатель получает входящее сообщение, он выполняет свою собственную функцию хеширования сообщения и вычисляет его хеш-значение.
Затем получатель сравнивает хеш-значение, полученное от отправителя, с хеш-значением, которое он высчитал. Если оба значения хеш-функций совпадают, это означает, что сообщение не было изменено во время передачи и целостность сохранена. Помимо этого, хэширование также помогает убедиться в том, что сообщение не было искажено в результате каких-либо сбоев или неполадок в сети.

### Доступность

Существуют типы кибератак, которые блокируют или затрудняют доступ законных пользователей к ресурсу. Другими словами, злоумышленники пытаются нарушить **доступность** (_availability_) данных и ресурсов. В области кибербезопасности доступность гарантирует, что данные и ресурсы всегда доступны для пользователей и систем, которым разрешен доступ к этим ресурсам.

Простым примером кибератаки, которая может быть использована для нарушения доступности сети, является атака «распределенный отказ в обслуживании» **Distributed Denial of Service** (DDoS). Этот тип атаки запускается из нескольких географических точек и нацелен на одну систему или сеть. Злоумышленник направляет к объекту атаки огромное количество легитимных запросов или другого типа трафика, в результате чего, система перегружается и становится неспособной обработать запросы от других пользователей. Цель состоит в том, чтобы сделать целевую систему или сеть непригодными для использования или недоступными для других пользователей.

### Объединение трех принципов

Некоторые организации ставят одни принципы выше других. Например, компания может больше внимание уделять на защите своих данных с помощью различных систем аутентификации и шифрования. Этот принцип фокусируется на конфиденциальности. Сосредоточение внимания на одном компоненте, таком как конфиденциальность, в большей степени, чем на других, повлечет меньшее внимание к другим - целостности и/или доступности.
![[Pasted image 20240527103501.png]]
Ключевой момент заключается в том, чтобы всегда обеспечивать баланс при реализации конфиденциальности, целостности и доступности в любой системе и сети. Важно уделять одинаковое внимание всем основным направлениям просто для того, чтобы не было недостатка ни в одном аспекте информационной безопасности.
