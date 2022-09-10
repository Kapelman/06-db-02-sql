# Домашнее задание к занятию "6.2. SQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/tree/master/additional/README.md).

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose манифест.

Решение:

- создадим необходимы папки:
```
vagrant@vagrant:~$ mkdir /home/vagrant/postgresql
vagrant@vagrant:~$ mkdir /home/vagrant/postgresql/data
vagrant@vagrant:~$ mkdir /home/vagrant/postgresql/backup
vagrant@vagrant:~$ cd /home/vagrant/postgresql
vagrant@vagrant:~/postgresql$ ls -al
total 16
drwxrwxr-x  4 vagrant vagrant 4096 Aug 29 09:26 .
drwxr-xr-x 30 vagrant vagrant 4096 Aug 29 09:25 ..
drwxrwxr-x  2 vagrant vagrant 4096 Aug 29 09:26 backup
drwxrwxr-x  2 vagrant vagrant 4096 Aug 29 09:25 data
```

- пропишем docker - compose файл
```
version: '2.1'
services:
  db:
    image: postgres:12
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - /home/vagrant/postgresql/data:/var/db-data
      - /home/vagrant/postgresql/backup:/var/db-backup

```

- проверим что сервис собирается и стартует
```
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose build
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose create
[+] Running 14/14
 ⠿ db Pulled                                                                                                                                                                                              313.3s
   ⠿ 7a6db449b51b Pull complete                                                                                                                                                                           193.8s
   ⠿ b4f184bc0704 Pull complete                                                                                                                                                                           199.4s
   ⠿ 606a73c0d34a Pull complete                                                                                                                                                                           199.9s
   ⠿ c39f1600d2b6 Pull complete                                                                                                                                                                           201.3s
   ⠿ 31f42f92b0fe Pull complete                                                                                                                                                                           214.1s
   ⠿ c8b67d2b0354 Pull complete                                                                                                                                                                           216.7s
   ⠿ 31107b8480ee Pull complete                                                                                                                                                                           217.0s
   ⠿ b26434cf8bfa Pull complete                                                                                                                                                                           217.4s
   ⠿ add9c6f96747 Pull complete                                                                                                                                                                           306.2s
   ⠿ 03095be05119 Pull complete                                                                                                                                                                           306.6s
   ⠿ 58acdb363e1b Pull complete                                                                                                                                                                           306.8s
   ⠿ 0a3ce596ea85 Pull complete                                                                                                                                                                           307.2s
   ⠿ acd9cd87ac43 Pull complete                                                                                                                                                                           307.9s
[+] Running 2/2
 ⠿ Network docker-compose-yam_default  Created                                                                                                                                                              0.3s
 ⠿ Container docker-compose-yam-db-1   Created                                                                                                                                                              0.6s
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose images
Container                 Repository          Tag                 Image Id            Size
docker-compose-yam-db-1   postgres            12                  f2f1f275f1a1        373MB
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose ps
NAME                      COMMAND                  SERVICE             STATUS              PORTS
docker-compose-yam-db-1   "docker-entrypoint.s…"   db                  created
```
- запустим контейнеры
```
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose build
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose up
[+] Running 1/1
 ⠿ Container docker-compose-yam-db-1  Recreated                                                                                                                                                             0.5s
Attaching to docker-compose-yam-db-1
docker-compose-yam-db-1  | The files belonging to this database system will be owned by user "postgres".
docker-compose-yam-db-1  | This user must also own the server process.
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | The database cluster will be initialized with locale "en_US.utf8".
docker-compose-yam-db-1  | The default database encoding has accordingly been set to "UTF8".
docker-compose-yam-db-1  | The default text search configuration will be set to "english".
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | Data page checksums are disabled.
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | fixing permissions on existing directory /var/lib/postgresql/data ... ok
docker-compose-yam-db-1  | creating subdirectories ... ok
docker-compose-yam-db-1  | selecting dynamic shared memory implementation ... posix
docker-compose-yam-db-1  | selecting default max_connections ... 100
docker-compose-yam-db-1  | selecting default shared_buffers ... 128MB
docker-compose-yam-db-1  | selecting default time zone ... Etc/UTC
docker-compose-yam-db-1  | creating configuration files ... ok
docker-compose-yam-db-1  | running bootstrap script ... ok
docker-compose-yam-db-1  | performing post-bootstrap initialization ... ok
docker-compose-yam-db-1  | syncing data to disk ... initdb: warning: enabling "trust" authentication for local connections
docker-compose-yam-db-1  | You can change this by editing pg_hba.conf or using the option -A, or
docker-compose-yam-db-1  | --auth-local and --auth-host, the next time you run initdb.
docker-compose-yam-db-1  | ok
docker-compose-yam-db-1  |
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | Success. You can now start the database server using:
docker-compose-yam-db-1  |
docker-compose-yam-db-1  |     pg_ctl -D /var/lib/postgresql/data -l logfile start
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | waiting for server to start....2022-08-29 10:20:42.828 UTC [48] LOG:  starting PostgreSQL 12.12 (Debian 12.12-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
docker-compose-yam-db-1  | 2022-08-29 10:20:42.839 UTC [48] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
docker-compose-yam-db-1  | 2022-08-29 10:20:42.914 UTC [49] LOG:  database system was shut down at 2022-08-29 10:20:40 UTC
docker-compose-yam-db-1  | 2022-08-29 10:20:42.935 UTC [48] LOG:  database system is ready to accept connections
docker-compose-yam-db-1  |  done
docker-compose-yam-db-1  | server started
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | waiting for server to shut down...2022-08-29 10:20:43.293 UTC [48] LOG:  received fast shutdown request
docker-compose-yam-db-1  | .2022-08-29 10:20:43.448 UTC [48] LOG:  aborting any active transactions
docker-compose-yam-db-1  | 2022-08-29 10:20:43.451 UTC [48] LOG:  background worker "logical replication launcher" (PID 55) exited with exit code 1
docker-compose-yam-db-1  | 2022-08-29 10:20:43.454 UTC [50] LOG:  shutting down
docker-compose-yam-db-1  | 2022-08-29 10:20:43.514 UTC [48] LOG:  database system is shut down
docker-compose-yam-db-1  |  done
docker-compose-yam-db-1  | server stopped
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | PostgreSQL init process complete; ready for start up.
docker-compose-yam-db-1  |
docker-compose-yam-db-1  | 2022-08-29 10:20:43.561 UTC [1] LOG:  starting PostgreSQL 12.12 (Debian 12.12-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
docker-compose-yam-db-1  | 2022-08-29 10:20:43.563 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
docker-compose-yam-db-1  | 2022-08-29 10:20:43.577 UTC [1] LOG:  listening on IPv6 address "::", port 5432
docker-compose-yam-db-1  | 2022-08-29 10:20:43.712 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
docker-compose-yam-db-1  | 2022-08-29 10:20:43.750 UTC [67] LOG:  database system was shut down at 2022-08-29 10:20:43 UTC
docker-compose-yam-db-1  | 2022-08-29 10:20:43.823 UTC [1] LOG:  database system is ready to accept connections
```

