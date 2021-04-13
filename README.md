# database
## Вариант 4 Khoroshilova M. Pavlova D. 19SE-1
### Уровень 1
1) Создаём таблицы
```sql
create table МЕДПЕРСОНАЛ
(
    ИДЕНТИФИКАТОР serial                                                 not null primary key,
    ФАМИЛИЯ       varchar(31)                                            not null,
    АДРЕС         varchar(255)                                           not null,
    "НАЛОГ, %"    smallint check ("НАЛОГ, %" >= 0 AND "НАЛОГ, %" <= 100) not null
);

create table "МЕСТО РАБОТЫ"
(
    ИДЕНТИФИКАТОР                    serial                                                   not null primary key,
    УЧРЕЖДЕНИЕ                       varchar(31)                                              not null,
    АДРЕС                            varchar(255)                                             not null,
    "ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %" smallint check ("ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %" >= 0 AND
                                                     "ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %" <= 100) not null
);

create table "ТИПЫ ОПЕРАЦИЙ"
(
    ИДЕНТИФИКАТОР    serial                               not null primary key,
    НАИМЕНОВАНИЕ     varchar(255)                         not null,
    "ОПОРНЫЙ ПУНКТ"  varchar(255)                         not null,
    ЗАПАСЫ           integer check (ЗАПАСЫ > 0)           not null,
    "СТОИМОСТЬ, РУБ" integer check ("СТОИМОСТЬ, РУБ" > 0) not null
);

create table "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
(
    ДОГОВОР        serial                            not null primary key,
    ДАТА           varchar(15)                       not null,
    "МЕД ПЕРСОНАЛ" serial                            not null references "МЕДПЕРСОНАЛ" ("ИДЕНТИФИКАТОР"),
    "МЕСТО РАБОТЫ" serial                            not null references "МЕСТО РАБОТЫ" ("ИДЕНТИФИКАТОР"),
    ОПЕРАЦИИ       serial                            not null references "ТИПЫ ОПЕРАЦИЙ" ("ИДЕНТИФИКАТОР"),
    "КОЛ-ВО"       smallint check ("КОЛ-ВО" > 0)     not null,
    "ОПЛАТА, РУБ"  integer check ("ОПЛАТА, РУБ" > 0) not null
);
```
2) Заполняем таблицы данными
```sql
INSERT INTO "МЕДПЕРСОНАЛ"("ИДЕНТИФИКАТОР", "ФАМИЛИЯ", "АДРЕС", "НАЛОГ, %")
VALUES (001, 'Медина', 'Вознесенское', 14),
       (002, 'Севастьянов',	'Навашино',	14),
       (003, 'Бессонов', 'Выкса', 10),
       (004, 'Губанов', 'Выкса', 10),
       (005, 'Боева', 'Починки', 5);

INSERT INTO "МЕСТО РАБОТЫ"("ИДЕНТИФИКАТОР", "УЧРЕЖДЕНИЕ", "АДРЕС", "ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %")
VALUES (001, 'Районная больница', 'Вознесенское', 10),
       (002, 'Травм.пункт', 'Выкса', 3),
       (003, 'Больница', 'Навашино', 4),
       (004, 'Род.дом', 'Вознесенское', 12),
       (005, 'Больница', 'Починки',	4),
       (006, 'Травм.пункт',	'Лукояново', 3);

INSERT INTO "ТИПЫ ОПЕРАЦИЙ"("ИДЕНТИФИКАТОР", "НАИМЕНОВАНИЕ", "ОПОРНЫЙ ПУНКТ", "ЗАПАСЫ", "СТОИМОСТЬ, РУБ")
VALUES (001, 'Наложение гипса',	'Выкса', 2000, 18000),
       (002, 'Блокада',	'Навашино',	10000, 14000),
       (003, 'Инъекция поливитаминов', 'Навашино', 20000, 11000),
       (004, 'Инъекция алоэ', 'Навашино', 12000, 11000),
       (005, 'ЭКГ',	'Вознесенское',	115, 10000),
       (006, 'УЗИ',	'Вознесенское',	20,	30000),
       (007, 'Флюорография', 'Выкса', 1000, 5000);

INSERT INTO "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"("ДОГОВОР", "ДАТА", "МЕД ПЕРСОНАЛ", "МЕСТО РАБОТЫ", "ОПЕРАЦИИ", "КОЛ-ВО", "ОПЛАТА, РУБ")
VALUES (51040, 'Понедельник', 001, 001, 007,	4, 20000),
       (51041, 'Понедельник', 003, 003, 006,	1, 30000),
       (51042, 'Понедельник', 004, 003, 004,	3, 33000),
       (51043, 'Понедельник', 004, 005,	001, 2, 36000),
       (51044, 'Понедельник', 004,	004, 006, 1, 30000),
       (51045, 'Среда', 002,	002, 005, 3, 30000),
       (51046, 'Четверг', 003,	006, 004, 4, 44000),
       (51047, 'Четверг', 004, 006, 002,	1, 28000),
       (51048, 'Четверг', 005, 003, 003,	4, 44000),
       (51049, 'Пятница', 002, 004,	005, 1, 10000),
       (51050, 'Пятница', 003, 006,	004, 2,	22000),
       (51051, 'Пятница', 003, 003,	001, 2,	36000),
       (51052, 'Пятница', 005, 003, 002, 1, 14000),
       (51053, 'Суббота', 003,	002, 007, 2, 10000),
       (51054, 'Суббота', 004, 006, 004,	1, 11000),
       (51055, 'Суббота', 005, 005, 004,	2, 22000),
       (51056, 'Суббота', 003, 006, 003,	2, 22000);
```
3) Выводим таблицы:
```sql
SELECT *
FROM "МЕДПЕРСОНАЛ";
```
| ИДЕНТИФИКАТОР | ФАМИЛИЯ | АДРЕС | НАЛОГ, % |
| :--- | :--- | :--- | :--- |
| 1 | Медина | Вознесенское | 14 |
| 2 | Севастьянов | Навашино | 14 |
| 3 | Бессонов | Выкса | 10 |
| 4 | Губанов | Выкса | 10 |
| 5 | Боева | Починки | 5 |

