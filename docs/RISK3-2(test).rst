﻿#####################################################################################
RISK3-2(тест). Замовником протягом терміну дії договору змінює термін виконання договору.
#####################################################################################

***************
Суть індикатора
***************

Даний індикатор виявляє ситуації, коли замовник терміну дії договору змінює термін виконання договору. 

************************************
Законодавче обґрунтування індикатора
************************************


********************************
Підстава для розробки індикатора
********************************

.

*********************************
Методологія розрахунку індикатора
*********************************

Етап існування процедури
========================
Індикатор розраховується, коли процедура знаходиться на етапі *контрактингу*.

Рівень розрахунку
=================
Індикатор розраховується на рівні *контракту*.

Джерела даних для розрахунку
============================

Для розрахунку індикатора вікористовуються наступні джерела даних:

- API модуля контрактинга електронної системи закупівель

Типи процедур
=============

Індикатор розраховується для наступних типів процедур:

- ``aboveThresholdUA`` - *відкриті торги*
- ``aboveThresholdEU`` - *відкриті торги з публікацією англійською мовою*

Типи замовників
===============

Індикатор розраховується для замовників типу (``general``)та (``special``).

Стадії процедур
===============

Подія, що вмикає розрахунок індикатора
--------------------------------------

Подія, що вмикає розрахунок індикатора - перехід процедури у статус ``complete``.

Подія, що вимикає розрахунок індикатора
---------------------------------------

Розрахунок індикатора вимикається, якщо замовник публікує повідомлення про виконання контракту.

Статуси процедур
----------------

Виходячи з подій, що вмикають та вимикають розрахунок індикатора, маємо наступні умови розрахунку:

- Індикатор розраховується на наступні статуси процедур:
  
  - ``complete``

Частота розрахунку
==================

Індикатор розраховується при будь-якій зміні json-документа, що відповідає контракту процедури, якщо присутні всі умови для його розрахунку.

Окрім цього індикатор перераховується раз на добу незалежно від змін у json-документі, що відповідає контракту процедури, якщо присутні всі умови для його розрахунку.


Поля для розрахунку
===================

Для розрахунку індикатора використовуються наступні поля з API модуля тендеринга:

- ``data.changes.status``
- ``data.changes.rationaleTypes``


Формула розрахунку
==================

1. Якщо в даних контракту відсутній блок ``data.changes``, індикатор приймає значення ``-2``. Розрахунок завершується.

2. Якщо в даних контракту присутній блок ``data.changes``, що має статус ``data.changes.status = 'active'`` та серед визначених причин внесення змін ``data.changes.rationaleTypes`` присутня ``durationExtension``, індикатор приймає значення ``1``, розрахунок завершуэться.
3. Якщо ми дійшли до цього пункту, індикатор приймає значення ``0``, розрахунок завершується.


Фактори, що впливають на неточність розрахунку
==============================================

Індикатор може бути порахований неточно у випадках, коли замовники не вірно вказують в системі причину внесення змін до договору.
