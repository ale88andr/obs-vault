
```sql
-- подключиться к postgres (утилита psql)
psql -U postgres

-- команды помощи
help
\h -- помощь по командам SQL
\? -- помощь по командам psql

-- выход из консоли postgres
\q

-- создать базу
CREATE DATABASE my_database;

-- подключиться к базе
\connect my_database;

-- создать таблицу my_table с полями field1 (тип целочисленный, обязательное для заполнения), field2 (тип строка 255 символов)
CREATE TABLE my_table (field1 INT NOT NULL, field2 VARCHAR(255));

-- вывести все таблицы
\d

-- удалить таблицу my_table
DROP TABLE my_table;

-- внести в таблицу запись
INSERT INTO my_table(field1, field2) VALUES(1,'Any text value');

-- вывести записи
SELECT * FROM my_table; -- все записи
SELECT * FROM my_table WHERE field1 = 1; -- все, где field1 = 1 
SELECT * FROM my_table WHERE field1 != 1; -- и т д
SELECT * FROM my_table WHERE field1 > 1;
SELECT * FROM my_table LIMIT 100; -- первые 100 записей;
SELECT * FROM my_table LIMIT 100 OFFSET 200; -- запись с 201 по 300;

-- сортировка при выводе
SELECT * FROM my_table ORDER BY field1 ASC; -- вывести отсортировав в возрастающем порядке
SELECT * FROM my_table ORDER BY field1 DESC; -- вывести отсортировав в убывающем порядке

-- изменить запись таблицы (поле field2 строки, где field1 = 1);
UPDATE my_table SET field2 = 'Other text value' WHERE field1 = 1;

-- удаление данных
DELETE FROM my_table; -- удалить все записи;
DELETE FROM my_table WHERE field1 = 1; -- удалить запись где field1 = 1;


-- ***************************************
-- нормализация (разбиение таблиц на несколько)
-- ***************************************

-- Constraints - ограничения типов данных
CREATE TABLE my_table (
  field1 INT NOT NULL, -- запись обязательна
  field2 VARCHAR(255) NOT NULL UNIQUE, -- запись должна быть уникальной
  field3 BOOLEAN NOT NULL DEFAULT TRUE -- значение по умолчанию - true
  ...
);

-- Первичный и внешние ключи
-- при создании записи таблицы с отсутствующим внешним ключом выведется запись об ошибке. будут выводится ошибки и в иных случаях, когда будут нарушаться связи.
CREATE TABLE IF NOT EXISTS my_table ( -- ключ IF NOT EXISTS проверяет, существует ли таблица.
  field1 SERIAL INT PRIMARY KEY, -- при добавлении PRIMARY KEY поле автоматически наследует ограничения NOT NULL и UNIQUE, и создается индекс. SERIAL тип данных являющийся автоматически увеличивающимся счетчиком (аналог ключа AUTOINCREMENT в Sqlite)
  field2 VARCHAR(255) NOT NULL UNIQUE,
  field3 INT NOT NULL,
  FOREIGN KEY(field3) REFERENCES other_table(field_name) -- поле ссылается на внешнюю таблицу other_table на поле field_name, которое обязательно должно быть с PRIMARY KEY
);

-- вывод данных из нескольких таблиц со связанными полями
SELECT * FROM table_1 LEFT JOIN table_2 ON (table_2.field = table_1.field);

-- алиасы, нужны для удобства. Также, при выводе наименование таблиц или полей выводится алиасом, при его наличии.
SELECT * FROM table_1 as tab1 LEFT JOIN table_2 as tab2 ON (tab1.field = tab2.field);


-- ***************************************
-- Редактирование таблиц, расширенные возможности SELECT, функции
-- ***************************************

-- Добавление поля в таблицу
ALTER TABLE table_name ADD COLUMN new_field
BOOLEAN NOT NULL DEFAULT TRUE;

-- добавление поля с автоинкрементом и primary key в таблицу
ALTER TABLE test1 ADD COLUMN id SERIAL PRIMARY KEY;

-- Удаление поля из таблицы
ALTER TABLE table_name DROP COLUMN new_field;

-- переименовать поле
ALTER TABLE table_name RENAME old_field TO new_field;

-- сменить тип данных
ALTER TABLE table_name ALTER COLUMN any_field SET
DATA TYPE VARCHAR(255);

-- изменить значение по умолчанию
ALTER TABLE table_name ALTER COLUMN any_field SET
DEFAULT 'new value';

-- добавить/удалить constraint NOT NULL
ALTER TABLE table_name ALTER COLUMN any_field
SET|DROP NOT NULL;

-- переименовать таблицу
ALTER TABLE table_name RENAME TO new_table_name;

-- Расширенные возможности SELECT
SELECT * FROM table WHERE field1 LIKE 'value'; -- field1 = 'value'
SELECT * FROM table WHERE field1 LIKE 'val%'; -- field1 начинается с 'val'
SELECT * FROM table WHERE field1 LIKE '%lue'; -- field1 заканчивается на 'lue'
SELECT * FROM table WHERE field1 LIKE '%e%'; -- field1 содержит 'e'
-- несколько условий
SELECT * FROM table WHERE field1 = 'value' AND field2 > 'value2';
SELECT * FROM table WHERE field1 = 'value' OR field2 > 'value2';

-- вывод уникальных записей
SELECT DISTINCT field1 FROM table;

-- группирование записей

SELECT field1, COUNT(field1) FROM table GROUP BY field1;
-- сгруппирует записи таблицы table по полю field и выведет уникальные значения field и количество повторений

SELECT field1, COUNT(field1) FROM table GROUP BY field1
HAVING COUNT(field) > 3;
-- сгруппирует записи таблицы table по полю field и выведет уникальные значения field и количество повторений, где количество повторений больше 3

Полезные ключи программы psql

-U - Указываем пользователя, например postgres
-W - Приглашение на ввод пароля
-d название_БД - Подключение к БД название_БД
-h имя_хоста - Подключение к хосту имя_хоста
-p порт - По какому порту постгря ожидает подключения
-c команда - Выполнение команды SQL без выхода в интерактивный режим
-f file.sql - Выполнение команд из файла file.sql
-S - Однострочный режим, то есть, переход на новую строку будет выполнять запрос (избавляет от ; в конце конструкции SQL)

Полезные psql команды

\? - Справка по командам psql
\h - Справка по SQL: список доступных команд или синтаксис конретной команды
\q - Выход из psql
\c название_БД - Выбрать базу данных
\l - Список баз данных
\s имя-файла-для-сохранения-истории-команд
\dt - Список таблиц
\di - Список индексов
\du - Список пользователей
\dv - Список представлений
\df - Список функций
\dn - Список схем
\dx - Список установленных расширений
\dp - Список привилегий
\d имя - Подробная информация по конкретному объекту
\d+ имя - еще более подробная информация по конкретному объекту
\di+ имя
\x - Переключает обычный табличный вывод (столбцы и строки) на расширенный (каждый столбец на отдельной строке) и обратно. Удобно для просмотра нескольких "широких" строк

Полезные команды
CREATE DATABASE db_name; - Создаем БД с названием db_name
CREATE USER db_user WITH PASSWORD 'db_user_pw'; - Создаем пользователя db_user с паролем db_user_pw
GRANT ALL PRIVILEGES ON DATABASE db_name to db_user; - Даем ВСЕ права пользователю db_user на базу db_name
SELECT rolname FROM pg_roles; - Глянуть все роли в базе
SELECT session_user; - Глянуть текущего пользователя, под которым выполняется сеанс

SELECT * FROM pg_stat_activity WHERE state = 'active'; - Получить все выполняемые запросы на сервере
SELECT pg_cancel_backend(<pid of the process>); - kill'яем неугодный процесс
SELECT pg_terminate_backend(<pid of the process>); - если процесс не может быть kill'ен, то пробуем это. Более сильная магия
SELECT pg_size_pretty(pg_database_size(current_database())); - получить красивый размер бд
SELECT pg_relation_size('accounts'); - получить размер конкретной таблицы


/usr/local/pgsql/data/postgresql.conf - обычно здесь лежит конфиг

```
## Базовые команды

