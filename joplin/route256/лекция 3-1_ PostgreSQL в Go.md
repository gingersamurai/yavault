---
title: 'лекция 3-1: PostgreSQL в Go'
updated: 2023-04-06 07:13:36Z
created: 2023-04-03 10:11:27Z
tags:
  - go
  - базы данных
---

# План занятия

1.  Коротко о PostgreSQL
2.  немного теории СУБД
3.  Та часть PostgreSQL, которая SQL
4.  Индексы и ограничения
5.  Библиотеки для работы с БД

# 1\. Slonik

## PostgreSQL

> Обьектно-реляционная СУБД

### Важные аспекты реляционных БД

- Единый интерфейм \- SQL
- ACID
- целостность данных

### Преимущества PostgreSQL

- Производительность
- Масштабируемость
- Open Source
- Широкий выбор инструментов и расширений

### Клиенты

- psql
- PgAdmin
- VS Code  SQL Tools
- Dbeaver
- DetaGrip

# 2\. Немного теории

## ACID

### Atomacity (атомарность)

> операция либо выполняется полностью, либо не выполняется

### Consistency (согласованность)

> операция не должна ломать согласованность данных (соблюдать ограничения)

### Isolation (изолированность)

> несколько атомарных операций, которые работают параллельно не должны влиять друг на друга

### Durability (надежность)

> если операция проведена, данные сохранены

### Уровни изолированности

<img src="../_resources/b1417d1531a998ca3295dba64ab9c098.png" alt="b1417d1531a998ca3295dba64ab9c098.png" width="516" height="140" class="jop-noMdConv">

### MVCC

По умолчанию уровень изолированности \- RC.

для обеспечения изолированности PostgreSQL реализует *MultiVersion Concurrency Control*

### *1NF*

> *определение*

*<img src="../_resources/c142f990e2593e82fbd821f94c2ee811.png" alt="c142f990e2593e82fbd821f94c2ee811.png" width="509" height="193" class="jop-noMdConv">*
[](../_resources/c142f990e2593e82fbd821f94c2ee811.png)
### *2NF*

*<img src="../_resources/d9d556d40dffce1f9e6deb16b004223f.png" alt="d9d556d40dffce1f9e6deb16b004223f.png" width="447" height="162" class="jop-noMdConv">*

переделываем в:

<img src="../_resources/79e5cdd6b76f6c276cc8c1b1922aedd5.png" alt="79e5cdd6b76f6c276cc8c1b1922aedd5.png" width="305" height="126" class="jop-noMdConv"> <img src="../_resources/0bdc83ef904abd4f5a23cef9cc05c51d.png" alt="0bdc83ef904abd4f5a23cef9cc05c51d.png" width="299" height="118" class="jop-noMdConv">

### 3NF

<img src="../_resources/b9e8667b738ac25559591cfdd375b122.png" alt="b9e8667b738ac25559591cfdd375b122.png" width="414" height="157" class="jop-noMdConv">

переделываем в:

<img src="../_resources/82c1a1efe3007445d7f48ce13b4dc04d.png" alt="82c1a1efe3007445d7f48ce13b4dc04d.png" width="398" height="114" class="jop-noMdConv">

# 3\. Которко об SQL

### DDL

> разметка данных

- CREATE
- ALTER
- DROP

### DML

> манипуляция данными

- SELECT
- INSERT
- UPDATE
- DELETE

### DCL

> контроль доступа

- GRANT
- REVOKE
- DENY

### TCL

> контроль над транзакциями

- BEGIN
- COMMIT
- ROLLBACK
- SAVEPOINT

## PL/PgSQL

<img src="../_resources/207569c211507c119fde70ca15c446dd.png" alt="207569c211507c119fde70ca15c446dd.png" width="458" height="134" class="jop-noMdConv">

# 4\. Индексы и ограничения

## Типы индексов

- B-Tree
- Hash
- GiST
- SP-GiST
- GIN
- BRIN

По умолчанию команда `CREATE TABLE` создает B-Tree индекс

Для того, чтобы задать тип индекса, явно используется конструкция:

`CREATE INDEX name ON table USING type (column)`

### Hash

<img src="../_resources/27ec03d64cf565c278bbd1dc8f455968.png" alt="27ec03d64cf565c278bbd1dc8f455968.png" width="503" height="112" class="jop-noMdConv">

- хеш функция возвращает 32битное число - ключ в табличке
- \*корзины \*\- ячейки хеш таблицы
- в корзинах хранятся хеши значений и строки, от которых брался хеш
- подходит для поиска по полному совпадению
- поддерживает оператор *=*

### B-Tree

<img src="../_resources/bdef94e330075f9cb6161c34c8e27796.png" alt="bdef94e330075f9cb6161c34c8e27796.png" width="496" height="152" class="jop-noMdConv">

- подходит для поиска значений в диапазоне и по полному совпадению
- может возвращать отсортированные данные
- можно создавать по нескольким столбцам
- может быть покрывающим
- поддерживает &lt; <=  = \&gt; >=
- сложность поиска *O(log n)*

### Gist

пример 1:

![c84f0f5275ee1f6a1fe2d5c7efe37400.png](c84f0f5275ee1f6a1fe2d5c7efe37400.png)

<img src="../_resources/f4a994d2e0fb75ab5e1810f40ce7f3ac.png" alt="f4a994d2e0fb75ab5e1810f40ce7f3ac.png" width="410" height="224" class="jop-noMdConv">

пример 2:

![9023e2f0e471e432a944a26acb6463d5.png](9023e2f0e471e432a944a26acb6463d5.png)

- обьединяет несколько методов индексации
- подходит для поиска по сложным типам данных
- можно создавать по нескольким столбцам
- может быть покрывающим

### SP-GiST

![2e47bfde4637f1eff0c2e3241bb9078f.png](2e47bfde4637f1eff0c2e3241bb9078f.png)

![e0e8399aa755b07057fa9ae1a25512d9.png](e0e8399aa755b07057fa9ae1a25512d9.png)

- обьединяет несколько методов индексации
- подходит для поиска несбалансированных данных
- может быть покрывающим

### GIN

![10de6deb5728c962f9b2d944a97b4b4d.png](10de6deb5728c962f9b2d944a97b4b4d.png)

- подходит для поиска по составным типам данных
- поддерживает fastupdate
- можно создавать по нескольким столбцам

### BRIN

- обьединяет несколько алгоритмов индексации
- подхожит для поиска в больших таблицах, где значение коррелирует с физическим положением данных в таблице
- неточный индекс, но занимает меньше места в памяти, чем другие индексы
- можно создавать по нескольким столбцам

# 5\. Библиотеки для работы с БД

1:06:30