```sql
SELECT *
FROM "МЕСТО РАБОТЫ";
```
| ИДЕНТИФИКАТОР | УЧРЕЖДЕНИЕ | АДРЕС | ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, % |
| :--- | :--- | :--- | :--- |
| 1 | Районная больница | Вознесенское | 10 |
| 2 | Травм. пункт | Выкса | 3 |
| 3 | Больница | Навашино | 4 |
| 4 | Род. дом | Вознесенское | 12 |
| 5 | Больница | Починки | 4 |
| 6 | Травм.пункт | Лукояново | 3 |

```sql
SELECT *
FROM "ТИПЫ ОПЕРАЦИЙ";
```
| ИДЕНТИФИКАТОР | НАИМЕНОВАНИЕ | ОПОРНЫЙ ПУНКТ | ЗАПАСЫ | СТОИМОСТЬ, РУБ |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Наложение гипса | Выкса | 2000 | 18000 |
| 2 | Блокада | Навашино | 10000 | 14000 |
| 3 | Инъекция поливитаминов | Навашино | 20000 | 11000 |
| 4 | Инъекция алоэ | Навашино | 12000 | 11000 |
| 5 | ЭКГ | Вознесенское | 115 | 10000 |
| 6 | УЗИ | Вознесенское | 20 | 30000 |
| 7 | Флюорография | Выкса | 1000 | 5000 |

```sql
SELECT *
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ";
```
| ДОГОВОР | ДАТА | МЕД ПЕРСОНАЛ | МЕСТО РАБОТЫ | ОПЕРАЦИИ | КОЛ-ВО | ОПЛАТА, РУБ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 51040 | Январь | 1 | 1 | 7 | 4 | 20000 |
| 51041 | Январь | 3 | 3 | 6 | 1 | 30000 |
| 51042 | Январь | 4 | 3 | 4 | 3 | 33000 |
| 51043 | Февраль | 4 | 5 | 1 | 2 | 36000 |
| 51044 | Февраль | 4 | 4 | 6 | 1 | 30000 |
| 51045 | Март | 2 | 2 | 5 | 3 | 30000 |
| 51046 | Март | 3 | 6 | 4 | 4 | 44000 |
| 51047 | Апрель | 4 | 6 | 2 | 1 | 28000 |
| 51048 | Апрель | 5 | 3 | 3 | 4 | 44000 |
| 51049 | Май | 2 | 4 | 5 | 1 | 10000 |
| 51050 | Май | 3 | 6 | 4 | 2 | 22000 |
| 51051 | Май | 3 | 3 | 1 | 2 | 36000 |
| 51052 | Июнь | 5 | 3 | 2 | 1 | 14000 |
| 51053 | Июль | 3 | 2 | 7 | 2 | 10000 |
| 51054 | Август | 4 | 6 | 4 | 1 | 11000 |
| 51055 | Август | 5 | 5 | 4 | 2 | 22000 |
| 51056 | Август | 3 | 6 | 3 | 2 | 22000 |

