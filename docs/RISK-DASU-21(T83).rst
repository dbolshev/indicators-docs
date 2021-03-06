﻿================================================================================================================
RISK-DASU-21(T83). Нетипово велика або мала сума закупівлі для конкретного предмету закупівлі Замовника (CPV(4))
================================================================================================================

***************
Суть індикатора
***************

Даний індикатор виявляє ситуації, де Замовник проводить процедури на суму, що є нетиповою для його постійних закупок по групі товарів CPV4.

********************************
Підстава для розробки індикатора
********************************

Цей індикатор було розроблено, щоб виявляти можливе неякісне планування закупівель Замовником.

*********************************
Методологія розрахунку індикатора
*********************************

Етап існування процедури
========================
Індикатор розраховується, коли процедура знаходиться на етапі *тендерингу*.

Рівень розрахунку
=================
Індикатор розраховується на рівні *тендера*.

Джерела даних для розрахунку
============================

Для розрахунку індикатора вікористовуються наступні джерела даних:

- API модуля тендеринга електронної системи закупівель.

- Аналітична таблиця tbl_meanStdOfBuyerByCPV4.


Типи процедур
=============

Індикатор розраховується для наступних типів процедур:

- ``reporting`` - *процедура звітування*

- ``belowThreshold`` - *допорогові закупівлі*

- ``aboveThresholdUA`` - *відкриті торги*

- ``aboveThresholdEU`` - *відкриті торги з публікацією англійською мовою*

- ``negotiation`` - *переговорна процедура*

- ``negotiation.quick`` - *прискорена переговорна процедура*

Типи замовників
===============

Індикатор розраховується для загальних замовників (``general``) та замовників, що здійснюють діяльність в окремих сферах господарювання (``special``).


Категорії товарів
=================

Індикатор розраховується для процедур закупівлі *товарів*, *послуг* та *робіт* відподівно до транзакційної змінної :ref:`tv_subjectOfProcurement` та :ref:`порядку визначення категоріі закупівлі за кодом Єдиного закупівельного словника<gsw_formula>`.

Стадії процедур
===============

Подія, що вмикає розрахунок індикатора
--------------------------------------
Подія, що вмикає розрахунок індикатора - публікація замовником у електронній системі оголошення про проведення закупівлі.

Подія, що вимикає розрахунок індикатора
---------------------------------------
Розрахунок індикатора вимикається тоді, коли закінчується період подачі пропозицій.


Статуси процедур
----------------

Виходячи з подій, що вмикають та вимикають розрахунок індикатора, маємо наступні умови розрахунку:

- Індикатор розраховується для наступних статусів процедур:

  - ``active.tendering``
  - ``active.enquiries``

Частота розрахунку
==================

Індикатор розраховується при будь-якій зміні json-документа, що відповідає процедурі, якщо присутні всі умови для його розрахунку.

Окрім цього індикатор перераховується раз на добу незалежно від змін у json-документі, що відповідає процедурі, якщо присутні всі умови для його розрахунку.

Поля для розрахунку
===================

Для розрахунку індикатора використовуються наступні поля з API модуля тендеринга:

- ``data.value.amount``
- ``data.procuringEntity.identifier.scheme``
- ``data.procuringEntity.identifier.id``
- ``data.items.classification.id``

Формула розрахунку
==================

1. Визначаємо замовника процедури - конкатенація ``data.procuringEntity.identifier.scheme`` та ``data.procuringEntity.identifier.id``.
   Визначаємо очікувану вартість процедури ``data.value.amount``.

2. Знаходимо ``cpv`` процедури. Якщо процедура однолотова, то її ``cpv = data.items.classification.id``.
   Якщо процедура багатолотова, то робимо наступним чином. Для кожного об'єкту ``data.items`` знаходимо ``data.items.classification.id``. Потім беремо найдовшу послідовність цифр (зліва направо), що співпадає в усіх об'єктах. Доповнюємо цю комбінацію цифр праворуч "0" так, щоб вийшло 8 цифр. Це буде ``cpv`` багатолотового тендера.

3. Знаходимо ``cpv4`` процедури. Беремо перші 4 цифри ``cpv`` та додаємо зправа "0000".

4. За ``cpv4`` та замовником шукаємо рядок в таблиці tbl_meanStdOfBuyerByCPV4. Якщо рядок є, беремо з нього середнє значення (mean)  та стандартне відхилення (std).

5. Рахуємо наступну формулу  ``abs(data.value.amount - mean) - 3 * std``. Якщо значення буде більше 0, то індикатор приймає значення ``1``.

Фактори, що впливають на неточність розрахунку
==============================================

Індикатор може бути порахований неточно у випадках, коли організації, що не є замовниками, помилково визначають себе в системі як замовники.

