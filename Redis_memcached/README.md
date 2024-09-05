### Домашнее задание к занятию "`Кеширование Redis/memcached`" - `Шафиков Ринат`  

#### Задание 1. Кеширование

Приведите примеры проблем, которые может решить кеширование.

Приведите ответ в свободной форме.

#### Решение 1

Долгий доступ к наиболее часто используемым данным. Увеличение времени простоя оборудования на поиск данных.

---

#### Задание 2. Memcached

Установите и запустите memcached.

Приведите скриншот systemctl status memcached, где будет видно, что memcached запущен.

### Решение 2
Установка memcached на сервер:
```
sudo apt update && sudo apt upgrade
```
```
sudo apt install memcached
```
```
sudo systemctl status memceched
```
![Статус memcached](https://github.com/shafikovrr/Redis_memcached/blob/main/img/status_memcached.png)

---

### Задание 3. Удаление по TTL в Memcached

Запишите в memcached несколько ключей с любыми именами и значениями, для которых выставлен TTL 5.

Приведите скриншот, на котором видно, что спустя 5 секунд ключи удалились из базы.


### Решение 3

```
telnet localhost 11211
```
```
add key 0 5 1
```
```
get key
```

![del ttl](https://github.com/shafikovrr/Redis_memcached/blob/main/img/del_ttl.png)

---

### Задание 4. Запись данных в Redis

Запишите в Redis несколько ключей с любыми именами и значениями.

Через redis-cli достаньте все записанные ключи и значения из базы, приведите скриншот этой операции.


### Решение 4

```
sudo apt update && sudo apt upgrade
```
```
sudo apt install redis
```
```
set name rinat
set age 42
set city ufa
```
```
keys *
```
```
get city
get age
get name
```

![redis_add_view](https://github.com/shafikovrr/Redis_memcached/blob/main/img/redis_add_view.png)

---

### Задание 5*. Работа с числами

Запишите в Redis ключ key5 со значением типа "int" равным числу 5. Увеличьте его на 5, чтобы в итоге в значении лежало число 10.

Приведите скриншот, где будут проделаны все операции и будет видно, что значение key5 стало равно 10.

### Решение 5

Выбор логической базы данных с индексом 10
```
select 10
```
Создание ячейки key5 со значением 5
```
set key5 5
```
Вывод списка ключей
```
keys *
```
Вывод значения ключа key5
```
get key5
```
Увеличение значения ключа key5 на 5
```
incrby key5 5
```
Вывод значения ключа key5
```
get key5
```

![redis_incrby](https://github.com/shafikovrr/Redis_memcached/blob/main/img/redis_incrby.png)