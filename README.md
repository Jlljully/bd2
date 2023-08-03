# Домашнее задание к занятию 2. «SQL»

## Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose-манифест.

### Ответ

![Скрин](https://github.com/Jlljully/bd2/blob/main/Untitled.png "2")

![Скрин](https://github.com/Jlljully/bd2/blob/main/Untitled2.png "2")

## Задача 2

В БД из задачи 1: 

- создайте пользователя test-admin-user и БД test_db;
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;
- создайте пользователя test-simple-user;
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Таблица orders:

- id (serial primary key);
- наименование (string);
- цена (integer).

Таблица clients:

- id (serial primary key);
- фамилия (string);
- страна проживания (string, index);
- заказ (foreign key orders).

Приведите:

- итоговый список БД после выполнения пунктов выше;
- описание таблиц (describe);
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;
- список пользователей с правами над таблицами test_db.

### Ответ

![Скрин](https://github.com/Jlljully/bd2/blob/main/Screenshot_7.png "2")

![Скрин](https://github.com/Jlljully/bd2/blob/main/Screenshot_8.png "2")

```
SELECT table_catalog,table_name,grantee,privilege_type FROM information_schema.table_privileges WHERE table_schema = 'public';
```

```
test_db=# SELECT table_catalog,table_name,grantee,privilege_type FROM information_schema.table_privileges WHERE table_schema = 'public';
 table_catalog | table_name |     grantee      | privilege_type
---------------+------------+------------------+----------------
 test_db       | orders     | admin            | INSERT
 test_db       | orders     | admin            | SELECT
 test_db       | orders     | admin            | UPDATE
 test_db       | orders     | admin            | DELETE
 test_db       | orders     | admin            | TRUNCATE
 test_db       | orders     | admin            | REFERENCES
 test_db       | orders     | admin            | TRIGGER
 test_db       | orders     | test-admin-user  | INSERT
 test_db       | orders     | test-admin-user  | SELECT
 test_db       | orders     | test-admin-user  | UPDATE
 test_db       | orders     | test-admin-user  | DELETE
 test_db       | orders     | test-admin-user  | TRUNCATE
 test_db       | orders     | test-admin-user  | REFERENCES
 test_db       | orders     | test-admin-user  | TRIGGER
 test_db       | orders     | test-simple-user | INSERT
 test_db       | orders     | test-simple-user | SELECT
 test_db       | orders     | test-simple-user | UPDATE
 test_db       | orders     | test-simple-user | DELETE
 test_db       | clients    | admin            | INSERT
 test_db       | clients    | admin            | SELECT
 test_db       | clients    | admin            | UPDATE
 test_db       | clients    | admin            | DELETE
 test_db       | clients    | admin            | TRUNCATE
 test_db       | clients    | admin            | REFERENCES
 test_db       | clients    | admin            | TRIGGER
 test_db       | clients    | test-admin-user  | INSERT
 test_db       | clients    | test-admin-user  | SELECT
 test_db       | clients    | test-admin-user  | UPDATE
 test_db       | clients    | test-admin-user  | DELETE
 test_db       | clients    | test-admin-user  | TRUNCATE
 test_db       | clients    | test-admin-user  | REFERENCES
 test_db       | clients    | test-admin-user  | TRIGGER
 test_db       | clients    | test-simple-user | INSERT
 test_db       | clients    | test-simple-user | SELECT
 test_db       | clients    | test-simple-user | UPDATE
 test_db       | clients    | test-simple-user | DELETE
(36 rows)


```

## Задача 3

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL-синтаксис:
- вычислите количество записей для каждой таблицы.

Приведите в ответе:

    - запросы,
    - результаты их выполнения.

### Ответ

```
INSERT INTO orders (Наименование, Цена) VALUES ('Шоколад', 10), ('Принтер', 3000), ('Книга', 500), ('Монитор', 7000), ('Гитара', 4000);

INSERT INTO clients (Фамилия, "Страна проживания") VALUES ('Иванов Иван Иванович', 'USA'), ('Петров Петр Петрович', 'USA'), ('Иоганн Себастьян Бах', 'Japan'), ('Ронни Джеймс Дио', 'Russia'), ('Ritchie Blackmore', 'Russia');

```

![Скрин](https://github.com/Jlljully/bd2/blob/main/Screenshot_9.png "2")


## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys, свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения этих операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.
 
Подсказка: используйте директиву `UPDATE`.

### Ответ



## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения.

### Ответ



## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

Остановите контейнер с PostgreSQL, но не удаляйте volumes.

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

### Ответ


