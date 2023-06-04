---
title: 'лекция 3-2: Анатомия PostgreSQL'
updated: 2023-04-08 11:33:47Z
created: 2023-04-07 12:30:35Z
latitude: 55.98715260
longitude: 37.20215140
altitude: 0.0000
tags:
  - go
  - базы данных
---

# План занятия

1.  Логическкое и физическое устройство БД
2.  Обработка запросов
3.  Методы доступа и соединения таблиц
4.  Процессы БД
5.  Костыли

# 1\. Устройство БД

## Логическое устройство

<img src="../_resources/e08bbdae3ac11f7d120b00eb73f1e2f2.png" alt="e08bbdae3ac11f7d120b00eb73f1e2f2.png" width="578" height="180" class="jop-noMdConv">

## Физическое устройство

<img src="../_resources/7e8862ac4ced11b9e05a85b7271c6475.png" alt="7e8862ac4ced11b9e05a85b7271c6475.png" width="576" height="260" class="jop-noMdConv">

## Табличные пространства

<img src="../_resources/c15fdc28ed2667ae6450aac628927a33.png" alt="c15fdc28ed2667ae6450aac628927a33.png" width="577" height="288" class="jop-noMdConv">

## Устройство heap блока

<img src="../_resources/648599516d5191d63319fac7a593a666.png" alt="648599516d5191d63319fac7a593a666.png" width="571" height="199" class="jop-noMdConv">

## Жизненный цикл запроса

<img src="../_resources/f7ebfed16415f6386598ece9469b5712.png" alt="f7ebfed16415f6386598ece9469b5712.png" width="403" height="467" class="jop-noMdConv">

### Parser

<img src="../_resources/90e2f7f10e27a6633a2de4b88ba86574.png" alt="90e2f7f10e27a6633a2de4b88ba86574.png" width="299" height="186" class="jop-noMdConv"> <img src="../_resources/c3f12de4495911c1ee00d376bd3d1bad.png" alt="c3f12de4495911c1ee00d376bd3d1bad.png" width="501" height="302" class="jop-noMdConv">

### Analyzer

<img src="../_resources/a927f628799a224d25f99f26d666d385.png" alt="a927f628799a224d25f99f26d666d385.png" width="473" height="208" class="jop-noMdConv">

### Rewriter

<img src="../_resources/69f4b61f825edfa70f4e1ea93c2dd40a.png" alt="69f4b61f825edfa70f4e1ea93c2dd40a.png" width="599" height="216" class="jop-noMdConv">

### Planner

<img src="../_resources/3ef358002022529e07d72c9bf4effd8b.png" alt="3ef358002022529e07d72c9bf4effd8b.png" width="605" height="172" class="jop-noMdConv">Стоимости:

- `seq_page_cost - 1`
- `random_page_cost - 4`
- `cpu_tuple_cost - 0.01`
- `cpu_index_tuple_cost - 0.005`
- `cpu_operator_cost - 0.005`

### Executor

<img src="../_resources/1628878e7c26c812e6a8478f43bfc2c7.png" alt="1628878e7c26c812e6a8478f43bfc2c7.png" width="605" height="451" class="jop-noMdConv">

# 3\. Методы доступа и соединения таблиц

## Методы доступа

- Sequental scan
- Index scan
- Index only scan
- Bitmap scan

## Соединения таблиц

### Nested Loop

<img src="../_resources/12d4646bfd8fa050e8f458ad9d08e27c.png" alt="12d4646bfd8fa050e8f458ad9d08e27c.png" width="536" height="176" class="jop-noMdConv">

- обычный вложенный цикл
- работает хорошо на небольших наборах данных, особенно для первой таблицы
- не требует подготовки
- поддерживает операции &lt; <= = &gt;= >

### Hash Join

<img src="../_resources/74246e79fbe875ba1033976bdb61c0d1.png" alt="74246e79fbe875ba1033976bdb61c0d1.png" width="539" height="220" class="jop-noMdConv">

- вычисляется хеш, создается массив хешей от выборок и джойнятся строчки из одной корзинф
- требует подготовки
- поддерживает только операцию =

### Merge Join

<img src="../_resources/c3c9f29637ad0a3b913d8b8f644e1f6e.png" alt="c3c9f29637ad0a3b913d8b8f644e1f6e.png" width="549" height="224" class="jop-noMdConv">

- работает на отсортированных данных
- требует подготовки
- работает быстро
- поддерживает только операцию =

## Тестовые данные

<img src="../_resources/69042b4c1bf8dc5cb3839b42a8e5288f.png" alt="69042b4c1bf8dc5cb3839b42a8e5288f.png" width="615" height="146" class="jop-noMdConv">

### Nested Loop

![2a7515291253e7ba159776f905365314.png](2a7515291253e7ba159776f905365314.png)

### Hash Join

![348b92c0488f2d36b15e1e6e7c082a02.png](348b92c0488f2d36b15e1e6e7c082a02.png)

### Merge join

![1f5ed7674e501f6a494b2f843581b3f2.png](1f5ed7674e501f6a494b2f843581b3f2.png)

# 4\. Процессы СУБД

## Процессы

![2d11cdb3a72c825021fa77689ca71f09.png](2d11cdb3a72c825021fa77689ca71f09.png)

## Память

![fc442a2ab0f8d3411172998862cc287d.png](fc442a2ab0f8d3411172998862cc287d.png)

### Shared Buffers

<img src="../_resources/2c078b29b907ae0eeba1923c673be8a3.png" alt="2c078b29b907ae0eeba1923c673be8a3.png" width="432" height="208" class="jop-noMdConv">

# 5\. Костыли

## Pooler

> мультиплексирует соединения клиентов к меньшему количество соединений к PostgreSQL

![17808e04c9252cf28c15c0290f883b00.png](17808e04c9252cf28c15c0290f883b00.png)

### PgBouncer

![823a1e00ed792c715d6c5a93e01d4cd7.png](823a1e00ed792c715d6c5a93e01d4cd7.png)

# Заключение

- Посмотрели внутреннее устройство PG
- Рассмотрели жизненный цикл запроса
- Поговорили о планах запроса и как их читать
- Разобрали процессы PostgreSQL
- Поговорили о недочетах архитектуры Postgres и как их обходить