### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp. 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp. 

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

### Решение
![Screenshot_20250226_021819](https://github.com/user-attachments/assets/83c309a1-a4c4-4c77-bfbf-c38af9c2e68c)
![image](https://github.com/user-attachments/assets/f6c3f79c-91f3-4474-ab7d-6974c7626160)
![Screenshot_20250226_023657](https://github.com/user-attachments/assets/2db2ee91-a964-420d-a1aa-8db21e30ad0d)
![Screenshot_20250226_024636](https://github.com/user-attachments/assets/5a834aa4-b32a-4447-b5a3-abb764e35913)
![image](https://github.com/user-attachments/assets/bf3fbb1e-e084-4a0d-b776-9d6be9f527c3)
```sql
-- 1.2. Создайте учётную запись sys_temp
CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';

-- 1.3. Выполните запрос на получение списка пользователей в базе данных
SELECT * FROM mysql.user;

-- 1.4. Дайте все права для пользователя sys_temp
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';
FLUSH PRIVILEGES;

-- 1.5. Выполните запрос на получение списка прав для пользователя sys_temp
SHOW GRANTS FOR 'sys_temp'@'localhost';

-- 1.6. Переподключитесь к базе данных от имени sys_temp
ALTER USER 'sys_temp'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';

-- Честно пробовал сменить пользователя "не отходя от базы"
CHANGE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';
ALTER USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';

SELECT VERSION();
SELECT CURRENT_USER();
```

### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```
### Решение



### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*
### Решение
