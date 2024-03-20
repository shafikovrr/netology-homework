# Домашнее задание к занятию "`Работа с данными (DDL/DML)`" - `Шафиков Ринат`

### Задание 1

`Приведите ответ в свободной форме........`

1. `Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.`
2. `Создайте учётную запись sys_temp.`
3. `Выполните запрос на получение списка пользователей в базе данных. (скриншот)`
4. `Дайте все права для пользователя sys_temp.`
5. `Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)`
6. `Переподключитесь к базе данных от имени sys_temp.`
   
Для смены типа аутентификации с sha2 используйте запрос:

```
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

7. `По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.`
8. `Восстановите дамп в базу данных.`
9. `При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)`

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

### Решение 1

1. `Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.`

Установка mysql
```
sudo apt update && sudo apt upgrade -y
sudo apt install mysql-server
sudo service mysql status
```
Если не пускает под рутом
```
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
exit
```

Настройка безопасности

```
sudo mysql_secure_installation
```
```
VALIDATE PASSWORD COMPONENT Press y|Y for Yes, any other key for No: y
Change the password for root ? ((Press y|Y for Yes, any other key for No) : n
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
```

2. `Создайте учётную запись sys_temp.`

Создаем пользователя sys_temp (с правом доступа только с localhost, иначе 'sys_temp'@'%')

```
mysql -u root -p
CREATE USER 'sys_temp'@'localhost' identified by 'password';
```

3. `Выполните запрос на получение списка пользователей в базе данных. (скриншот)`

Вывод списка пользователей

```
select user from mysql.user;
```

![mysql_users](img/mysql_users.png)

4. `Дайте все права для пользователя sys_temp.`

Даем все права пользователю sys_temp

```
grant all privileges on *.* to 'sys_temp'@'localhost' with grant option;
flush privileges;
```
опция with grant option - дает возможность давать права другим пользователям.


5. `Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)`

```
SHOW GRANTS FOR 'sys_temp'@'localhost';
exit
```

![show_grants](img/show_grants.png)

6. `Переподключитесь к базе данных от имени sys_temp.`

```
mysql -u sys_temp -p
exit
```
Сменим тип аутентификации с sha2 используя запрос:

```
mysql -u root -p
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
exit
```

7. `По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.`

```
wget https://downloads.mysql.com/docs/sakila-db.zip
```
8. `Восстановите дамп в базу данных.`

```
sudo apt install unzip
unzip sakila-db.zip
mysql -p -h 127.0.0.1 -P 3306 -u sys_temp -p < sakila-db/sakila-schema.sql
mysql -p -h 127.0.0.1 -P 3306 -u sys_temp -p < sakila-db/sakila-data.sql
```

9. `При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)`

```
mysql -u sys_temp -p
show databases;
use sakila
show tables;
```

![show_sakila_tables](img/show_sakila_tables.png)`

---

### Задание 2

1. `Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)`

```
Название таблицы | Название первичного ключа
customer         | customer_id
```



`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

### Задание 3

1. `Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.`
2. `Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)`

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.`

### Решение 3



`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