4) Выводим с помощью запросов:
 * a) различные адреса всех медработников;
  ```sql
SELECT DISTINCT "АДРЕС" FROM "МЕДПЕРСОНАЛ";
  ```
| АДРЕС |
| :--- |
| Навашино |
| Выкса |
| Вознесенское |
| Починки |

* b) список всех различных медучреждений;
```sql
SELECT DISTINCT "УЧРЕЖДЕНИЕ" FROM "МЕСТО РАБОТЫ";
```
| УЧРЕЖДЕНИЕ |
| :--- |
| Больница |
| Род.дом |
| Травм.пункт |
| Районная больница |

* c)	различные даты, для которых хранится информация о трудовой деятельности;
```sql
SELECT DISTINCT "ДАТА" FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ";
```
| ДАТА |
| :--- |
| Май |
| Апрель |
| Июнь |
| Февраль |
| Июль |
| Август |
| Март |
| Январь |

5) Найти:
* a)	даты и номера договоров, когда производились операции на сумму не менее 14000руб.
```sql
SELECT "ДОГОВОР", "ДАТА"
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "ОПЛАТА, РУБ" > 14000
```
| ДОГОВОР | ДАТА |
| :--- | :--- |
| 51040 | Январь |
| 51041 | Январь |
| 51042 | Январь |
| 51043 | Февраль |
| 51044 | Февраль |
| 51045 | Март |
| 51046 | Март |
| 51047 | Апрель |
| 51048 | Апрель |
| 51050 | Май |

* b)	размер налога для медперсонала из Выксы или Навашино;
```sql
SELECT DISTINCT "НАЛОГ, %"
FROM "МЕДПЕРСОНАЛ"
WHERE "АДРЕС" = 'Выкса'
   OR "АДРЕС" = 'Навашино'
```
| НАЛОГ, % |
| :--- |
| 10 |
| 14 |

* c)	название, стоимость и адрес опорного пункта для операций,
в названии которых есть слово “Инъекция”, и стоящих более 10000руб.
Результат отсортировать по адресу и стоимости.
```sql
SELECT "НАИМЕНОВАНИЕ", "СТОИМОСТЬ, РУБ", "ОПОРНЫЙ ПУНКТ"
FROM "ТИПЫ ОПЕРАЦИЙ"
WHERE "НАИМЕНОВАНИЕ" like '%Инъекция%'
  and "СТОИМОСТЬ, РУБ" > 10000
ORDER BY "СТОИМОСТЬ, РУБ", "НАИМЕНОВАНИЕ" ASC;
```
| НАИМЕНОВАНИЕ | СТОИМОСТЬ, РУБ | ОПОРНЫЙ ПУНКТ |
| :--- | :--- | :--- |
| Инъекция алоэ | 11000 | Навашино |
| Инъекция поливитаминов | 11000 | Навашино |