## Задача 2

В БД из задачи 1: 
- создайте пользователя test-admin-user и БД test_db

Решение:
- зайдем в контейнер и сделаем необходимые шаги

```
vagrant@vagrant:~/docker-compose-yam$ sudo docker exec -it docker-compose-yam-db-1 bash
root@c1a5670b3700:~# chown postgres:postgres /var/db-data/

root@c1a5670b3700:/# psql -U postgres
psql (12.12 (Debian 12.12-1.pgdg110+1))
Type "help" for help.


postgres=# CREATE USER "test-admin-user";
CREATE ROLE

postgres=# CREATE TABLESPACE "test-tablespace" OWNER CURRENT_USER
postgres-# LOCATION '/var/db-data'
postgres-# ;
ERROR:  could not set permissions on directory "/var/db-data": Operation not permitted
postgres=# CREATE TABLESPACE "test-tablespace" OWNER CURRENT_USER
LOCATION '/var/db-data'
;
CREATE TABLESPACE
postgres=#


postgres=# CREATE DATABASE "test_db" WITH TABLESPACE = "test-tablespace";
CREATE DATABASE
```
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)

Решение:
```
postgres=# \connect test_db
You are now connected to database "test_db" as user "postgres".


test_db=# CREATE TABLE orders(
    id          serial primary key,
    title       text NOT NULL,
    price       integer
);
CREATE TABLE


test_db=# CREATE TABLE clients(
    id            serial primary key,
    surname       text NOT NULL,
    from_country_id  integer NOT NULL,
    from_country_name text NOT NULL,
    order_id      integer REFERENCES orders
);
CREATE TABLE
```
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db