Подключение к инстансу на локалхосте под пользователем postgres:

```console
$psql -U postgres -h localhost
```

Список баз:

```postgresql
\l
```

Подключиться к базе:

```postgresql
\c dbname
```

Cписок таблиц в базе:

```postgresql
\dt
```

Cписок таблиц в базе, в названии которых есть _mytable_:

```postgresql
\dt *mytable*
```

Cписок индексов:

```postgresql
\di
```

Включает или выключает вывод результата списком, а не таблицей:

```postgresql
\x
```

Вот пример вывода таблицей и списком:

```postgresql
postgres=# select * from pg_user;
     usename     | usesysid | usecreatedb | usesuper | userepl | usebypassrls |  passwd  | valuntil | useconfig
-----------------+----------+-------------+----------+---------+--------------+----------+----------+-----------
 postgres        |       10 | t           | t        | t       | t            | ******** |          |
 testuser        |    16321 | f           | f        | f       | f            | ******** |          |
(2 rows)

postgres=# \x
Expanded display is on.
postgres=# select * from pg_user;
-[ RECORD 1 ]+----------------
usename      | postgres
usesysid     | 10
usecreatedb  | t
usesuper     | t
userepl      | t
usebypassrls | t
passwd       | ********
valuntil     |
useconfig    |
-[ RECORD 2 ]+----------------
usename      | testuser
usesysid     | 16321
usecreatedb  | f
usesuper     | f
userepl      | f
usebypassrls | f
passwd       | ********
valuntil     |
useconfig    |
```