6) На основании данных о проведенных операциях выводим в следующем формате все записи:
* a) дата, фамилия медперсонала, название места работы, название операции;
```sql
SELECT "ДАТА", "ФАМИЛИЯ", "МЕСТО РАБОТЫ"."УЧРЕЖДЕНИЕ", "НАИМЕНОВАНИЕ"
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ",
     "МЕДПЕРСОНАЛ",
     "МЕСТО РАБОТЫ",
     "ТИПЫ ОПЕРАЦИЙ"
WHERE "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ" = "ТИПЫ ОПЕРАЦИЙ"."ИДЕНТИФИКАТОР"
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ" = "МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР";
```
| ДАТА | ФАМИЛИЯ | УЧРЕЖДЕНИЕ | НАИМЕНОВАНИЕ |
| :--- | :--- | :--- | :--- |
| Понедельник | Медина | Районная больница | Флюорография |
| Понедельник | Бессонов | Больница | УЗИ |
| Понедельник | Губанов | Больница | Инъекция алоэ |
| Понедельник | Губанов | Больница | Наложение гипса |
| Понедельник | Губанов | Род.дом | УЗИ |
| Среда | Севастьянов | Травм.пункт | ЭКГ |
| Четверг | Бессонов | Травм.пункт | Инъекция алоэ |
| Четверг | Губанов | Травм.пункт | Блокада |
| Четверг | Боева | Больница | Инъекция поливитаминов |
| Пятница | Севастьянов | Род.дом | ЭКГ |
| Пятница | Бессонов | Травм.пункт | Инъекция алоэ |
| Пятница | Бессонов | Больница | Наложение гипса |
| Пятница | Боева | Больница | Блокада |
| Суббота | Бессонов | Травм.пункт | Флюорография |
| Суббота | Губанов | Травм.пункт | Инъекция алоэ |
| Суббота | Боева | Больница | Инъекция алоэ |
| Суббота | Бессонов | Травм.пункт | Инъекция поливитаминов |

* b) номер договора, название места работы, количество операций, оплата. Отсортировать по возрастанию оплаты.
```sql
SELECT DISTINCT "ДОГОВОР", "МЕСТО РАБОТЫ"."УЧРЕЖДЕНИЕ", "КОЛ-ВО", "ОПЛАТА, РУБ"
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ",
     "МЕДПЕРСОНАЛ",
     "МЕСТО РАБОТЫ"
WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ" = "МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР"
ORDER BY "ОПЛАТА, РУБ" DESC;

```
| ДОГОВОР | УЧРЕЖДЕНИЕ | КОЛ-ВО | ОПЛАТА, РУБ |
| :--- | :--- | :--- | :--- |
| 51046 | Травм.пункт | 4 | 44000 |
| 51048 | Больница | 4 | 44000 |
| 51043 | Больница | 2 | 36000 |
| 51051 | Больница | 2 | 36000 |
| 51042 | Больница | 3 | 33000 |
| 51041 | Больница | 1 | 30000 |
| 51044 | Род.дом | 1 | 30000 |
| 51045 | Травм.пункт | 3 | 30000 |
| 51047 | Травм.пункт | 1 | 28000 |
| 51050 | Травм.пункт | 2 | 22000 |
| 51055 | Больница | 2 | 22000 |
| 51056 | Травм.пункт | 2 | 22000 |
| 51040 | Районная больница | 4 | 20000 |
| 51052 | Больница | 1 | 14000 |
| 51054 | Травм.пункт | 1 | 11000 |
| 51049 | Род.дом | 1 | 10000 |
| 51053 | Травм.пункт | 2 | 10000 |

7.	Определить:
* a)	фамилии и места проживания медперсонала, проведших более одного наложения гипса в день;
```sql
SELECT DISTINCT "МЕДПЕРСОНАЛ"."ФАМИЛИЯ", "МЕДПЕРСОНАЛ"."АДРЕС"
FROM "МЕДПЕРСОНАЛ",
     "ТИПЫ ОПЕРАЦИЙ",
     "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ" = 1
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."КОЛ-ВО" > 1
```
| ФАМИЛИЯ | АДРЕС |
| :--- | :--- |
| Губанов | Выкса |
| Бессонов | Выкса |

* b)	название операций, которые проводили врачи из Вознесенского или Выксы в больницах;
```sql
SELECT DISTINCT "ТИПЫ ОПЕРАЦИЙ"."НАИМЕНОВАНИЕ"
FROM "МЕДПЕРСОНАЛ",
     "ТИПЫ ОПЕРАЦИЙ",
     "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ" = "ТИПЫ ОПЕРАЦИЙ"."ИДЕНТИФИКАТОР"
  AND ("МЕДПЕРСОНАЛ"."АДРЕС" = 'Вознесенское' OR "МЕДПЕРСОНАЛ"."АДРЕС" = 'Навашино')
```
| НАИМЕНОВАНИЕ |
| :--- |
| Флюорография |
| ЭКГ |

