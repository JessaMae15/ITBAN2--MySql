## By E. Longao. & J. Roxas.

### 1. Retrieve Product Information:

* Write a query to fetch the names and descriptions of all products.

```sql
SELECT name, description
FROM products
```
<img width="670" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/964c1702-53a1-4eff-aaee-680f8471a6b2">

* Extend the previous query to include specific attributes such as color, size, and price.
```sql
SELECT name, description,
       JSON_EXTRACT(attributes, '$.color') AS color,
       JSON_EXTRACT(attributes, '$.size') AS size,
       JSON_EXTRACT(attributes, '$.price') AS price,
JSON_EXTRACT(attributes, '$.brand') AS brand
FROM products;
```
<img width="772" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/c67a22ae-0c27-489b-ba17-84335f09e103">


### 2. Query Orders and Order Details:
* Retrieve the details of all orders placed, including the order date, customer ID, product name, quantity, and price.

```sql
SELECT
    o.order_id,
    o.order_date,
    o.customer_id,
    p.name AS product_name,
    od.quantity,
    od.price
FROM
    orders o JOIN
    order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id;
```
<img width="756" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/1f4aa725-5396-418f-85e7-dc00d73a4ba5">


* Calculate the total cost of each order.

```sql
SELECT
    o.order_id,
    SUM(od.quantity * od.price) AS total_cost
FROM
    orders o
JOIN
    order_details od ON o.order_id = od.order_id
GROUP BY
    o.order_id;
```
<img width="762" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/fc882612-5e5e-4d12-90ed-6b5421f55233">


 ### 3.Filtering Products Based on Attributes:

 * Write a query to find all products with a price greater than $50.

   ```sql
   SELECT * FROM products
   WHERE JSON_EXTRACT(attributes, '$.price') > 50;
   ```
<img width="759" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/4c01f3e0-c82c-4eb0-b724-22d8b208af5a">

* Filter products by color and brand, and display their names and prices.
```sql
SELECT name, JSON_EXTRACT(attributes, '$.price') AS price
FROM products
WHERE JSON_EXTRACT(attributes, '$.color') = 'teal' 
AND JSON_EXTRACT(attributes, '$.brand') = 'Chanel';

```
<img width="719" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/608edbd3-db65-4e60-8803-fcf606e5a878">


### 4. Calculating Aggregate Data:

* Calculate the total sales revenue generated by each product.
```sql
SELECT 
    p.product_id,
    p.name AS product_name,
    SUM(od.quantity * od.price) AS total_revenue
FROM 
    products p
JOIN 
    order_details od ON p.product_id = od.product_id
GROUP BY 
    p.product_id, p.name;
```
<img width="747" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/22dec27f-0747-4608-a38c-1f51783dfb49">


* Determine the total quantity of each product ordered.
* 
  ```sql
  SELECT 
    p.product_id,
    p.name AS product_name,
    SUM(od.quantity) AS total_quantity
  FROM 
    products p
  JOIN 
    order_details od ON p.product_id = od.product_id
  GROUP BY 
    p.product_id, p.name;
  ```
<img width="757" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/7eff57a2-39ca-435e-8ff5-353731197407">

### 5. Advanced Filtering and Aggregation:

* Find the top 5 best-selling products based on total quantity sold.

```sql
SELECT
    p.product_id,
    p.name AS product_name,
    SUM(od.quantity) AS total_quantity_sold
FROM
    products p
JOIN
    order_details od ON p.product_id = od.product_id
GROUP BY
    p.product_id, p.name
ORDER BY
    total_quantity_sold DESC Limit 5;
```
<img width="773" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/f406ce8b-d21e-44f0-8b8d-c8649248ac72">

* Identify the average price of products from a specific brand.
 ```sql
SELECT
    AVG(JSON_EXTRACT(attributes, '$.price')) AS average_price,
    JSON_EXTRACT(attributes, '$.brand') AS brand
FROM
    products
WHERE
    JSON_EXTRACT(attributes, '$.brand') = 'Zara'
GROUP BY
    JSON_EXTRACT(attributes, '$.brand');

```
<img width="743" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/93d2c967-46a4-46a2-a8d2-e9fb4181f63c">

### 6. Nested JSON Queries:

* Retrieve the color and size of a specific product.
```sql
SELECT
    JSON_EXTRACT(attributes, '$.color') AS color,
    JSON_EXTRACT(attributes, '$.size') AS size
FROM
    products
WHERE
    product_id = 1234;
```
<img width="750" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/a43a1cf8-e3bb-42d4-9b35-fbc6a69882f4">

* Extract and display all available attributes of products in JSON format.
```sql
SELECT JSON_OBJECT(
    'product_id', product_id,
    'name', name,
    'description', description,
    'attributes', attributes
) AS product_info
FROM products;
```
<img width="772" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/20e63dc7-15c2-4c25-a6e2-a395cd89a07d">

### 7. Joining Multiple Tables:
* Write a query to find all orders placed by customers along with the products ordered and their quantities.
```sql
SELECT 
    o.order_id,
    o.order_date,
    c.customer_id,
    p.name AS product_name,
    od.quantity
FROM 
    orders o
JOIN 
    order_details od ON o.order_id = od.order_id
JOIN 
    products p ON od.product_id = p.product_id
JOIN 
    customers c ON o.customer_id = c.customer_id;
```
<img width="753" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/a7fa4a5a-8ca8-45b6-88b9-b4b9bcb4153d">

* Calculate the total revenue generated by each customer.

  ```sql
  SELECT
    CONCAT(c.firstname, ' ', COALESCE(c.middlename, ''), ' ', c.lastname) AS full_name,
    c.customer_id,
    SUM(od.quantity * JSON_EXTRACT(p.attributes, '$.price')) AS total_revenue
  FROM
    customers c
  JOIN
    orders o ON c.customer_id = o.customer_id
  JOIN
    order_details od ON o.order_id = od.order_id
  JOIN
    products p ON od.product_id = p.product_id
  GROUP BY
    c.customer_id, full_name;
  ```
<img width="754" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/acae8b40-9fc7-4119-9b56-a0776d52379b">

### 8. Data Manipulation with JSON Functions:
* Update the price of a specific product stored as JSON attribute.
**The original price of the product id 120.**

```sql
SELECT product_id,
JSON_EXTRACT(attributes, '$.price')
AS price
FROM products
WHERE product_id=120;
```
<img width="701" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/20bb2acb-c71c-4887-8350-0936f2fe2bdc">

**The query to update the specific price of the product id 120.**
```sql
UPDATE products
SET attributes = JSON_SET(attributes, '$.price', 500.00)
WHERE product_id = 120;
```
<img width="625" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/c01a7117-45dc-44ea-9d82-22eae828beda">

**Updated Price of the product id number 120.**
```sql
SELECT product_id, JSON_EXTRACT(attributes, '$.price') AS price
FROM products
WHERE product_id=120;
```
<img width="602" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/5c7ced1f-8633-49e0-b897-f6be38b4f5d0">

* Add a new attribute to all products with a default value.
  ```sql
  ALTER TABLE products ADD COLUMN weight VARCHAR(255) DEFAULT 'default_value';
  ```
  <img width="514" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/3d8df3e7-96eb-474c-84b5-851c58318448">


```sql
SELECT name, description,
JSON_EXTRACT(attributes, '$.color') AS color,
JSON_EXTRACT(attributes, '$.size') AS size,
JSON_EXTRACT(attributes, '$.price') AS price,
JSON_EXTRACT(attributes, '$.brand') AS brand,
JSON_EXTRACT(attributes, '$.weight') AS weight
FROM products;
```
<img width="764" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/b9090422-a43e-47f0-bead-e18cb3b44be1">


### 9. Advanced JSON Operations:
* Find products with specific attributes that match a given criteria using JSON path expressions.
```sql
SELECT *
FROM products
WHERE JSON_EXTRACT(attributes, '$.color') = 'cyan'
AND JSON_EXTRACT(attributes, '$.size') = 'Large';
```
<img width="774" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/3d1a19f8-f3a8-4137-b532-6a88e1dfff5d">

* Extract and display the first element of an array stored within a JSON attribute.

  ```sql
  SELECT JSON_UNQUOTE(JSON_EXTRACT(attributes,'$.color'))
  AS first_element FROM `products`;
  ```
<img width="663" alt="image" src="https://github.com/JessaMae15/ITBAN2--MySql/assets/125847496/3dd5bb1c-378f-495b-85f5-5666790e0199">



### THANK YOU!!!























   