Решение:
```
test_db=# GRANT ALL PRIVILEGES ON DATABASE test_db to "test-admin-user";
GRANT
test_db=# GRANT ALL PRIVILEGES ON TABLE orders, clients  to "test-admin-user";
GRANT
```
- создайте пользователя test-simple-user  
Решение:
```
test_db=# CREATE USER "test-simple-user";
CREATE ROLE
```
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

Решение:
```
test_db=# GRANT SELECT, INSERT, UPDATE, DELETE ON TABLE orders, clients  to "test-simple-user";
GRANT
```
Таблица orders:
- id (serial primary key)
- наименование (string)
- цена (integer)

Таблица clients:
- id (serial primary key)
- фамилия (string)
- страна проживания (string, index)
- заказ (foreign key orders)

Приведите:
- итоговый список БД после выполнения пунктов выше,

Решение:
```
test_db=# \list
                                     List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |       Access privileges
-----------+----------+----------+------------+------------+--------------------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres                   +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres                   +
           |          |          |            |            | postgres=CTc/postgres
 test_db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =Tc/postgres                  +
           |          |          |            |            | postgres=CTc/postgres         +
           |          |          |            |            | "test-admin-user"=CTc/postgres
```
- описание таблиц (describe)
```
test_db=# \d orders
                            Table "public.orders"
 Column |  Type   | Collation | Nullable |              Default
--------+---------+-----------+----------+------------------------------------
 id     | integer |           | not null | nextval('orders_id_seq'::regclass)
 title  | text    |           | not null |
 price  | integer |           |          |
Indexes:
    "orders_pkey" PRIMARY KEY, btree (id)
Referenced by:
    TABLE "clients" CONSTRAINT "clients_order_id_fkey" FOREIGN KEY (order_id) REFERENCES orders(id)
```
```
test_db=# \d clients
                                  Table "public.clients"
      Column       |  Type   | Collation | Nullable |               Default
-------------------+---------+-----------+----------+-------------------------------------
 id                | integer |           | not null | nextval('clients_id_seq'::regclass)
 surname           | text    |           | not null |
 from_country_id   | integer |           | not null |
 from_country_name | text    |           | not null |
 order_id          | integer |           |          |
Indexes:
    "clients_pkey" PRIMARY KEY, btree (id)
Foreign-key constraints:
    "clients_order_id_fkey" FOREIGN KEY (order_id) REFERENCES orders(id)
```


- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db

- Решение
```
test_db=# SELECT distinct grantee
FROM information_schema.role_table_grants
WHERE table_name in ('orders','clients');
     grantee
------------------
 postgres
 test-admin-user
 test-simple-user
(3 rows)
```


- список пользователей с правами над таблицами test_db

