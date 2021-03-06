﻿.. _tbl_supplierForPEWith3CPV:

=========================
tbl_supplierForPEWith3CPV
=========================

************************
Суть аналітичної таблиці
************************

Дана аналітична таблиця відображає перелік Постачальників для кожного Замовника, з якими Замовник укладав контракти, а також кількість унікальних CPV, що були предметами закупівель в цих контрактах.  

*************************
Форма аналітичної таблиці
*************************

Аналітична таблиця містить наступну інформацію:

- Ідентифікатор замовника.
- Ідентифікатор постачальника.
- Кількість унікальних груп CPV.

Приклад того, як може виглядати аналітична таблиця:

========== ============== =============
Замовник   Постачальник   Кількість CPV
---------- -------------- -------------
Замовник 1 Постачальник 1 Кількість
Замовник 1 Постачальник 2 Кількість
Замовник 1 Постачальник 3 Кількість
Замовник 2 Постачальник 1 Кількість
Замовник 2 Постачальник 2 Кількість
...        ...            ...
========== ============== =============

******************************
Розрахунок аналітичної таблиці
******************************

Джерела даних для розрахунку
============================

Для розрахунку аналітичної таблиці вікористовуються наступні джерела даних:

- API модуля тендеринга електронної системи закупівель

Частота розрахунку
==================

Аналітична таблиця розраховується раз на добу.

Поля для розрахунку
===================

Для розрахунку використовуються наступні поля з API модуля тендеринга:

-  ``data.contracts.status``

- ``data.contracts.items.classification.id``

- ``data.procuringEntity.identifier.scheme``

- ``data.procuringEntity.identifier.id``

- ``data.contracts.suppliers.identifier.scheme``

- ``data.contracts.suppliers.identifier.id``

- ``data.status``

Формула розрахунку
==================

1. На початку розрахунку таблиця очищується.

2. До уваги беруться завершені процедури, тобто такі, що мають ``data.status = 'complete'``.

3. Знаходимо ідентифікатор замовника процедури - конкатенація ``data.procuringEntity.identifier.scheme`` та ``data.procuringEntity.identifier.id``.

4. З процедур вибираємо активні блоки контрактів - такі ``data.contracts``, що мають статус ``data.contracts.status = 'active'``.

5. З кожного знайденого ``data.contracts`` беремо CPV предмета закупівлі - ``data.contracts.items.classification.id`` і ідентификатор постачальника - конкатенація ``data.contracts.suppliers.identifier.scheme`` та ``data.contracts.suppliers.identifier.id``.

6. Для кожного CPV визначаємо його групу CPV - беремо перші 4 цифри CPV.

6. Групуємо отримані дані по ідентифікатору замовника та ідентифікатору постачальника, рахуючи кількість унікальних груп CPV предметів закупівлі. Результат групування сформує нашу таблицю.