## Работа с пользователями

Список пользователей:

```postgresql
\du
```

Создать пользователя:

```postgresql
CREATE USER username WITH PASSWORD 'password';
```

Выдать права на базу:

```postgresql
GRANT ALL PRIVILEGES ON DATABASE dbname TO username;
```

Изменить пароль пользователя:

```postgresql
ALTER USER username WITH PASSWORD 'new_password';
```

Удалить пользователя:

```postgresql
DROP USER IF EXISTS username;
```

## Обслуживание

Посмотреть размер всех баз:

```postgresql
SELECT datname, pg_size_pretty(pg_database_size(datname))
FROM pg_database
ORDER BY pg_database_size(datname) DESC;
```

Размер таблиц и индексов:

```postgresql
SELECT
    TABLE_NAME,
    pg_size_pretty(table_size) AS table_size,
    pg_size_pretty(indexes_size) AS indexes_size,
    pg_size_pretty(total_size) AS total_size
FROM (
    SELECT
        TABLE_NAME,
        pg_table_size(TABLE_NAME) AS table_size,
        pg_indexes_size(TABLE_NAME) AS indexes_size,
        pg_total_relation_size(TABLE_NAME) AS total_size
    FROM (
        SELECT ('"' || table_schema || '"."' || TABLE_NAME || '"') AS TABLE_NAME
        FROM information_schema.tables
    ) AS all_tables
    ORDER BY total_size DESC
) AS pretty_sizes;
```

Список всех работающих запросов:

```postgresql
SELECT 
  pid,
  age(clock_timestamp(), query_start),
  usename,
  query 
FROM pg_stat_activity 
WHERE query != '<IDLE>' AND query NOT ILIKE '%pg_stat_activity%' 
ORDER BY query_start desc;
```

Список запросов работающих дольше 5 минут:

```postgresql
SELECT
  pid,
  now() - pg_stat_activity.query_start AS duration,
  query,
  state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes';
```

Остановить запрос по pid:

```postgresql
SELECT pg_cancel_backend(pid);
```

Принудительно остановить запрос:

```postgresql
SELECT pg_terminate_backend(pid);
```

Остановить запросы работающие дольше 5 минут:

```postgresql
SELECT
  pg_terminate_backend(pid),
  now() - pg_stat_activity.query_start AS duration,
  query,
  state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes';
```

Остановить все запросы:

```postgresql
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE datname = current_database()  
  AND pid <> pg_backend_pid();
```

## Бэкапы

Для бэкапов есть консольные утилиты _pg_dump_ и _pg_dumpall_. Первая делает sql-дамп отдельно взятой базы, а вторая, как следует из названия, делает то же самое для всех баз (+бэкапит пользователей). Есть еще утилита _pg_basebackup_, она делает полный бэкап инстанса на уровне файлов.

