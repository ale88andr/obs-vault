
Если вы специалист по безопасности, то вам потребуется часто анализировать хосты, если будет замечена подозрительная активность. Чтобы дольше оставаться незамеченными, злоумышленники зачастую используют совершенно легитимные инструменты и процессы, которые можно найти в любой ОС **Microsoft Windows**.

Поэтому важно понимать, как Windows обрабатывает процессы и какие встроенные инструменты может использовать специалист по безопасности для анализа активностей на хосте.

---

### Процессы, потоки и службы

В Windows, когда приложение запущено, оно создает процесс. Обычно приложение может иметь один или несколько выделенных ему процессов. **Процесс** - это все ресурсы, необходимые для обеспечения возможности выполнения/запуска приложения в операционной системе хоста. Представьте, что вы открываете диспетчер задач, чтобы проверить производительность вашего компьютера. Операционная система создаст процесс со всеми необходимыми ресурсами для этого приложения.

На следующем рисунке показаны текущие процессы на компьютере с Windows 10:

![[Pasted image 20240122102938.png]]

Как показано на предыдущем рисунке, диспетчер задач - это служебная программа, которая предоставляет информацию о процессах, службах и производительности устройства. На вкладке «Процессы» вы увидите список всех запущенных в данный момент приложений в операционной системе хоста, список фоновых процессов и ресурсы, которые выделяются каждому приложению (ЦП, память).

Фоновые процессы в Windows выполняются как службы. **Служба** - это программа, которая выполняется в фоновом режиме операционной системы, обеспечивая поддержку приложения и/или операционной системы. Эти службы можно настроить на автоматический запуск при загрузке Windows. Вы можете запускать, останавливать и перезапускать службу вручную.

На следующем снимке экрана показано окно панели управления службами в операционной системе Windows 10:

![[Pasted image 20240122102954.png]]

На предыдущем рисунке показан список служб в операционной системе хоста. Здесь вы можете настроить свойства службы в Windows. Дважды щелкнув на службу, откроется окно свойств.

На следующем рисунке показано окно свойств службы:

![[Pasted image 20240122103008.png]]

Как показано на предыдущем рисунке, вы можете настроить тип запуска службы.

Каждое приложение создает родительский процесс с одним или несколькими дочерними процессами, иногда называемыми потоком. Каждый дочерний процесс или поток отвечает за функцию, обеспечивающую выполнение приложения. Когда приложение выполняется в операционных системах Microsoft Windows, родительский процесс использует системный вызов `fork()`, который позволяет родительскому процессу для запущенного приложения создать один или несколько дочерних процессов. Однако имейте в виду, что у дочернего процесса может быть только один родительский процесс, а у родительского процесса может быть несколько дочерних процессов.

Когда приложение выполняется операционной системой или пользователем, операционная система задействует физическую память из оперативной памяти и создает виртуальную память для выделения запущенному процессу или дочернему процессу. Таким образом, процессы выполняются в виртуальном адресном пространстве операционной системы.

> Важное примечание! Операционная система Windows управляет выделением виртуальной памяти процессу.

Иногда, когда приложение закрывается, родительский процесс и все дочерние процессы завершаются, тем самым высвобождая ресурсы обратно операционной системе. Однако родительский процесс может завершиться, пока дочерние процессы остаются активными. В этой ситуации виртуальная память и любые другие ресурсы по-прежнему выделяются каждому дочернему процессу. Дочерний процесс, у которого нет родительского процесса, называется сиротским процессом (**orphan process**). Пользователь может вручную завершить дочерний процесс в диспетчере задач или выполнить перезагрузку системы. Перезагрузка системы завершит все процессы и перезагрузит операционную систему.

На следующем рисунке показан список всех запущенных процессов на вкладке «Подробности» в диспетчере задач:

![[Pasted image 20240122103104.png]]

Как показано на предыдущем снимке экрана, мы можем видеть все процессы, идентификатор процесса (**PID**) для каждого процесса, статус, какой пользователь запускает процесс, и распределение ресурсов. Также в Microsoft Windows существует утилита Resource Monitor Монитор ресурсов предоставляет более подробную информацию обо всех процессах и о том, как они используют ЦП, память диск и сеть на устройстве.

