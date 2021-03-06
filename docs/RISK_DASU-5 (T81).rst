﻿================================================================================================================
RISK_DASU-5 (T81) Наявність пов'язаних Учасників у конкурентних процедурах Замовника (взаємна участь більше 70%)
================================================================================================================

***************
Суть індикатора
***************



********************************
Підстава для розробки індикатора
********************************



*********************************
Методологія та алгоритм розрахунку індикатора
*********************************

Рівень розрахунку
=================
Індикатор розраховується для відкритих процедур на рівні *лоту*.

Джерела даних для розрахунку
============================

Для розрахунку індикатора використовуються наступні джерела даних:

- API модуля тендеринга електронної системи закупівель
- Таблиця зі списком участників (постачальників) які заключали доовори тільки з одним замовником (на дату розрахунку укладено бульше трьох договорів з замовником). Відсутня практика успішних процедур з іншими замовниками. :ref:`tb_SuppliersSingleBuyer`

Типи процедур
=============

Індикатор розраховується для наступних типів процедур:

 - ``aboveThresholdUA`` - Відкриті торги
 - ``aboveThresholdEU`` - Відкриті торги з публікацією англійською мовою

Типи замовників
===============

Індикатор розраховується для замовників які в системі визначені як "А — Замовник (загальний)"  -  (``general``) та "В — Замовник (спеціальний)"  -  (``special``).

Стадії процедур
===============

Подія, що вмикає розрахунок індикатора
--------------------------------------

Подія, що вмикає розрахунок індикатора - Замовник публікує рішення про намір укласти договір та одразу вносить інформацію про учасників та визначає Переможця переговорів. У електронній системі закупівель цій подіі відповідає поява об'єкту ``data.contracts`` зі статусом ``data.contracts.status = 'pending'``

Подія, що вимикає розрахунок індикатора
---------------------------------------

Розрахунок індикатора вимикається одразу після того, як замовник переводить договір в статус «підписаний» (``active``), після чого окремою дією Замовник переводить Тендер в статус ``complete``. 

Статуси процедур
----------------

Виходячи з подій, що вмикають та вимикають розрахунок індикатора, маємо наступні умови розрахунку:

- Індикатор розраховується, якщо в json-документі, що відповідає процедурі, присутній блок ``data.contracts``, де хоча б в одного об'єкту виконується ``data.contracts.status = 'pending'``

- Індикатор розраховується для даної процедури тоді і тільки тоді, коли він ще не був ніколи порахований для цієї процедури.

Частота розрахунку
==================

Індикатор розраховується при публікації замовником наміру про укладення договору з постачальником в разі якщо намір удласти договір було скасовано та визначено іншого кандидата для укладання договору - індикаторо повинен бути розрахований знову.

Поля для розрахунку
===================

Для розрахунку індикатора використовуються наступні поля з API модуля тендеринга:

- ``data:awards:status``
- ``data:awards:suppliers:identifier:scheme``
- ``data:awards:suppliers:identifier:id``

Для розрахунку індикатора використовуються наступні транзакційні змінні:



Для розрахунку індикатора використовуються наступні аналітичні таблиці:

- :ref:`tb_SuppliersSingleBuyer``

Формула розрахунку
==================

Індикатор розраховується наступним чином:



Фактори, що впливають на неточність розрахунку
==============================================