Дамп базы в сжатый файл (опция _-C_ добавляет команду CREATE DATABASE в дамп):

```console
$pg_dump -C -U username -h hostname dbname | gzip > dump.sql.gz
```

Полный дамп всех баз и пользователей в сжатый файл:

```console
$pg_dumpall -U username -h hostname | gzip > dump.sql.gz
```

Восстановление базы (или полного дампа, но тогда не нужно указывать dbname) из сжатого файла:

```console
$zcat dump.sql.gz | psql -U username -h hostname dbname
```

## Команды PostgreSQL (Шпаргалка по PostgreSQL)

Доступ к серверу PostgreSQL через **psql** с определенным пользователем:

```
psql -U [username];
```

Например, следующая команда использует пользователя **postgres** для доступа к серверу базы данных PostgreSQL:

```
psql -U postgres
```

Подключение к определенной базе данных:

```
\c database_name;
```

Подключение к базе данных dvdrental:

```
\c dvdrental;
You are now connected to database "dvdrental" as user "postgres".
```


Чтобы выйти из psql:

```
\q
```

Список всех баз данных на сервере PostgreSQL

```
\l
```

Список всех схем:

```
\dn
```

Список всех хранимых процедур и функций:

```
\df
```

Список всех представлений

```
\dv
```

Список всех таблиц в текущей базе данных.

```
\dt
```

Или для получения дополнительной информации о таблицах в текущей базе данных:

```
\dt+
```

Получение подробной информации о таблице.

```
\d+ table_name
```

Показывает хранимую процедуру или код функции:

```
\df+ function_name
```

Показывает вывод запроса в красивом формате:

```
\x
```

Список всех пользователей:

```
\du
```

Создает новую роль:

```sql
CREATE ROLE role_name;
```

Создает новую роль с: `username` и `password`:

```sql
CREATE ROLE username NOINHERIT LOGIN PASSWORD password;
```

Изменяет роль для текущей сессии на  `new_role`:

```sql
SET ROLE new_role;
```

Разрешить  `role_1`  установить свою роль как `role_2`:

```sql
GRANT role_2 TO role_1;
```

## Управление базами данных

Создание новой базы данных:

```sql
CREATE DATABASE [IF NOT EXISTS] db_name;
```

Удаление базы данных навсегда:

```sql
DROP DATABASE [IF EXISTS] db_name;
```

## Управление таблицами

Создание новой или временной таблицы

```sql
CREATE [TEMP] TABLE [IF NOT EXISTS] table_name(
   pk SERIAL PRIMARY KEY,
   c1 type(size) NOT NULL,
   c2 type(size) NULL,
   ...
);
```

Добавление нового столбца в таблицу:

```sql
ALTER TABLE table_name ADD COLUMN new_column_name TYPE;
```

Удаление столбца в таблице:

```sql
ALTER TABLE table_name DROP COLUMN column_name;
```

Переименовать столбец:

```sql
ALTER TABLE table_name RENAME column_name TO new_column_name;
```

Установите или удалите значение по умолчанию для столбца:

```sql
ALTER TABLE table_name ALTER COLUMN [SET DEFAULT value | DROP DEFAULT]
```

Добавление первичного ключа к таблице.

```sql
ALTER TABLE table_name ADD PRIMARY KEY (column,...);
```

Удаление первичного ключа из таблицы.

```sql
ALTER TABLE table_name 
DROP CONSTRAINT primary_key_constraint_name;
```

Переименовать таблицу.

```sql
ALTER TABLE table_name RENAME TO new_table_name;
```

Удаление таблицы и зависимых от нее объектов:

```sql
DROP TABLE [IF EXISTS] table_name CASCADE;
```

## Управление представлениями

Создаем представление:


```
CREATE OR REPLACE view_name AS
query;
```


Создаем рекурсивное представление:


```
CREATE RECURSIVE VIEW view_name(column_list) AS
SELECT column_list;
```


Создайте детализированное представление:


```
CREATE MATERIALIZED VIEW view_name
AS
query
WITH [NO] DATA;
```


Обновление детализированного представления:


```
REFRESH MATERIALIZED VIEW CONCURRENTLY view_name;
```


Удаление существующего представления.


```
DROP VIEW [ IF EXISTS ] view_name;
```


