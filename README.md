# database
## Вариант 4 Khoroshilova M. Pavlova D. 19SE-1
### Уровень 1
1)
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
    ДАТА           varchar(10)                       not null,
    "МЕД ПЕРСОНАЛ" serial                            not null references МЕДПЕРСОНАЛ (ИДЕНТИФИКАТОР),
    "МЕСТО РАБОТЫ" serial                            not null references "МЕСТО РАБОТЫ" (ИДЕНТИФИКАТОР),
    ОПЕРАЦИИ       serial                            not null references "ТИПЫ ОПЕРАЦИЙ" (ИДЕНТИФИКАТОР),
    "КОЛ-ВО"       smallint check ("КОЛ-ВО" > 0)     not null,
    "ОПЛАТА, РУБ"  integer check ("ОПЛАТА, РУБ" > 0) not null
);
```