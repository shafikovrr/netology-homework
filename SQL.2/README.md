# Домашнее задание к занятию "`SQL. Часть 2`" - `Шафиков Ринат`

---

### Задание 1

`Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:`

- `фамилия и имя сотрудника из этого магазина;`
- `город нахождения магазина;`
- `количество пользователей, закреплённых в этом магазине.`

### Решение 1

```
SELECT CONCAT(s3.first_name, ' ', s3.last_name)  AS "Staff_name", c.city AS City, COUNT(c2.store_id) 
FROM staff s3 
INNER JOIN store s ON s.store_id = s3.store_id
INNER JOIN address a ON a.address_id = s3.address_id
INNER JOIN city c ON c.city_id  = a.city_id
INNER JOIN customer c2 ON c2.store_id = s3.store_id
GROUP BY s3.staff_id
HAVING COUNT(c2.store_id) > 300;
```

![INNER_JOIN_300](img/INNER_JOIN_300.png)

---

### Задание 2

`Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.`

### Решение 2

```
SELECT COUNT(f.film_id) AS 'Nuber'
FROM film f
WHERE f.`length` > (SELECT AVG(`length`) FROM film f2);
```

![SELECT_COUNT_BOL_AVG](img/SELECT_COUNT_BOL_AVG.png)

---

### Задание 3

`Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.`

### Решение 3

```
SELECT 
	SUM(p.amount) AS sum, 
	DATE_FORMAT(p.payment_date, '%Y-%m') AS pay_per_mon, 
	COUNT(r.rental_id) AS total_rent 
FROM payment p
INNER JOIN rental r ON p.rental_id = r.rental_id 
GROUP BY pay_per_mon
ORDER BY sum DESC
LIMIT 1;
```

![max_payment_per_month](img/max_payment_per_month.png)

---

### Задание 4

`Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».`


```

```

![*](img/*.png)

---

### Задание 5

`Найдите фильмы, которые ни разу не брали в аренду.`


```

```

![*](img/*.png)

---
