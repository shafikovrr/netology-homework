mysql -p -h 127.0.0.1 -P 13306 -u root < sakila-db/sakila-schema.sql
mysql -p -h 127.0.0.1 -P 13306 -u root < sakila-db/sakila-data.sql


### Master

mysql -u root -p

CREATE USER 'repl'@'%' IDENTIFIED WITH mysql_native_password BY 'slaverepl';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
SHOW GRANTS FOR repl@'%';
SHOW MASTER STATUS;

### Slave

CHANGE MASTER TO
SOURCE_HOST='mysql-master',
SOURCE_USER='repl',
SOURCE_PASSWORD='slaverepl',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=660;

START SLAVE;
SHOW SLAVE STATUS\G;
SHOW DATABASE;