Удаление детализированного представления:


```
DROP MATERIALIZED VIEW view_name;
```


Переименование представления:


```
ALTER VIEW view_name RENAME TO new_name;
```


## Управление индексами

Создание индекса с указанным именем для таблицы

```sql
CREATE [UNIQUE] INDEX index_name
ON table (column,...)
```

Удаление указанного индекса из таблицы

```sql
DROP INDEX index_name;
```

## Запрос данных из таблиц

Запросить все данные из таблицы:

```sql
SELECT * FROM table_name;
```

Запрос данных из указанных столбцов всех строк таблицы:

```sql
SELECT column_list
FROM table;
```

Запрашивает данные и выбирает только уникальные строки:

```sql
SELECT DISTINCT (column)
FROM table;
```

Запрашивает данные из таблицы с помощью фильтра:

```sql
SELECT *
FROM table
WHERE condition;
```

Назначение псевдонима столбцу в наборе результатов:

```sql
SELECT column_1 AS new_column_1, ...
FROM table;
```

Запрос данных с помощью оператора  `LIKE`

```sql
SELECT * FROM table_name
WHERE column LIKE '%value%'
```

Запрос данных с помощью оператора BETWEEN:

```sql
SELECT * FROM table_name
WHERE column BETWEEN low AND high;
```

Запрос данных с помощью оператора IN:

```sql
SELECT * FROM table_name
WHERE column IN (value1, value2,...);
```

Ограничение возвращаемых строк с помощью условия LIMIT:

```sql
SELECT * FROM table_name
LIMIT limit OFFSET offset
ORDER BY column_name;
```

Запрос данных из множества с использованием inner join, left join, full outer join, cross join и natural join:

```sql
SELECT * 
FROM table1
INNER JOIN table2 ON conditions
```

```sql
SELECT * 
FROM table1
LEFT JOIN table2 ON conditions
```

```sql
SELECT * 
FROM table1
FULL OUTER JOIN table2 ON conditions
```


```sql
SELECT * 
FROM table1
CROSS JOIN table2;
```



```sql
SELECT * 
FROM table1
NATURAL JOIN table2;
```


Возвращает количество строк таблицы.


```sql
SELECT COUNT (*)
FROM table_name;
```


Сортировка строк в порядке возрастания или убывания:

```sql
SELECT select_list
FROM table
ORDER BY column ASC [DESC], column2 ASC [DESC],...;
```

Группировка строк с помощью `GROUP BY.`


```sql
SELECT *
FROM table
GROUP BY column_1, column_2, ...;
```


Фильтруйте группы, используя `HAVING.`


```sql
SELECT *
FROM table
GROUP BY column_1
HAVING condition;
```


## Операции

Объединение набора результатов двух или более запросов с помощью оператора  `UNION`:


```sql
SELECT * FROM table1
UNION
SELECT * FROM table2;
```


Минус результат используя оператор `EXCEPT`:


```sql
SELECT * FROM table1
EXCEPT
SELECT * FROM table2;
```


Получаем пересечение наборов результатов двух запросов:


```sql
SELECT * FROM table1
INTERSECT
SELECT * FROM table2;
```


## Изменение данных

Добавляет новую строку в таблицу:


```
INSERT INTO table(column1,column2,...)
VALUES(value_1,value_2,...);
```


Добавляет несколько строк в таблицу:


```
INSERT INTO table_name(column1,column2,...)
VALUES(value_1,value_2,...),
      (value_1,value_2,...),
      (value_1,value_2,...)...
```


Обновление данных для всех строк:


```
UPDATE table_name
SET column_1 = value_1,
    ...;
```


Обновление данных для набора строк, заданных условием в предложении  `WHERE.`


```
UPDATE table
SET column_1 = value_1,
    ...
WHERE condition;
```


Удаление всех строк таблицы:


```
DELETE FROM table_name;
```


Удаление определенных строк на основе условия:


```
DELETE FROM table_name
WHERE condition;
```


## Производительность

Показать план запроса


```
EXPLAIN query;
```


Показать и выполнить план запроса:


```
EXPLAIN ANALYZE query;
```


Сбор статистических данных:


```
ANALYZE table_name;
```