На следующем рисунке показан интерфейс монитора ресурсов на компьютере с Windows 10:

![[Pasted image 20240122103121.png]]

Еще одним инструментом, который поможет вам определить адресное пространство и распределение памяти в Microsoft Windows, является инструмент **RAMMap**, который входит в набор инструментов Windows Sysinternals от Microsoft.

На следующем рисунке показан сводный список и список подкачки на хосте, использующем RAMMap:

![[Pasted image 20240122103151.png]]

Как показано на предыдущем снимке экрана, RAMMap показывает сводную информацию о выделении виртуальной памяти и ее использовании. Кроме того, вкладка Процессы содержит полный список всех процессов:

![[Pasted image 20240122103216.png]]

Как показано на предыдущем рисункеа, на этой вкладке вы можете увидеть каждый запущенный процесс и распределение виртуальной памяти. Этот инструмент действительно полезен для демонстрации того, как ваша операционная система распределяет физическую память и сколько памяти используется в качестве кэша для данных на устройстве.

---

### Файл подкачки Windows

По мере того, как в память загружается больше приложений, операционная система выделяет части физической памяти (ОЗУ) каждому процессу, используя виртуальную память. Каждый родительский процесс и его дочерние процессы выполняются в одном виртуальном адресном пространстве в операционной системе хоста. Как уже упоминалось, за выделение памяти отвечает операционная система, однако есть некоторые приложения, которым для бесперебойной работы требуется намного больше памяти, чем другим, и это может создать нехватку доступной памяти для других приложений.

Операционная система Windows использует часть памяти из другой области, с жесткого диска или SSD. Windows берет небольшую часть памяти с локального диска и преобразует ее в виртуальную память. Это называется **файлом подкачки**.

Файл подкачки позволяет операционной системе хоста использовать эту часть памяти для загрузки приложений и, следовательно, снижает нагрузку на физическую память (ОЗУ) в системе.

Чтобы получить доступ к настройкам файлов подкачки, выполните следующие действия:

- Щелкните значок Windows в нижнем левом углу экрана и выберете _Система_. Откроется окно _«Система»_. Слева выберите пункт «Дополнительные параметры системы».
- Откроется окно _«Свойства системы»_. Выберите пункт _«Дополнительно»_ и нажмите _«Параметры»_ в разделе _«Быстродействие»_, как показано ниже:![[Pasted image 20240122103250.png]]
- Откроется окно параметров быстродействия. Чтобы изменить размер файла подкачки, нажмите «Изменить…»:![[Pasted image 20240122103315.png]]

Вам будет предоставлена возможность настроить размер файла подкачки для всех дисков в локальной системе. Размер файла подкачки по умолчанию зависит от объема оперативной памяти хост-системы. Операционная система Windows 10 автоматически управляет размером файла подкачки в зависимости от конфигурации хоста и объема оперативной памяти в системе.

Windows использует файл подкачки в качестве виртуальной памяти в случае, если в ОЗУ недостаточно физической памяти.

---

### Реестр Windows

Вся информация о конфигурациях и настройках операционной системы Windows и ее пользователей хранится в базе данных, известной как _реестр_ (**registry**). Самый высокий уровень реестра известен как куст. В реестре Windows есть пять кустов, и каждое значение данных хранится в разделе или подразделе куста. Древо (куст) реестра — это подмножество разделов, подразделов и параметров реестра, которому сопоставлен набор вспомогательных файлов, содержащих резервные копии этих данных. Часто для обозначения конкретных путей в реестре применяют термин ветка. Например ветка реестра `HKEY_LOCAL_MACHINESYSTEM.`

Ниже перечислены пять кустов и их функции в Windows:

- `HKEY_CLASSES_ROOT (HKCR)`: этот куст отвечает за правильное выполнение всех текущих приложений в проводнике Windows. Кроме того, этот куст содержит сведения о ярлыках и правилах перетаскивания в операционной системе хоста.
- `HKEY_CURRENT_USER (HKCU)`: этот куст хранит информацию о текущей учетной записи пользователя в локальной системе. Эта информация будет включать настройки панели управления, настройки папок и настройки персонализации пользователя.
- `HKEY_LOCAL_MACHINE (HKLM)`: этот куст отвечает за хранение специфичных для оборудования деталей операционной системы, таких как конфигурация системы и подключенные диски.
- `HKEY_USERS (HKU)`: содержит данные конфигурации профилей пользователей в локальной системе.
- `HKEY_CURRENT_CONFIG (HKCC)`: содержит подробную информацию о текущих конфигурациях системы.

Для доступа к реестру используйте **Registry Editor** (`regedit`) в строке поиска Windows.

На следующем рисунке показан редактор реестра Windows:

![[Pasted image 20240122103340.png]]

Как показано на предыдущем рисунке, вы можете видеть, что каждый куст находится в верху своего уровня. Если вы развернете куст, вы увидите папки, а в каждой папке есть ключи, которые содержат сведения о конкретной функции или конфигурации в операционной системе.

Реестр может предоставить ценную информацию во время расследования. В каждом реестре есть значение, известное как LastWrite, которое просто указывает время последнего изменения объекта или файла. Эта информация может использоваться для определения времени инцидента или события, связанного с безопасностью. В реестре также содержатся сведения о приложениях, которые запускаются автоматически со стартом системы - AutoRun. Для закрепления в системе, злоумышленники часто модифицируют ветки реестра, которые отвечают за автоматический запуск процессов и сервисов и добавляют в них ссылки на вредоносные программы, чтобы они каждый раз запускались со стартом системы и могли пережить перезагрузку.

Ниже приведены основные ветки, отвечающие за автоматический старт приложений и сервисов:

- `[HKEY_LOCAL_MACHINESoftwareMicrosoftWindowsCurrentVersionRunOnce]`
- `[HKEY_LOCAL_MACHINESoftwareMicrosoftWindowsCurrentVersionRun]`
- `[HKEY_LOCAL_MACHINESoftwareMicrosoftWindowsCurrentVersionRunServices]`
- `[HKEY_LOCAL_MACHINESoftwareMicrosoftWindowsCurrentVersionRunServicesOnce]`
- `[HKEY_LOCAL_MACHINESoftwareMicrosoftWindows NTCurrentVersionWinlogonUserinit]`
- `[HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionRun]`
- `[HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionRunOnce]`
- `[HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionRunServices]`
- `[HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionRunServicesOnce]`
- `[HKEY_CURRENT_USERSoftwareMicrosoftWindows NTCurrentVersionWindows]`

Например, чтобы при каждом старте Windows запускался блокнот, в ветку `[HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionRun]` можно добавить новое значение и указать в нём ссылку на исполняемый файл на компьютере - `"Notepad"="c:windows notepad.exe"`

---

### Windows Management Instrumentation

Управлять несколькими компьютерами с ОС Windows в небольшой сети очень просто. Однако по мере роста сети и подключения все большего числа устройств на базе Windows управление политиками и службами на уровне приложений может стать сложной задачей. **Windows Management Instrumentation** (WMI) - это инструмент, встроенный в операционную систему Windows, который позволяет системному администратору или специалисту по безопасности управлять многими системами на базе Windows в корпоративной сети.

WMI - это функция администрирования Windows, которая обеспечивает единую среду для локального и удаленного доступа к системным компонентам Windows, а также позволяет собирать статистическую информацию об удаленных компьютерах в вашей сети. Вы сможете собирать статистику как по оборудованию, так и по программному обеспечению и даже отслеживать состояние каждого устройства.

Чтобы открыть WMI на компьютере с Windows, выполните следующие действия:

- Откройте приложение _Computer Management_ (управление компьютером) в Windows 10/Server 2019.
- Слева разверните _«Службы и приложения»_.
- Щелкните правой кнопкой мыши элемент управления WMI и выберите _«Свойства»_.

На следующем рисунке показан интерфейс свойств элемента управления WMI:

![[Pasted image 20240122103427.png]]

Злоумышленники могут использовать инструментарий управления Windows (WMI) для удаленного управления. WMI работает через протоколы **SMB** и службу удаленного вызова процедур (**RPCS**) для удаленного доступа. RPCS работает через порт 135.

