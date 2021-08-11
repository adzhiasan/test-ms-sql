# test-ms-sql
В базе данных есть таблицы продукты и категории.
```sql
CREATE TABLE products(
  id BIGINT NOT NULL PRIMARY KEY IDENTITY(1,1),
  name VARCHAR(30)
  )
  
CREATE TABLE categories(
  id BIGINT NOT NULL PRIMARY KEY IDENTITY(1,1),
  name VARCHAR(30)
  )
```
Одному продукту может соответствовать много категорий, в одной категории может быть много продуктов. Поэтому для связи многие ко многим создаём третью таблицу.
```sql
CREATE TABLE products_categories(
  id BIGINT NOT NULL PRIMARY KEY IDENTITY(1,1),
  product_id BIGINT FOREIGN KEY REFERENCES products(id),
  category_id BIGINT FOREIGN KEY REFERENCES categories(id)
  )
```
Заполняем таблицы данными.
```sql
INSERT INTO products VALUES ('milk')
INSERT INTO products VALUES ('butter')
INSERT INTO products VALUES ('cheese')
INSERT INTO products VALUES ('kefir')
INSERT INTO products VALUES ('yogurt')
INSERT INTO products VALUES ('chocolate')
INSERT INTO products VALUES ('biscuit')
INSERT INTO products VALUES ('candy')
INSERT INTO products VALUES ('meat')

INSERT INTO categories VALUES ('Milk products')
INSERT INTO categories VALUES ('Sweet')

INSERT INTO products_categories VALUES(1, 1)
INSERT INTO products_categories VALUES(2, 1)
INSERT INTO products_categories VALUES(3, 1)
INSERT INTO products_categories VALUES(4, 1)
INSERT INTO products_categories VALUES(5, 1)
INSERT INTO products_categories VALUES(5, 2)
INSERT INTO products_categories VALUES(6, 2)
INSERT INTO products_categories VALUES(7, 2)
INSERT INTO products_categories VALUES(8, 2)
```
*SQL*-запрос выбирает все пары «Имя продукта – Имя категории». Если у продукта нет категорий, то его имя все равно выводится. Решение реализовано с помощью <code>LEFT JOIN</code>.
```sql
SELECT p.name AS product_name, c.name AS category_name
FROM products AS p
LEFT JOIN products_categories as pc
ON p.id = pc.product_id
LEFT JOIN categories AS c
ON pc.category_id = c.id
```
