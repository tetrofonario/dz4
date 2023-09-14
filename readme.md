***Домашнее задание*** <br>
Работа с базами данных, пользователями и правами <br>

Цель:
создание новой базы данных, схемы и таблицы
создание роли для чтения данных из созданной схемы созданной базы данных
создание роли для чтения и записи из созданной схемы созданной базы данных<br>

Описание/Пошаговая инструкция выполнения домашнего задания:

1 создайте новый кластер PostgresSQL 14 <br>
```
Ver Cluster Port Status Owner    Data directory              Log file
14  main    5432 online postgres /var/lib/postgresql/14/main /var/log/postgresql/postgresql-14-main.log

```
2 зайдите в созданный кластер под пользователем postgres <br>
```agsl
sergt@ubuntu1:~$ sudo -u postgres psql
could not change directory to "/home/sergt": Permission denied
psql (14.9 (Ubuntu 14.9-1.pgdg22.04+1))
Type "help" for help.
postgres=#

```
3 создайте новую базу данных testdb <br>
```
postgres=# CREATE DATABASE testdb;
CREATE DATABASE
```
4 зайдите в созданную базу данных под пользователем postgres <br>
```
postgres-# \c testdb;
You are now connected to database "testdb" as user "postgres".
testdb-#
```
5 создайте новую схему testnm <br>
```
testdb=# CREATE SCHEMA testnm;-+-0
CREATE SCHEMA
```
6 создайте новую таблицу t1 с одной колонкой c1 типа integer <br>
```
testdb=# CREATE TABLE t1(c1 integer);
CREATE TABLE
```
7 вставьте строку со значением c1=1 <br>
```
testdb=# INSERT INTO t1 values(1);
INSERT 0 1
```
8 создайте новую роль readonly <br>
```
testdb=# CREATE role readonly;
CREATE ROLE
```
9 дайте новой роли право на подключение к базе данных testdb <br>
```
testdb=# grant connect on DATABASE testdb TO readonly;
GRANT
```
10 дайте новой роли право на использование схемы testnm <br>
```
testdb=# grant usage on SCHEMA testnm to readonly;
GRANT
```
11 дайте новой роли право на select для всех таблиц схемы testnm <br>
```
testdb=# grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
GRANT
```
12 создайте пользователя testread с паролем test123 <br>
```
testdb=# CREATE USER testread with password 'test123';
CREATE ROLE
```
13 дайте роль readonly пользователю testread <br>
```
postgres=# grant readonly TO testread;
GRANT ROLE
```
14 зайдите под пользователем testread в базу данных testdb <br>
```
postgres=# \c testdb testread;
connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  Peer authentication failed for user "testread"
Previous connection kept
```
***-- Войти не получилось <br>***
вторая попытка по подсказке 
```
postgres=# \q
sergt@ubuntu1:~$ psql -h 127.0.0.1 -U testread -d testdb -W
Password:
psql (14.9 (Ubuntu 14.9-1.pgdg22.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.
testdb=>
```

15 сделайте select * from t1; <br>
```
testdb=> select * from t1;
ERROR:  permission denied for table t1
```
16 получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже) <br>

***-- Нет не получилось.*** <br>
17 напишите что именно произошло в тексте домашнего задания<br>
***В схеме testnm нет такой таблицы, она в public а к схеме public у роли  readonly нет доступа.*** <br>
18 у вас есть идеи почему? ведь права то дали?<br>
***Права дали на другой объект .*** <br>
19 посмотрите на список таблиц<br>
```
testdb=> \dt
        List of relations
 Schema | Name | Type  |  Owner
--------+------+-------+----------
 public | t1   | table | postgres
(1 row)
```
20 подсказка в шпаргалке под пунктом 20<br>
```

```
21 а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)<br>
```

```
22 вернитесь в базу данных testdb под пользователем postgres<br>
```
sergt@ubuntu1:~$ sudo -u postgres psql
could not change directory to "/home/sergt": Permission denied
psql (14.9 (Ubuntu 14.9-1.pgdg22.04+1))
Type "help" for help.
postgres=#
```
23 удалите таблицу t1<br>
```

postgres=# \c testdb;
You are now connected to database "testdb" as user "postgres".
postgres=# \c testdb;
testdb=# DROP TABLE t1;
DROP TABLE
testdb=#
```
24 создайте ее заново но уже с явным указанием имени схемы testnm<br>
```
testdb=# CREATE TABLE testnm.t1(c1 integer);
CREATE TABLE
```
25 вставьте строку со значением c1=1<br>
```
testdb=# insert into testnm.t1 values(1);
INSERT 0 1
```
26 зайдите под пользователем testread в базу данных testdb<br>
```
sergt@ubuntu1:~$ psql -h 127.0.0.1 -U testread -d testdb -W
Password:
psql (14.9 (Ubuntu 14.9-1.pgdg22.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.
testdb=>
```
27 сделайте select * from testnm.t1;<br>
```
testdb=> select * from testnm.t1;
ERROR:  permission denied for table t1
```
28 получилось?<br>
***Нет т.к. права назначались до появления таблицы.***<br> 
29 есть идеи почему? если нет - смотрите шпаргалку<br>
```

```
30 как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку<br>
```
postgres=# \c testdb postgres;
You are now connected to database "testdb" as user "postgres".
testdb=# ALTER default privileges in SCHEMA testnm grant SELECT on TABLES to readonly;
ALTER DEFAULT PRIVILEGES
testdb=#
grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
```
31 сделайте select * from testnm.t1;<br>
```

sergt@ubuntu1:~$ psql -h 127.0.0.1 -U testread -d testdb -W
Password:
psql (14.9 (Ubuntu 14.9-1.pgdg22.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

testdb=> select * from testnm.t1;
 c1
----
  1
(1 row)

testdb=>

```
32 получилось?<br6>
***Да*** <br>
33 есть идеи почему? если нет - смотрите шпаргалку<br>
***Выдал права роли readonly на селект всем таблицам в схеме testnm и уже существующей таблице.***<br>
31 сделайте select * from testnm.t1;<br>
```
testdb=> select * from testnm.t1;
 c1
----
  1
(1 row)

```
32 получилось?<br>
***да***<br>
33 ура!<br>
34 теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);<br>
```
testdb=> create table t2(c1 integer); insert into t2 values (2);
CREATE TABLE
INSERT 0 1
testdb=>

```
35 а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?<br>
```

```
36 есть идеи как убрать эти права? если нет - смотрите шпаргалку<br>
```
postgres=# \c testdb postgres;
You are now connected to database "testdb" as user "postgres".
testdb=# REVOKE CREATE on SCHEMA public FROM public;
REVOKE ALL on DATABASE testdb FROM public;
REVOKE
REVOKE
```
37 если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды<br>
***По умолчанию .***<br>
38 теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);<br>
```
testdb=> create table t3(c1 integer); insert into t2 values (2);
ERROR:  permission denied for schema public
LINE 1: create table t3(c1 integer);
                    ^
INSERT 0 1
```
39 расскажите что получилось и почему<br>
```
Таблицу создать не получилось т.к. изменили привелегии по умолчанию схемы public
```