* c)	названия и размер отчислений в местный бюджет для тех учреждений,
где проводили операции те, у кого налог не менее 7%, но не более 16%.
Включить в вывод фамилии таких людей и отсортировать по размеру отчислений и налогу;
```sql
SELECT "МЕДПЕРСОНАЛ"."ФАМИЛИЯ",
                "МЕСТО РАБОТЫ"."УЧРЕЖДЕНИЕ",
                "МЕСТО РАБОТЫ"."ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %"
FROM "МЕДПЕРСОНАЛ",
     "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ",
     "МЕСТО РАБОТЫ"
WHERE "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"
  AND "МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ"
  AND "МЕДПЕРСОНАЛ"."НАЛОГ, %" > 7
  AND "МЕДПЕРСОНАЛ"."НАЛОГ, %" < 16
GROUP BY "МЕСТО РАБОТЫ"."УЧРЕЖДЕНИЕ", "МЕДПЕРСОНАЛ"."ФАМИЛИЯ", "МЕСТО РАБОТЫ"."ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %"
ORDER BY max("МЕСТО РАБОТЫ"."ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %"), max("МЕДПЕРСОНАЛ"."НАЛОГ, %")
```
| ФАМИЛИЯ | УЧРЕЖДЕНИЕ | ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, % |
| :--- | :--- | :--- |
| Губанов | Травм.пункт | 3 |
| Бессонов | Травм.пункт | 3 |
| Севастьянов | Травм.пункт | 3 |
| Бессонов | Больница | 4 |
| Губанов | Больница | 4 |
| Медина | Районная больница | 10 |
| Губанов | Род.дом | 12 |
| Севастьянов | Род.дом | 12 |

* d)	даты, идентификаторы операций и фамилии тех, кто проводил операции стоимостью не менее 7000руб больше одного раза.
```sql
SELECT DISTINCT "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ДАТА",
                "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ",
                "МЕДПЕРСОНАЛ"."ФАМИЛИЯ"
FROM "МЕДПЕРСОНАЛ",
     "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ",
     "ТИПЫ ОПЕРАЦИЙ"
WHERE "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ" = "ТИПЫ ОПЕРАЦИЙ"."ИДЕНТИФИКАТОР"
  AND "ТИПЫ ОПЕРАЦИЙ"."СТОИМОСТЬ, РУБ" >= 7000
  AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."КОЛ-ВО" > 1
```
| ДАТА | ОПЕРАЦИИ | ФАМИЛИЯ |
| :--- | :--- | :--- |
| Март | 5 | Севастьянов |
| Август | 3 | Бессонов |
| Август | 4 | Боева |
| Январь | 4 | Губанов |
| Апрель | 3 | Боева |
| Февраль | 1 | Губанов |
| Май | 1 | Бессонов |
| Май | 4 | Бессонов |
| Март | 4 | Бессонов |

8) Создаём запрос для модификации всех значений столбца с суммарной величиной оплаты, чтобы он содержал истинную сумму, получаемую медперсоналом ( за вычетом налога).
```sql
UPDATE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
SET "ОПЛАТА, РУБ" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЛАТА, РУБ"/100 * (100 - "МЕДПЕРСОНАЛ"."НАЛОГ, %")
FROM "МЕДПЕРСОНАЛ" WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ" = "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР";
```

Выводим новые значения.
```sql
SELECT *
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
ORDER BY "ОПЛАТА, РУБ" DESC;
```
| ДОГОВОР | ДАТА | МЕД ПЕРСОНАЛ | МЕСТО РАБОТЫ | ОПЕРАЦИИ | КОЛ-ВО | ОПЛАТА, РУБ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 51048 | Четверг | 5 | 3 | 3 | 4 | 41800 |
| 51046 | Четверг | 3 | 6 | 4 | 4 | 39600 |
| 51043 | Понедельник | 4 | 5 | 1 | 2 | 32400 |
| 51051 | Пятница | 3 | 3 | 1 | 2 | 32400 |
| 51042 | Понедельник | 4 | 3 | 4 | 3 | 29700 |
| 51044 | Понедельник | 4 | 4 | 6 | 1 | 27000 |
| 51041 | Понедельник | 3 | 3 | 6 | 1 | 27000 |
| 51045 | Среда | 2 | 2 | 5 | 3 | 25800 |
| 51047 | Четверг | 4 | 6 | 2 | 1 | 25200 |
| 51055 | Суббота | 5 | 5 | 4 | 2 | 20900 |
| 51056 | Суббота | 3 | 6 | 3 | 2 | 19800 |
| 51050 | Пятница | 3 | 6 | 4 | 2 | 19800 |
| 51040 | Понедельник | 1 | 1 | 7 | 4 | 17200 |
| 51052 | Пятница | 5 | 3 | 2 | 1 | 13300 |
| 51054 | Суббота | 4 | 6 | 4 | 1 | 9900 |
| 51053 | Суббота | 3 | 2 | 7 | 2 | 9000 |
| 51049 | Пятница | 2 | 4 | 5 | 1 | 8600 |