Решение:
```
test_db=# SELECT grantee, table_name, privilege_type
FROM information_schema.role_table_grants
WHERE table_name in ('orders','clients');
     grantee      | table_name | privilege_type
------------------+------------+----------------
 postgres         | orders     | INSERT
 postgres         | orders     | SELECT
 postgres         | orders     | UPDATE
 postgres         | orders     | DELETE
 postgres         | orders     | TRUNCATE
 postgres         | orders     | REFERENCES
 postgres         | orders     | TRIGGER
 test-admin-user  | orders     | INSERT
 test-admin-user  | orders     | SELECT
 test-admin-user  | orders     | UPDATE
 test-admin-user  | orders     | DELETE
 test-admin-user  | orders     | TRUNCATE
 test-admin-user  | orders     | REFERENCES
 test-admin-user  | orders     | TRIGGER
 test-simple-user | orders     | INSERT
 test-simple-user | orders     | SELECT
 test-simple-user | orders     | UPDATE
 test-simple-user | orders     | DELETE
 postgres         | clients    | INSERT
 postgres         | clients    | SELECT
 postgres         | clients    | UPDATE
 postgres         | clients    | DELETE
 postgres         | clients    | TRUNCATE
 postgres         | clients    | REFERENCES
 postgres         | clients    | TRIGGER
 test-admin-user  | clients    | INSERT
 test-admin-user  | clients    | SELECT
 test-admin-user  | clients    | UPDATE
 test-admin-user  | clients    | DELETE
 test-admin-user  | clients    | TRUNCATE
 test-admin-user  | clients    | REFERENCES
 test-admin-user  | clients    | TRIGGER
 test-simple-user | clients    | INSERT
 test-simple-user | clients    | SELECT
 test-simple-user | clients    | UPDATE
 test-simple-user | clients    | DELETE
(36 rows)
```
## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Решение:
```

test_db=# insert into orders(title, price) VALUES ('Шоколад', 10);
INSERT 0 1
test_db=# insert into orders(title, price) VALUES ('Принтер', 3000);
INSERT 0 1
test_db=# insert into orders(title, price) VALUES ('Книга', 500);
INSERT 0 1
test_db=# insert into orders(title, price) VALUES ('Монитор', 7000);
INSERT 0 1
test_db=# insert into orders(title, price) VALUES ('Гитара',4000);
INSERT 0 1

test_db=# select count(*) from orders;
 count
-------
     5
(1 row)
```

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Решение:
```
insert into clients (surname, from_country_id, from_country_name) VALUES ('Иванов Иван Иванович', 1, 'USA');
insert into clients (surname, from_country_id, from_country_name) VALUES ('Петров Петр Петрович', 2, 'Canada');
insert into clients (surname, from_country_id, from_country_name) VALUES ('Иоганн Себастьян Бах', 3, 'Japan');
insert into clients (surname, from_country_id, from_country_name) VALUES ('Ронни Джеймс Дио', 4, 'Russia');
insert into clients (surname, from_country_id, from_country_name) VALUES ('Ritchie Blackmore', 4, 'Russia');

test_db=# select count(*) from clients
test_db-# ;
 count
-------
     5
(1 row)
```

- записи появились в таблице:
```
test_db=# select * from clients
test_db-# ;
 id |       surname        | from_country_id | from_country_name | order_id
----+----------------------+-----------------+-------------------+----------
 11 | Иванов Иван Иванович |               1 | USA               |
 12 | Петров Петр Петрович |               2 | Canada            |
 13 | Иоганн Себастьян Бах |               3 | Japan             |
 14 | Ронни Джеймс Дио     |               4 | Russia            |
 15 | Ritchie Blackmore    |               4 | Russia            |
(5 rows)
```



## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
 
Подсказк - используйте директиву `UPDATE`.



Решение:
```
test_db=# select * from clients
test_db-# ;
 id |       surname        | from_country_id | from_country_name | order_id
----+----------------------+-----------------+-------------------+----------
 11 | Иванов Иван Иванович |               1 | USA               |
 12 | Петров Петр Петрович |               2 | Canada            |
 13 | Иоганн Себастьян Бах |               3 | Japan             |
 14 | Ронни Джеймс Дио     |               4 | Russia            |
 15 | Ritchie Blackmore    |               4 | Russia            |
(5 rows)

test_db=# UPDATE clients set order_id = orders.id from orders where orders.title like 'Книга' and clients.surname like 'Иванов Иван Иванович';
UPDATE 1
test_db=# UPDATE clients set order_id = orders.id from orders where orders.title like 'Монитор' and clients.surname like 'Петров Петр Петрович';
UPDATE 1
test_db=# UPDATE clients set order_id = orders.id from orders where orders.title like 'Гитара' and clients.surname like 'Иоганн Себастьян Бах';
UPDATE 1
test_db=#


test_db=# select * from clients;
 id |       surname        | from_country_id | from_country_name | order_id
----+----------------------+-----------------+-------------------+----------
 14 | Ронни Джеймс Дио     |               4 | Russia            |
 15 | Ritchie Blackmore    |               4 | Russia            |
 11 | Иванов Иван Иванович |               1 | USA               |        3
 12 | Петров Петр Петрович |               2 | Canada            |        4
 13 | Иоганн Себастьян Бах |               3 | Japan             |        5
(5 rows)

```

## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.

Решение:
```
test_db=# select clients, order_id, orders.id from clients, orders where orders.title like 'Книга' and clients.surname like 'Иванов Иван Иванович'
test_db-# ;
               clients               | order_id | id
-------------------------------------+----------+----
 (11,"Иванов Иван Иванович",1,USA,3) |        3 |  3
(1 row)

test_db=# explain select clients, order_id, orders.id from clients, orders where orders.title like 'Книга' and clients.surname like 'Иванов Иван Иванович'
;
                              QUERY PLAN
----------------------------------------------------------------------
 Nested Loop  (cost=0.00..45.06 rows=24 width=108)
   ->  Seq Scan on orders  (cost=0.00..25.00 rows=6 width=4)
         Filter: (title ~~ 'Книга'::text)
   ->  Materialize  (cost=0.00..19.77 rows=4 width=104)
         ->  Seq Scan on clients  (cost=0.00..19.75 rows=4 width=104)
               Filter: (surname ~~ 'Иванов Иван Иванович'::text)
(6 rows)
```
Пояснение:

План представлен в виде дерева. Нижне узлы собственно и извлекают результатирующие строки. Каждые узел имеет следующие параметры:

стоимость (первая цифра - стоимость для начала вывода результат, вторая итоговая стоимость, единица изменения - условные очки),
примерное кол-во извлеченных строк 
ширина вывода в байтах.

Самый верхний узел содержит итоговую оценку, которая содержит все остальные оценки.


## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

---

Решение:

- Применим команды pg_dump и pg_restore, т.к. у нас одна база данных, этого будет достаточно.
```
root@c1a5670b3700:/var# chown postgres:postgres db-backup

root@c1a5670b3700:/var# ls -al
total 64
drwxr-xr-x 1 root     root     4096 Aug 29 11:24 .
drwxr-xr-x 1 root     root     4096 Aug 29 11:24 ..
drwxr-xr-x 2 root     root     4096 Jun 30 21:35 backups
drwxr-xr-x 1 root     root     4096 Aug 23 12:55 cache
drwxrwxr-x 2 postgres postgres 4096 Aug 30 01:23 db-backup
drwx------ 3 postgres postgres 4096 Aug 29 11:41 db-data
```
- сделаем backup базы в виде плоского текстого файла. 
```
root@c1a5670b3700:/var# cd db-backup/
root@c1a5670b3700:/var/db-backup# su - postgres

postgres@c1a5670b3700:/var/db-backup$ pg_dump  -Fc test_db > test_db.dump
postgres@c1a5670b3700:/var/db-backup$ ls -al
total 20
drwxrwxr-x 2 postgres postgres 4096 Aug 30 01:32 .
drwxr-xr-x 1 root     root     4096 Aug 29 11:24 ..
-rw-r--r-- 1 postgres postgres 4274 Aug 30 01:32 test_db.dump
```

