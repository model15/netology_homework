### Задание 1

Получите уникальные названия районов из таблицы с адресами, 
которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
### Решение

```
SELECT * FROM address;

SELECT
	DISTINCT district
FROM
	address
WHERE
	district LIKE 'K%a'
	and (POSITION(' ' IN district) = 0);

SELECT
	DISTINCT district
FROM
	address
WHERE
	district LIKE 'K%a'
	and district NOT LIKE '% %';

```
---

### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, 
которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** 
и стоимость которых превышает 10.00.
### Решение

```
SELECT * FROM payment;

SELECT
	*
FROM
	payment
WHERE
	(payment_date BETWEEN '2005-06-15' AND '2005-06-19')
	AND (amount > 10.00) ;
```
---


### Задание 3

Получите последние пять аренд фильмов.
### Решение

```
SELECT * FROM rental;

SELECT
	*
FROM
	rental
ORDER BY
	rental.rental_date DESC
LIMIT 5;
```
---

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.
### Решение

```
SELECT * FROM sakila.customer;

SELECT
	REPLACE(LOWER(CONCAT_WS(' ', customer.first_name, customer.last_name)), 'll', 'pp') AS 'Customer full name'
FROM
	sakila.customer
WHERE
	first_name = 'Kelly'
	or first_name = 'Willie';
```
---

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: 
в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.
### Решение

```
SELECT
	SUBSTR( email, 1 , POSITION('@' IN email) -1) AS 'before',
	SUBSTR( email, POSITION('@' IN email) + 1 ) AS 'after'
FROM
	sakila.customer;

SELECT
	SUBSTRING_INDEX(email , '@', 1) AS 'before',
	SUBSTRING_INDEX(email , '@', -1) AS 'after'
FROM
	sakila.customer;
```
---

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: 
первая буква должна быть заглавной, остальные — строчными.
### Решение

```
SELECT
	CONCAT(UPPER( SUBSTRING(SUBSTRING_INDEX(email , '@', 1), 1, 1 ) ), LOWER( SUBSTRING(SUBSTRING_INDEX(email , '@', 1), 2))) AS 'before',
	CONCAT(UPPER( SUBSTRING(SUBSTRING_INDEX(email , '@', -1), 1, 1 ) ), LOWER( SUBSTRING(SUBSTRING_INDEX(email , '@', -1), 2))) AS 'before'
FROM
	sakila.customer;
```