9) Расширяем таблицу с данными об операциях столбцом, содержащим величину отчислений в местный бюджет для мед.учреждения, где проводилась операция. 
```sql
ALTER TABLE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ" ADD "ОТЧИСЛЕНИЯ В МЕСТНЫЙ БЮДЖЕТ, РУБ" INTEGER;

UPDATE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
SET "ОТЧИСЛЕНИЯ В МЕСТНЫЙ БЮДЖЕТ, РУБ" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЛАТА, РУБ"/100 * ("МЕСТО РАБОТЫ"."ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %")
FROM "МЕСТО РАБОТЫ" WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ" = "МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР";
```

Создаём запрос для ввода конкретных значений во все строки таблицы операций.
```sql
SELECT *
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
ORDER BY "ОПЛАТА, РУБ" DESC;
```
| ДОГОВОР | ДАТА | МЕД ПЕРСОНАЛ | МЕСТО РАБОТЫ | ОПЕРАЦИИ | КОЛ-ВО | ОПЛАТА, РУБ | ОТЧИСЛЕНИЯ В МЕСТНЫЙ БЮДЖЕТ, РУБ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 51048 | Четверг | 5 | 3 | 3 | 4 | 41800 | 1672 |
| 51046 | Четверг | 3 | 6 | 4 | 4 | 39600 | 1188 |
| 51043 | Понедельник | 4 | 5 | 1 | 2 | 32400 | 1296 |
| 51051 | Пятница | 3 | 3 | 1 | 2 | 32400 | 1296 |
| 51042 | Понедельник | 4 | 3 | 4 | 3 | 29700 | 1188 |
| 51044 | Понедельник | 4 | 4 | 6 | 1 | 27000 | 3240 |
| 51041 | Понедельник | 3 | 3 | 6 | 1 | 27000 | 1080 |
| 51045 | Среда | 2 | 2 | 5 | 3 | 25800 | 774 |
| 51047 | Четверг | 4 | 6 | 2 | 1 | 25200 | 756 |
| 51055 | Суббота | 5 | 5 | 4 | 2 | 20900 | 836 |
| 51056 | Суббота | 3 | 6 | 3 | 2 | 19800 | 594 |
| 51050 | Пятница | 3 | 6 | 4 | 2 | 19800 | 594 |
| 51040 | Понедельник | 1 | 1 | 7 | 4 | 17200 | 1720 |
| 51052 | Пятница | 5 | 3 | 2 | 1 | 13300 | 532 |
| 51054 | Суббота | 4 | 6 | 4 | 1 | 9900 | 297 |
| 51053 | Суббота | 3 | 2 | 7 | 2 | 9000 | 270 |
| 51049 | Пятница | 2 | 4 | 5 | 1 | 8600 | 1032 |

### Уровень 2
10) Используя операцию IN (NOT IN)  реализовать следующие запросы:
* a)	найти фамилии медперсонала из Навашино, проводивших инъекции в Выксе;
```sql
```
* b)	найти те операции, которые не проводились до среды;
```sql
```
* c)	запросы задания 7.с и 7.d.
```sql
```

