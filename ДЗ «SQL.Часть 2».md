### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;c
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.
### Решение
```sql

SELECT
	CONCAT_WS(' ', staff.first_name, staff.last_name) AS staff,
	city.city,
	COUNT(customer.customer_id) customers ,
	store.store_id
FROM
	store
LEFT JOIN staff ON
	store.store_id = staff.store_id
LEFT JOIN address ON
	store.address_id = address.address_id
LEFT JOIN city ON
	address.city_id = city.city_id
RIGHT JOIN customer ON
	store.store_id = customer.store_id
GROUP BY
	store.store_id,
	staff.first_name,
	staff.last_name
HAVING
	customers > 300;
```
---
### Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
### Решение
```sql
SELECT
	COUNT(*)
FROM
	film
WHERE
	film.length > (
	SELECT
		AVG(film.length)
	FROM
		film);
```
---

### Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
### Решение
```sql
SELECT
	DATE_FORMAT(payment_date, '%y-%m') AS 'year_month',
	SUM(amount) AS total_payment,
	COUNT(rental_id) AS rentals
FROM
	payment
GROUP BY
	DATE_FORMAT(payment_date, '%y-%m')
ORDER BY
	total_payment DESC
LIMIT 1;
```
---

### Задание 4*
Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».
### Решение
```sql
SELECT
	staff.staff_id,
	CONCAT_WS(' ', staff.first_name, staff.last_name) staff_name,
	COUNT(payment.payment_id) sales,
	CASE
		WHEN COUNT(payment.payment_id) > 8000 THEN 'Да'
		ELSE 'Нет'
	END AS bonus
FROM
	staff
RIGHT JOIN payment ON
	staff.staff_id = payment.staff_id
GROUP BY
	staff.staff_id,
	staff.first_name,
	staff.last_name;c
```
---

### Задание 5*
Найдите фильмы, которые ни разу не брали в аренду.
### Решение
```sql
SELECT
	film.title,
	film.film_id,
	rental.inventory_id,
	rental.rental_id
FROM
	film
LEFT JOIN inventory ON
	film.film_id = inventory.film_id
LEFT JOIN rental ON
	inventory.inventory_id = rental.inventory_id
WHERE
	rental.rental_id IS NULL;
```
