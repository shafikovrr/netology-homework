# Домашнее задание к занятию "`Репликация и масштабирование. Часть 1`" - `Шафиков Ринат`

---

### Задание 1

`На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.
Ответить в свободной форме.`

### Решение 1

```

```

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
SHOW MASTER STATUS
```

![show_master_status](img/show_master_status_2.png)


На слейве

```
SHOW SLAVE STATUS\G;
```

![show_status_slave_2](img/show_slave_status_4.png)

---

show_status_slave_2