11) Используя операции ALL-ANY реализовать следующие запросы:
* a) найти среди больниц ту, которая имеет наименьший процент отчислений;
```sql
SELECT DISTINCT "УЧРЕЖДЕНИЕ"
FROM "МЕСТО РАБОТЫ"
WHERE "ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %" = ALL (SELECT MIN("ОТЧИСЛЕНИЕ В МЕСТНЫЙ БЮДЖЕТ, %")
          FROM "МЕСТО РАБОТЫ");
```
| УЧРЕЖДЕНИЕ |
| :--- |
| Травм.пункт |
* b) найти педперсонал, проводивший операции с самой малой суммой оплаты;
```sql
SELECT DISTINCT "ФАМИЛИЯ"
FROM "МЕДПЕРСОНАЛ","ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "ОПЛАТА, РУБ" = ALL (SELECT MIN("ОПЛАТА, РУБ")
          FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ")
AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"="МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР";
```
| ФАМИЛИЯ |
| :--- |
| Севастьянов |
* c) найти цену самой дорогой операции, проведенной в четверг или пятницу;
```sql
SELECT DISTINCT "СТОИМОСТЬ, РУБ"
FROM "ТИПЫ ОПЕРАЦИЙ","ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "ОПЛАТА, РУБ" = ALL (SELECT MAX("ОПЛАТА, РУБ")
          FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ")
AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"="ТИПЫ ОПЕРАЦИЙ"."ИДЕНТИФИКАТОР"
AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ДАТА" = ANY(SELECT "ДАТА" FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "ДАТА" IN ('Четверг', 'Пятница'));
```
| СТОИМОСТЬ, РУБ |
| :--- |
| 10000 |
* d) запрос задания 7.а
```sql
SELECT DISTINCT "МЕДПЕРСОНАЛ"."ФАМИЛИЯ", "МЕДПЕРСОНАЛ"."АДРЕС"
FROM "МЕДПЕРСОНАЛ",
     "ТИПЫ ОПЕРАЦИЙ",
     "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."КОЛ-ВО" = ANY(SELECT "КОЛ-ВО" FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "КОЛ-ВО">1)
  AND "ОПЕРАЦИИ" = 1
  AND "МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР" = "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ";
```
| ФАМИЛИЯ | АДРЕС |
| :--- | :--- |
| Губанов | Выкса |
| Бессонов | Выкса |

12.	Используя операцию UNION получить места проживания медпероснала и опероные пункты для операций.
```sql
```

13) Используя операцию EXISTS ( NOT EXISTS ) реализовать нижеследующие запросы. В случае, если для текущего состояния БД запрос будет выдавать пустое множество строк, требуется указать какие добавления в БД необходимо провести.
* a) определить тот медперсонал, который не работал в субботу;
```sql
SELECT "ФАМИЛИЯ"
FROM "МЕДПЕРСОНАЛ"
WHERE NOT EXISTS(
        SELECT "МЕД ПЕРСОНАЛ"
        FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
        WHERE "ДАТА" = 'Суббота' AND "МЕД ПЕРСОНАЛ"="МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР");
```
| ФАМИЛИЯ |
| :--- |
| Медина |
| Севастьянов |
* b) найти такие операции, которые проводились всеми врачами в Выксе;
```sql
SELECT "НАИМЕНОВАНИЕ"
FROM "ТИПЫ ОПЕРАЦИЙ"
WHERE NOT EXISTS(
        SELECT "ОПЕРАЦИИ"
        FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
        WHERE "МЕСТО РАБОТЫ" <> 2  AND "ОПЕРАЦИИ"="ТИПЫ ОПЕРАЦИЙ"."ИДЕНТИФИКАТОР"); -- Выкса id = 2
```
| НАИМЕНОВАНИЕ |
| :--- |

Возможные изменения:
1. добавить новый вид операции и добавить запись о проведении её в Выксе
2. изменить существующие записи о проведении операции(н-р ЭКГ(id=5) было проведено в городах 2 и 4 - удалить или редактировать запись с городом 4)
* c) определить те места работы, где не делали УЗИ более раза;
```sql
SELECT "УЧРЕЖДЕНИЕ"
FROM "МЕСТО РАБОТЫ"
WHERE NOT EXISTS(
        SELECT "ОПЕРАЦИИ"
        FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ" -- УЗИ id = 6
        WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ" = 6  AND "КОЛ-ВО">1
          AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ"="МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР");
```
| УЧРЕЖДЕНИЕ |
| :--- |
| Районная больница |
| Травм.пункт |
| Больница |
| Род.дом |
| Больница |
| Травм.пункт |

Получаем все учреждения, так как если УЗИ и делали, то по 1 разу.

