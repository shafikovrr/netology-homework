# Домашнее задание к занятию "`Базы данных`" - `Шафиков Ринат`

### Легенда

Заказчик передал вам файл в формате Excel, в котором сформирован отчёт.

На основе этого отчёта нужно выполнить следующие задания.

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных:
- какие данные хранятся в этих таблицах;
- какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
Приведите решение к следующему виду:

Сотрудники (
идентификатор int, auto_increment, serial,
фамилия varchar(50),
...
идентификатор структурного подразделения, внешний ключ, integer).

### Решение 1

#### Данные хранящиеся в таблице:

- ФИО сотрудника	varchar
- Оклад	DECIMAL/NUMERIC
- Должность	varchar
- Тип подразделения	varchar
- Структурное подразделение	varchar
- Дата найма	date
- Адрес филиала	varchar
- Проект на который назначен varchar

#### PostgreSQL:

- Оклад	DECIMAL/NUMERIC
- Должность	TEXT
- Тип подразделения	TEXT
- Структурное подразделение	TEXT
- Дата найма	date
- Адрес филиала	TEXT
- Проект на который назначен TEXT

#### Таблица 1 Сотрудники
```
Worker (
worker_id    INT NOT NULL  SERIAL  PRIMARY KEY
first_name   varchar(25) NOT NULL
last_name    varchar(25) NOT NULL
middle_name, varchar(25)) NOT NULL
);
```
#### Таблица 2 Оклад
```
salary (
salary_id      INT  NOT NULL  SERIAL  PRIMARY KEY
salary_amount  NUMERIC) NOT NULL
);
```
#### Таблица 3 Дата найма
```
Date of employment (
date_employment_id  INT  NOT NULL  SERIAL  PRIMARY KEY
data                DATE)  NOT NULL
);
```
#### Таблица 4 Должность
```
Post (
post_id        INT  NOT NULL  SERIAL  PRIMARY KEY
post_name      varchar(50)  NOT NULL
);
```
#### Таблица 5 Тип подразделения
```
type_division (
division_id,     INT  NOT NULL  SERIAL  PRIMARY KEY
должность        varchar(25)  NOT NULL
);
```
#### Таблица 6 Структурное подразделение
```
structural_division (
structural_division_id    INT  NOT NULL  SERIAL  PRIMARY KEY
structural_division_name varchar(50)  NOT NULL
);
```
#### Таблица 7 Административно территориальная единица
```
ATE (
ate_id,     INT  INT NOT  SERIAL SERIAL  PRIMARY KEY
ate_name    varchar(50) NOT NULL
);
```
#### Таблица 8 Город
```
city (
city_id,     INT  NOT NULL  SERIAL  PRIMARY KEY
ate_name    varchar(50)  NOT NULL
);
```
#### Таблица 9 Улица
```
streat (
streat_id,     INT  NOT NULL  SERIAL  PRIMARY KEY
streat_name    varchar(50)  NOT NULL
);
```
#### Таблица 9 Адрес подразделения
```
address of the department (
address_department_id,     INT  NOT NULL  SERIAL  PRIMARY KEY
ate_id,                    INT  NOT NULL  integer 
city_id,                   INT  NOT NULL  integer  
streat_id                  INT  NOT NULL  integer 
house_number               VARCHAR(10)  NOT NULL

FOREIGN KEY (ate_id) REFERENCES ate(id),
FOREIGN KEY (city_id) REFERENCES city(id),
FOREIGN KEY (street_id) REFERENCES street(id)
);
```

#### Таблица 9 Проект
```
project (
project_id,     INT  NOT NULL  PRIMARY KEY
project_name    VARCHAR(100)  NOT NULL
);
```
---

### Задание 2

Перечислите, какие, на ваш взгляд, в этой денормализованной таблице встречаются функциональные зависимости и какие правила вывода нужно применить, чтобы нормализовать данные.

### Решение 2

Зависимость ключей (в отношении “Сотрудники”, ключ “Идентификатор сотрудника” функционально зависит от всех остальных атрибутов (имя, должность и т.д.))

Сотрудник - Проект

Сотрудник - Должность

Сотрудник - Оклад

Сотрудник - Тип подразделения

Сотрудник - Дата найма

Проект - Адресс 

Оклад зависит от Должности, Должность зависит от Отдела, т.е. Оклад зависит от Должности и Отдела одновременно - правило произведения

Сотрудник зависит от Должности и от Проекта, Проект зависит от Заказчика т.е. Сотрудник зависит от Должности и Заказчика - правое сокращение

Оклад и Должность зависят от Структурного подразделения, а Должность зависит от Проекта, т.е. Оклад зависит от Структурного подразделения и Проекта - левое сокращение.

![tables](img/tables.png)

---
