mysql -p -h 127.0.0.1 -P 13306 -u root < sakila-db/sakila-schema.sql
mysql -p -h 127.0.0.1 -P 13306 -u root < sakila-db/sakila-data.sql


### Master_1

mysql -u root -p

CREATE USER 'repl'@'%' IDENTIFIED BY 'masterpass';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
SHOW GRANTS FOR repln@'%';

SHOW MASTER STATUS;

CHANGE MASTER TO
SOURCE_HOST='mysql-master_2',
SOURCE_USER='repl',
SOURCE_PASSWORD='masterpass',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=660;

START SLAVE;

SHOW SLAVE STATUS\G;

CREATE DATABASE master_master;

SHOW DATABASES;

SHOW MASTER STATUS;

use master_master;

show tables;

describe table;

### Master_2

mysql -u root -p

CREATE USER 'repl'@'%' IDENTIFIED BY 'masterpass';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
SHOW GRANTS FOR repl@'%';
SHOW MASTER STATUS;

CHANGE MASTER TO
SOURCE_HOST='mysql-master_1',
SOURCE_USER='repl',
SOURCE_PASSWORD='masterpass',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=660;

START SLAVE;

SHOW SLAVE STATUS\G;

SHOW DATABASES;

SHOW MASTER STATUS;

use master_master;

create table table (id INT, fio VARCHAR(100));

show tables;