* d) определить места работы, где работали все врачи из чужих населенных пунктов
```sql
SELECT "УЧРЕЖДЕНИЕ","АДРЕС"
FROM "МЕСТО РАБОТЫ"
WHERE NOT EXISTS(
        SELECT "ИДЕНТИФИКАТОР"
        FROM "МЕДПЕРСОНАЛ"
        WHERE "МЕСТО РАБОТЫ"."АДРЕС"="МЕДПЕРСОНАЛ"."АДРЕС" AND EXISTS(
            SELECT "МЕСТО РАБОТЫ"
            FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
            WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ"=public."МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР" 
              AND "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"="МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР"));
```
| УЧРЕЖДЕНИЕ | АДРЕС |
| :--- | :--- |
| Больница | Навашино |
| Род.дом | Вознесенское |
| Травм.пункт | Лукояново |
14.	Реализовать запросы с использованием аггрегатных функций:
* a)	найти число различных мест работы для медперсонала, работавшего в мед.учреждениях Выксы;
```sql
```
* b)	определить средний размер налога для медперсонала, производившего иньекции;
```sql
```
* c)	кто из медперсонала делал операцию с минимальной стоимостью;
```sql
```
* d)	определить количество операций стоимостью не более 15000, проведенных в понедельник Губановым .
```sql
```

15) Используя средства группировки реализовать следующие запросы:
* a)  определить для каждого дня недели и каждой операции сколько раз ее проводили;
```sql
SELECT "НАИМЕНОВАНИЕ","ДАТА", SUM("КОЛ-ВО")
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ","ТИПЫ ОПЕРАЦИЙ"
WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."ОПЕРАЦИИ"="ТИПЫ ОПЕРАЦИЙ"."ИДЕНТИФИКАТОР"
GROUP BY "НАИМЕНОВАНИЕ","ДАТА"
ORDER BY "ДАТА";
```
| НАИМЕНОВАНИЕ | ДАТА | sum |
| :--- | :--- | :--- |
| Инъекция алоэ | Понедельник | 3 |
| Наложение гипса | Понедельник | 2 |
| УЗИ | Понедельник | 2 |
| Флюорография | Понедельник | 4 |
| Блокада | Пятница | 1 |
| Инъекция алоэ | Пятница | 2 |
| Наложение гипса | Пятница | 2 |
| ЭКГ | Пятница | 1 |
| ЭКГ | Среда | 3 |
| Инъекция алоэ | Суббота | 3 |
| Инъекция поливитаминов | Суббота | 2 |
| Флюорография | Суббота | 2 |
| Блокада | Четверг | 1 |
| Инъекция алоэ | Четверг | 4 |
| Инъекция поливитаминов | Четверг | 4 |

* b)  найти для каждого медработника среднюю стоимость всех проведенных им операций;
```sql
SELECT "ФАМИЛИЯ", AVG("ОПЛАТА, РУБ")
FROM "МЕДПЕРСОНАЛ", "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
WHERE "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕД ПЕРСОНАЛ"="МЕДПЕРСОНАЛ"."ИДЕНТИФИКАТОР"
GROUP BY "ФАМИЛИЯ";
```
| ФАМИЛИЯ | avg |
| :--- | :--- |
| Бессонов | 24600 |
| Губанов | 24840 |
| Медина | 17200 |
| Севастьянов | 17200 |
| Боева | 25333.333333333333 |

* c)  определить те мед.учреждения, где суммарная величина стоимости всех проведенных в них операций была более 30000;
```sql
SELECT "УЧРЕЖДЕНИЕ", "АДРЕС"
FROM "МЕСТО РАБОТЫ"
JOIN "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ" ON "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"."МЕСТО РАБОТЫ"="МЕСТО РАБОТЫ"."ИДЕНТИФИКАТОР"
GROUP BY "УЧРЕЖДЕНИЕ", "АДРЕС" HAVING SUM("ОПЛАТА, РУБ")>30000
```
| УЧРЕЖДЕНИЕ | АДРЕС |
| :--- | :--- |
| Род.дом | Вознесенское |
| Травм.пункт | Выкса |
| Больница | Починки |
| Больница | Навашино |
| Травм.пункт | Лукояново |

* d)  для каждого дня недели найти число проведенных в этот день операций
```sql
SELECT "ДАТА", SUM("КОЛ-ВО")
FROM "ТРУДОВАЯ ДЕЯТЕЛЬНОСТЬ"
GROUP BY "ДАТА"
ORDER BY "ДАТА";
```
| ДАТА | sum |
| :--- | :--- |
| Понедельник | 11 |
| Пятница | 6 |
| Среда | 3 |
| Суббота | 7 |
| Четверг | 9 |