-дамп базы появился в папке, его видно в т.ч. с хоста с docker-compose
```
vagrant@vagrant:~$ cd /home/vagrant/postgresql/backup
vagrant@vagrant:~/postgresql/backup$ ls -al
total 16
drwxrwxr-x 2 systemd-coredump systemd-coredump 4096 Aug 30 01:32 .
drwxrwxr-x 4 vagrant          vagrant          4096 Aug 29 09:26 ..
-rw-r--r-- 1 systemd-coredump systemd-coredump 4274 Aug 30 01:32 test_db.dump
```
-- остановим первый контейнер
```
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose ps
NAME                      COMMAND                  SERVICE             STATUS              PORTS
docker-compose-yam-db-1   "docker-entrypoint.s…"   db                  running             5432/tcp
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose stop
[+] Running 1/1
 ⠿ Container docker-compose-yam-db-1  Stopped                                                                                                                                                               2.3s
vagrant@vagrant:~/docker-compose-yam$ sudo docker compose ps
NAME                      COMMAND                  SERVICE             STATUS              PORTS
docker-compose-yam-db-1   "docker-entrypoint.s…"   db                  exited (0)
```

-- поднимем новый контейнер:
```
vagrant@vagrant:~/docker-compose-yam2$ sudo docker compose build
vagrant@vagrant:~/docker-compose-yam2$ sudo docker compose up
[+] Running 2/2
 ⠿ Network docker-compose-yam2_default  Created                                                                                                                                                             1.1s
 ⠿ Container docker-compose-yam2-db2-1  Created                                                                                                                                                             0.5s
Attaching to docker-compose-yam2-db2-1
docker-compose-yam2-db2-1  | The files belonging to this database system will be owned by user "postgres".
docker-compose-yam2-db2-1  | This user must also own the server process.
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | The database cluster will be initialized with locale "en_US.utf8".
docker-compose-yam2-db2-1  | The default database encoding has accordingly been set to "UTF8".
docker-compose-yam2-db2-1  | The default text search configuration will be set to "english".
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | Data page checksums are disabled.
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | fixing permissions on existing directory /var/lib/postgresql/data ... ok
docker-compose-yam2-db2-1  | creating subdirectories ... ok
docker-compose-yam2-db2-1  | selecting dynamic shared memory implementation ... posix
docker-compose-yam2-db2-1  | selecting default max_connections ... 100
docker-compose-yam2-db2-1  | selecting default shared_buffers ... 128MB
docker-compose-yam2-db2-1  | selecting default time zone ... Etc/UTC
docker-compose-yam2-db2-1  | creating configuration files ... ok
docker-compose-yam2-db2-1  | running bootstrap script ... ok
docker-compose-yam2-db2-1  | performing post-bootstrap initialization ... ok
docker-compose-yam2-db2-1  | syncing data to disk ... initdb: warning: enabling "trust" authentication for local connections
docker-compose-yam2-db2-1  | You can change this by editing pg_hba.conf or using the option -A, or
docker-compose-yam2-db2-1  | --auth-local and --auth-host, the next time you run initdb.
docker-compose-yam2-db2-1  | ok
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | Success. You can now start the database server using:
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  |     pg_ctl -D /var/lib/postgresql/data -l logfile start
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | waiting for server to start....2022-08-30 01:53:09.067 UTC [48] LOG:  starting PostgreSQL 12.12 (Debian 12.12-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.151 UTC [48] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.342 UTC [49] LOG:  database system was shut down at 2022-08-30 01:53:06 UTC
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.370 UTC [48] LOG:  database system is ready to accept connections
docker-compose-yam2-db2-1  |  done
docker-compose-yam2-db2-1  | server started
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.581 UTC [48] LOG:  received fast shutdown request
docker-compose-yam2-db2-1  | waiting for server to shut down....2022-08-30 01:53:09.665 UTC [48] LOG:  aborting any active transactions
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.668 UTC [48] LOG:  background worker "logical replication launcher" (PID 55) exited with exit code 1
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.673 UTC [50] LOG:  shutting down
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.754 UTC [48] LOG:  database system is shut down
docker-compose-yam2-db2-1  |  done
docker-compose-yam2-db2-1  | server stopped
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | PostgreSQL init process complete; ready for start up.
docker-compose-yam2-db2-1  |
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.876 UTC [1] LOG:  starting PostgreSQL 12.12 (Debian 12.12-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.877 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.877 UTC [1] LOG:  listening on IPv6 address "::", port 5432
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.911 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.948 UTC [67] LOG:  database system was shut down at 2022-08-30 01:53:09 UTC
docker-compose-yam2-db2-1  | 2022-08-30 01:53:09.964 UTC [1] LOG:  database system is ready to accept connections

vagrant@vagrant:~/docker-compose-yam2$ sudo docker compose ps
NAME                        COMMAND                  SERVICE             STATUS              PORTS
docker-compose-yam2-db2-1   "docker-entrypoint.s…"   db2                 running             5432/tcp
```

