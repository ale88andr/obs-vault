### **Краткая историческая справка:**

История TLS начинается в 1995 году, когда компания Netscape создала предшественника TLS, протокол SSL. SSL был далек от совершенства, и в обеих версиях SSL 2.0 и SSL 3.0 были дефекты, относящиеся к безопасности. SSL нельзя использовать ни в коем случае! Важно так же помнить, что не все версии TLS безопасны. Всего на текущий момент существует три версии TLS:

**TLS 1.0 (1999)** – наименее безопасная версия, но безопаснее чем SSL 3.0;

**TLS 1.1 (2006)** – безопаснее чем версия 1.0, но включает, по текущим меркам, ряд нестойких алгоритмов;

**TLS 1.2 (2008)** – версия, обеспечивающая высокую безопасность. Однако версия 1.2 унаследовала множество функций и проектных решений от прежних версий, поэтому не оптимальна с точки зрения безопасности и производительности. К примеру, протокол данной версии поддерживает AES в режиме CBC, который уязвим к атакам на оракула дополнения;

TLS 1.3 (2018) – данная версия протокола есть результат полной переработки предыдущих версий. Чтобы расчистить завалы, инженеры-криптографы почти заново изобрели TLS оставив только лучшее от предшественников и добавив самые актуальные средства безопасности. По факту TLS 1.3 – это зрелый TLS.

## **TLS**

**TLS –** протокол обеспечения безопасного обмена информацией между двумя точками. Обычно этими точками являются клиенты (браузеры, приложения на мобильных устройствах) пользователей с одной стороны и web сервера (сервисы) с другой стороны.

Если обратиться к модели TCP/IP, то на каком же уровне работает TLS? правильный ответ: **между прикладным и транспортным** (TCP - TLS полагается на TCP).

TLS берет данные от прикладного уровня, обрабатывает их в соответствии с внутренней логикой реализации и передает дальше TCP. Протоколу безопасной передачи без разницы какие данные передавать. В этом плане он очень похож на TCP и подходит для любых приложений.

![[Pasted image 20240606143922.png]]

**Протокол подтверждения связи (TLS handshake) –** протокол выработки ключа TLS. Процедура инициируется клиентом, желающим установить безопасное соединение. Клиент посылает начальное сообщение **ClientHello**, содержащее параметры, в т.ч. шифр, который хочет использовать клиент.

#### Процесс подтверждения связи в TLS 1.3 при подключении к HTTPS - сайту

![[Pasted image 20240606144332.png]]

*ClientHello: поддерживаемые шифры и версия протокола*

Сервер получает сообщение **ClientHello**, проверяет его и отправляет клиенту **ServerHello**:

![[Pasted image 20240606144520.png]]

Помимо **Server Hello**, сервер передает заголовки: **Certificate**, **Server Key Exchange**, **Server Hello Done**.

Заголовки могут передаваться в разных **TCP** сегментах так как размер сегмента может быть ограничен. Если **MSS** (Maximum segment size) позволяет, то сервер может уместить все в один сегмент. Но помнить важно следующее: обращайте внимание на заголовок **Server Hello Done**. Именно он сигнализирует о том, что сервер передал клиенту все что хотел.

## **Certificate**

Самый важный шаг процедуры квитирования (подтверждения связи) – **проверка сертификата**.

Сервер предъявляет клиенту сертификат, подтверждая тем свою **подлинность**. Еще говорят по-другому: предъявляя сертификат сервер аутентифицирует себя клиенту. 

**Сертификат** – это, по факту, открытый ключ, дополненный подписью данного ключа, и дополнительная информация (в т.ч. доменное имя и цепочка сертификации). На браузер или клиентское приложение возлагается задача проверки **подлинности сертификата** и **легитимность его предъявления**. Подлинность сертификата проверяется по средством подтверждения/не подтверждения данного факта цепочкой удостоверяющих центров, вплоть до корневого центра (CA). Проверив подлинность проверяется **подпись сертификата**. На этом этапе клиент сравнивает получившийся хеш с подписью. Если равенство выполняется, то это подтверждает факт обладания сервера **секретным ключом**. Доменное же имя дает возможность дополнительной проверки на предмет подмены адреса сервера. Браузер производит резолв указанного доменного имя и сравнивает полученный адрес с адресом, участвующим в текущем сеансе установления TLS соединения. Если адреса соответствуют друг другу, то можно довериться серверу и продолжить взаимодействие.

![[Pasted image 20240606145619.png]]

В Wireshark есть полезная возможность экспорта сертификата.

![[Pasted image 20240606145645.png]]Сохраняем файл в формате *.crt и открываем его:

![[Pasted image 20240606150040.png]]

### **Server Key Exchange и Client Key Exchange**

Конечная цель TLS заключается в том, чтобы клиент и сервер могли выработать master keys для симметричного шифрования передаваемого трафика. Если обратиться к нашей схеме, то речь об этом:

**На стороне клиента:**

`keys = DH(c,S)`, где `c` - закрытый ключ клиента, `S` - ephemeral (сеансовый) публичный ключ сервера.

**На стороне сервера:**

`keys = DH(s,C)`, где `s` - закрытый ключ c сервера, `С` - ephemeral (сеансовый) публичный ключ клиента.