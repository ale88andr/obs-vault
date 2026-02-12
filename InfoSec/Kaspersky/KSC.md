
### Компоненты KSC

- Сервер администрирования
- Агент администрирования
- Консоль администрирования на базе ММС
- Web Console

### Пути установки KSC

- `%ProgramFiles(x86)%\Kaspersky Lab\Kaspersky Security Center` —
программные файлы
- `%ProgramFiles%\Kaspersky Lab\Kaspersky Security Center Web Console 13`
— программные файлы
- `%ProgramDafa%\KasperskyLab\adminkit` — настройки
- `%ProgramDafa%\KasperskySC\SC_Backup` — папка для резервных копий
- KLSHARE — локальный путь `%ProgramDafa%lKasperskyLab\adminkit\1093\.working\Share`

### Службы KSC

- Сервер администрирования Kaspersky Security Center
- Агент администрирования Kaspersky Security Center
- Объект автоматизации Kaspersky Security Center
- Прокси-сервер Kaspersky Security Network
- Веб-сервер «Лаборатории Касперского»
- Прокси-сервер активации «Лаборатории Касперского»
- Kaspersky Security Center 13 Management Service
- Kaspersky Security Center 13 Web Console
- Kaspersky Security Center 13 Web Console Message Queue

##### В результате стандартной установки:

**Группы**:	
- `KLAdmns`
- `KLOparators`

**Учетные записи**:	
- `KL-AK-* ` — запускает службу Сервер администрирования Kaspersky Security Center
- `KIScSvc` — запускает службы Прокси-сервер активации «Лаборатории Касперского», Прокси-сервер Kaspersky Security Network и Веб-сервер «Лаборатории Касперского».

> Записи `KL-AK-<*>` и `KIScSvc` имеют права, эквивалентные правам локального администратора, хотя и не входят во встроенную группу администраторов `KIPxeUser`— служебный пользователь для РХЕ-сервера

**Порты**:
- `8060` — http-порт Веб-сервера «Лаборатории Касперского»
- `8061` — https-порт Веб-сервера «Лаборатории Касперского»
- `13000` — для SSL-подключений Агентов
- `14000` — для обычных подключений Агентов и Консолей администрирования
- `13291` — для SSL-подключений Консолей администрирования
- `13111` — порт службы Kaspersky Security Network proxy server
- `17000` — порт прокси-сервера активации
- `13299` — для SSL-подключений Kaspersky Security Center Web Console

**SQL-сервер**:
Имя базы — KAV