- восстановим базу, делать это будем через утилиту psql
```
vagrant@vagrant:~/docker-compose-yam2$ sudo docker exec -it docker-compose-yam2-db2-1 bash
```
- база не содежит данных
```
root@152a0fa4158f:/# cd /var
root@152a0fa4158f:/var# ls -al
total 52
drwxr-xr-x 1 root     root     4096 Aug 30 01:53 .
drwxr-xr-x 1 root     root     4096 Aug 30 01:53 ..
drwxr-xr-x 2 root     root     4096 Jun 30 21:35 backups
drwxr-xr-x 1 root     root     4096 Aug 23 12:55 cache
drwxrwxr-x 2 postgres postgres 4096 Aug 30 01:32 db-backup
drwx------ 3 postgres postgres 4096 Aug 29 11:41 db-data
drwxr-xr-x 1 root     root     4096 Aug 23 12:55 lib
drwxrwsr-x 2 root     staff    4096 Jun 30 21:35 local
lrwxrwxrwx 1 root     root        9 Aug 22 00:00 lock -> /run/lock
drwxr-xr-x 1 root     root     4096 Aug 23 12:55 log
drwxrwsr-x 2 root     mail     4096 Aug 22 00:00 mail
drwxr-xr-x 2 root     root     4096 Aug 22 00:00 opt
lrwxrwxrwx 1 root     root        4 Aug 22 00:00 run -> /run
drwxr-xr-x 2 root     root     4096 Aug 22 00:00 spool
drwxrwxrwt 2 root     root     4096 Jun 30 21:35 tmp
root@152a0fa4158f:/var# cd db-backup
root@152a0fa4158f:/var/db-backup# ls -al
total 16
drwxrwxr-x 2 postgres postgres 4096 Aug 30 01:32 .
drwxr-xr-x 1 root     root     4096 Aug 30 01:53 ..
-rw-r--r-- 1 postgres postgres 4274 Aug 30 01:32 test_db.dump
root@152a0fa4158f:/var/db-backup# su - postgres
postgres@152a0fa4158f:~$ psql
psql (12.12 (Debian 12.12-1.pgdg110+1))
Type "help" for help.

postgres=# \list
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)
```
 - загрузим  дамп 
```
postgres=# CREATE DATABASE "test_db";
CREATE DATABASE
postgres=# quit
postgres@152a0fa4158f:/var/db-backup$ pg_restore -d test_db test_db.dump
postgres@152a0fa4158f:/var/db-backup$ psql
psql (12.12 (Debian 12.12-1.pgdg110+1))
Type "help" for help.

postgres=# \connect test_db
You are now connected to database "test_db" as user "postgres".
test_db=# select * from clients
test_db-# ;
 id |                surname                 | from_country_id | from_country_name | order_id
----+----------------------------------------+-----------------+-------------------+----------
 14 | Ронни Джеймс Дио         |               4 | Russia            |
 15 | Ritchie Blackmore                      |               4 | Russia            |
 11 | Иванов Иван Иванович |               1 | USA               |        3
 12 | Петров Петр Петрович |               2 | Canada            |        4
 13 | Иоганн Себастьян Бах |               3 | Japan             |        5
(5 rows)

test_db=# select * from orders;
 id |     title      | price
----+----------------+-------
  1 | Шоколад |    10
  2 | Принтер |  3000
  3 | Книга     |   500
  4 | Монитор |  7000
  5 | Гитара   |  4000
(5 rows)
```

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
