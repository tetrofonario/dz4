***Домашнее задание*** <br>
Работа с базами данных, пользователями и правами <br>

Цель:
создание новой базы данных, схемы и таблицы
создание роли для чтения данных из созданной схемы созданной базы данных
создание роли для чтения и записи из созданной схемы созданной базы данных<br>

Описание/Пошаговая инструкция выполнения домашнего задания:

1 создайте новый кластер PostgresSQL 14 <br>
```

```
2 зайдите в созданный кластер под пользователем postgres <br>
3 создайте новую базу данных testdb <br>
4 зайдите в созданную базу данных под пользователем postgres <br>
5 создайте новую схему testnm <br>
6 создайте новую таблицу t1 с одной колонкой c1 типа integer <br>
7 вставьте строку со значением c1=1 <br>
8 создайте новую роль readonly <br>
9 дайте новой роли право на подключение к базе данных testdb <br>
10 дайте новой роли право на использование схемы testnm <br>
11 дайте новой роли право на select для всех таблиц схемы testnm <br>
12 создайте пользователя testread с паролем test123 <br>
13 дайте роль readonly пользователю testread <br>
14 зайдите под пользователем testread в базу данных testdb <br>
15 сделайте select * from t1; <br>
16 получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже) <br>
17 напишите что именно произошло в тексте домашнего задания<br>
18 у вас есть идеи почему? ведь права то дали?<br>
19 посмотрите на список таблиц<br>
20 подсказка в шпаргалке под пунктом 20<br>
21 а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)<br>
22 вернитесь в базу данных testdb под пользователем postgres<br>
23 удалите таблицу t1<br>
24 создайте ее заново но уже с явным указанием имени схемы testnm<br>
25 вставьте строку со значением c1=1<br>
26 зайдите под пользователем testread в базу данных testdb<br>
27 сделайте select * from testnm.t1;<br>
28 получилось?<br>
29 есть идеи почему? если нет - смотрите шпаргалку<br>
30 как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку<br>
31 сделайте select * from testnm.t1;<br>
32 получилось?<br>
33 есть идеи почему? если нет - смотрите шпаргалку<br>
31 сделайте select * from testnm.t1;<br>
32 получилось?<br>
33 ура!<br>
34 теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);<br>
35 а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?<br>
36 есть идеи как убрать эти права? если нет - смотрите шпаргалку<br>
37 если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды<br>
38 теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);<br>
39 расскажите что получилось и почему<br>