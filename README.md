### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Решение

Текст запроса:
```
SELECT CONCAT(s.first_name, ' ', s.last_name) AS manager, c.city AS city, COUNT(c2.customer_id) AS customers
FROM staff s
INNER JOIN store s2 ON s2.manager_staff_id = s.staff_id 
INNER JOIN customer c2 ON c2.store_id = s2.store_id 
INNER JOIN address a ON a.address_id = s2.address_id 
INNER JOIN city c ON c.city_id = a.city_id
GROUP BY s2.store_id
HAVING customers  > 300
```

Скриншот из DBeaver:
![alt text](https://github.com/masterchoo495/SQL-2/blob/main/001.png)

---

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение

Текст запроса:
```
SELECT COUNT(film_id)
FROM film
WHERE length > (SELECT AVG(length) FROM film)
```

Скриншот из DBeaver:
![alt text](https://github.com/masterchoo495/SQL-2/blob/main/002.png)

---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Решение

Текст запроса:
```
SELECT DATE_FORMAT(payment_date, '%M %Y'), SUM(amount), COUNT(rental_id)
FROM payment
GROUP BY DATE_FORMAT(payment_date, '%M %Y')
ORDER BY SUM(amount) DESC 
LIMIT 1
```

Скриншот из DBeaver:
![alt text](https://github.com/masterchoo495/SQL-2/blob/main/003.png)

---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

### Решение

Текст запроса:
```
SELECT CONCAT(s.first_name, ' ', s.last_name) as sailer, COUNT(rental_id) AS sales_number,
	CASE
		WHEN COUNT(rental_id) > 8000 THEN 'Yes'
		ELSE 'No'
	END AS bonus
FROM staff s
INNER JOIN payment p ON p.staff_id = s.staff_id 
GROUP BY sailer
```

Скриншот из DBeaver:
![alt text](https://github.com/masterchoo495/SQL-2/blob/main/004.png)

---

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

### Решение

Текст запроса:
```
SELECT f.title AS kino 
FROM film f
LEFT JOIN inventory i ON i.film_id = f.film_id
LEFT JOIN rental r ON r.inventory_id = i.inventory_id
WHERE r.rental_id IS NULL
```

Скриншот из DBeaver:
![alt text](https://github.com/masterchoo495/SQL-2/blob/main/005.png)