WMI управляется через утилиту командной строки **WMI command-line** (WMIC). Пример команды - `wmic process call create`.

Использование WMI должно быть ограничено и ограничиваться только авторизованными пользователями, внимательно следя за его использованием.

---

### Инструменты мониторинга

В операционной системе Windows существует множество инструментов мониторинга, которые специалист по безопасности может использовать для мониторинга различных ресурсов и действий на устройстве. Одним из таких инструментов является **Performance Monitor**, который позволяет пользователю собирать более подробные данные, чем ранее упомянутый Resource Monitor. Монитор производительности (системный монитор) - это основной инструмент, используемый как в Windows 10, так и в Windows Server. Специалист по безопасности может использовать этот инструмент для сбора статистики о системе за различные периоды времени, например, часы или дни. Затем собранные данные можно проанализировать на предмет любых аномалий.

На следующем рисунке показан монитор производительности в системе Windows 10/Server 2019:

![[Pasted image 20240122103452.png]]

Еще одним отличным инструментом, встроенным в Windows, является **Монитор стабильности системы**. Монитор стабильности системы позволяет специалисту по безопасности просматривать историю проблем, возникших в основной системе в течение нескольких дней или недель. Пользователь может нажать на событие в инструменте, чтобы получить подробную информацию о проблеме, и существует система оценок от 1 до 10, отражающая серьезность проблемы.

На следующем рисунке показан Монитор стабильности системы на компьютере с Windows 10:

![[Pasted image 20240122103506.png]]

Как показано на предыдущем рисунке, в системе произошел ряд критических событий за определенный период времени. Выбрав событие, монитор покажет подробную информацию о службе или приложении, вызвавшем событие, сводку и время его возникновения. Специалист по безопасности может использовать статистику и информацию, найденные здесь, чтобы лучше понять, вызвало ли вредоносное ПО или неавторизованные приложения нарушение безопасности в хост-системе.

---

### Инструменты мониторинга

Когда в системе Windows происходит какое-либо событие, создается сообщение журнала о данном событии. Инструмент, позволяющий анализировать данные события - **Event Viewer**. Event Viewet содержит журналы безопасности, приложений и системных событий.

Представьте, что злоумышленник пытается войти в учетную запись пользователя с неверными учетными данными. Для каждой попытки создастся событие сообщения журнала, и по этим данным можно будет обнаружить атаку. Существует 4 основных журнала событий:

- **Security Безопасность** - хранит события безопасности
- **Application Приложение** - хранит журналы приложений в ситстеме и установленного ПО
- **Setup Установка** - хранит сведения об установке ОС
- **System Система** - хранит сведения о работе самой системы

На следующем рисунке показан инструмент просмотра событий в Windows (Event Viewer):

![[Pasted image 20240122103527.png]]

Если вы развернете категорию, такую как **Безопасность**, вы увидите список всех журналов, связанных с безопасностью. На следующем снимке экрана показаны сведения в окне просмотра событий входа в систему безопасности:

![[Pasted image 20240122103540.png]]

Информация, содержащаяся в сообщениях журнала, помогает специалисту по безопасности определить, что, когда и как произошел инцидент в системе.

Обратите внимание на код события - **4624**. Этот код соответствует успешному входу в системе Windows. В случае неуспешного входа сгенерируется событие с кодом **4625**. Данные события также будут содержать другую полезную информацию, такую как: имя пользователя, осуществляющего вход, информацию о системе с которой осуществляется вход, тип входа (интерактивный, удаленный, сетевой, вход сервиса), процесс входа, ID входа и другое.

Другие важные коды событий в системах Windows, хранящиеся в журнале Безопасности/Security (начиная с версии 7):

- **4725** - Отключение Учетной записи
- **4723** - Изменение пароля учетной записи
- **4724** - Сброс пароля для учетной записи
- **47204726** - Создание/удаление пользователя
- **4648** - вход с явным указанием учетных данных
- **4698** - Создание задачи через планировщик задач
- **4697** - Создание службы в системе
- **46884689** - Создание/завершение процесса
