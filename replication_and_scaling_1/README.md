# Домашнее задание к занятию "`Репликация и масштабирование. Часть 1`" - `Шафиков Ринат`

---

### Задание 1

`На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.
Ответить в свободной форме.`

### Решение 1

`Master-slave: создаются минимум два сервера (Master - на нем производятся добавление, обновление и удаление данных. Slave - копирует данные с Master-сервера. Данные с него можно только читать). Master-Master - создаются как минимум два сервера. Оба сервера доступны на добавление, обновление и удаление данных. Между ними происходит синхронизация. Возможна балансировка нагрузки на данные сервера`

---

### Задание 2

`Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.
Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.`

### Решение 2

На мастере

```
mysql -u root -p
CREATE USER 'repl'@'%' IDENTIFIED WITH mysql_native_password BY 'slaverepl';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
SHOW GRANTS FOR repl@'%';
SHOW MASTER STATUS;
```

![show_master_status](img/show_master_status.png)

```
SHOW DATABASES;
```
![show_databases_master](img/show_databases_master.png)

На слейве

```
mysql -u root -p
```
```
CHANGE MASTER TO
SOURCE_HOST='mysql-master',
SOURCE_USER='repl',
SOURCE_PASSWORD='slaverepl',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=660;
```
```
START SLAVE;
SHOW SLAVE STATUS\G;
```
![show_slave_status_1](img/show_slave_status_1.png)

![show_slave_status_2](img/show_slave_status_2.png)

![show_slave_status_3](img/show_slave_status_3.png)

```
SHOW DATABASES;
```

![show_databases_slave](img/show_databases_slave.png)


Создадим на мастере тестовую таблицу

```
CREATE DATABASE netology_shafikov;
SHOW DATABASES;

```
![create_show_netology_shafikov](img/create_show_netology_shafikov.png)

На слейве

```
SHOW DATABASES;
```

![show_slave_netology_shafikov](img/show_slave_netology_shafikov.png)

На мастере

```
SHOW MASTER STATUS;
```

![show_master_status](img/show_master_status_2.png)


На слейве

```
SHOW SLAVE STATUS\G;
```

![show_status_slave_2](img/show_slave_status_4.png)

---

### Задание 3

`Выполните конфигурацию master-master репликации. Произведите проверку.
Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.`

### Решение 3

На мастере 1

```
mysql -u root -p
CREATE USER 'repl'@'%' IDENTIFIED BY 'masterpass';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
SHOW GRANTS FOR repl@'%';
SHOW MASTER STATUS;
```

На мастере 2

```
mysql -u root -p
CREATE USER 'repl'@'%' IDENTIFIED BY 'masterpass';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
SHOW GRANTS FOR repl@'%';
SHOW MASTER STATUS;
```

```
CHANGE MASTER TO
SOURCE_HOST='mysql-master_1',
SOURCE_USER='repl',
SOURCE_PASSWORD='masterpass',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=660;
```

```
START SLAVE;
SHOW SLAVE STATUS\G;
```

![master_2_show_status](img/master_2_show_status.png)

На мастере 1

```
CHANGE MASTER TO
SOURCE_HOST='mysql-master_2',
SOURCE_USER='repl',
SOURCE_PASSWORD='masterpass',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=660;
```
```
START SLAVE;
SHOW SLAVE STATUS\G;
```

![master_1_show_status](img/master_1_show_status.png)

На мастере 1 создадим новую таблицу.

```
CREATE DATABASE netology_shafikov_master_master;
SHOW DATABASES;
```

![master_1_create_new_database](img/master_1_create_new_database.png)

На мастере 2 появилась созданная на мастере 1 новая база данных

```
SHOW DATABASES;
```

![master_2_show_new_database](img/master_2_show_new_database.png)


На мастере 2 создадим в новой базе данных таблицу

```
use netology_shafikov_master_master;
create table shafikov_table (id INT, fio VARCHAR(100));
show tables;
```

![master_2_create_new_table](img/master_2_create_new_table.png)


На мастере 1 база данных обновилась

```
use netology_shafikov_master_master;
show tables;
describe shafikov_table;
```

![master_1_show_new_table_describe](img/master_1_show_new_table_describe.png)

---
