# Sql Task 
## create a database (ecommerce)

```sql 
create databse ecommerce;
use ecommerce;
```

# customers table.

```sql
create table customers(id int primary key auto_increment,name varchar(20),email varchar(20),address varchar(50));

desc customers;
````

# customers table Data.

``` sql
insert into customers values
(1,"raj","raj@gmail.com","coimbatore"),
(2,"pravin","pravin@gmail.com","salem"),
(3,"kumar","kumar@gmail.com","chennai");

select * from customers;
```

 # Output:

+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| id      | int         | NO   | PRI | NULL    | auto_increment |
| name    | varchar(20) | YES  |     | NULL    |                |
| email   | varchar(20) | YES  |     | NULL    |                |
| address | varchar(50) | YES  |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
+----+--------+------------------+------------+
| id | name   | email            | address    |
+----+--------+------------------+------------+
|  1 | raj    | raj@gmail.com    | coimbatore |
|  2 | pravin | pravin@gmail.com | salem      |
|  3 | kumar  | kumar@gmail.com  | chennai    |
+----+--------+------------------+------------+


# orders Table

```sql
create table  orders(id int primary key auto_increment,customer_id int references customers(id),order_date date ,total_amount int)	;

desc orders;
```

# order table Data
```sql
insert into orders values
(1,2,"2024-12-7",91),
(2,3,"2024-11-9",103),
(3,1,"2024-10-1",156);

select * from orders;
```

# Output: 

-----+-----+---------+----------------+
| Field        | Type | Null | Key | Default | Extra          |
+--------------+------+------+-----+---------+----------------+
| id           | int  | NO   | PRI | NULL    | auto_increment |
| customer_id  | int  | YES  |     | NULL    |                |
| order_date   | date | YES  |     | NULL    |                |
| total_amount | int  | YES  |     | NULL    |                |
+--------------+------+------+-----+---------+----------------+
+----+-------------+------------+--------------+
| id | customer_id | order_date | total_amount |
+----+-------------+------------+--------------+
|  1 |           2 | 2024-12-07 |           91 |
|  2 |           3 | 2024-11-09 |          103 |
|  3 |           1 | 2024-10-01 |          156 |
+----+-------------+------------+--------------+



# products Table

``` sql
create table products(id int primary key auto_increment,name varchar(20),price int,description varchar(50));

desc products;
```
# products table Data.

```sql
insert into products values
(1,"product A",199,"electronics"),
(2,"product B",299,"groceries"),
(3,"product c",399,"vegetables");

select * from products;
```

# Output:
-----+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int         | NO   | PRI | NULL    | auto_increment |
| name        | varchar(20) | YES  |     | NULL    |                |
| price       | int         | YES  |     | NULL    |                |
| description | varchar(50) | YES  |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+
+----+-----------+-------+-------------+
| id | name      | price | description |
+----+-----------+-------+-------------+
|  1 | product A |   199 | electronics |
|  2 | product B |   299 | groceries   |
|  3 | product c |   399 | vegetables  |
+----+-----------+-------+-------------+

# Queries

# 1. Retrieve all customers who have placed an order in the last 30 days.

```sql
select distinct customers.id, customers.name,customers.email,customers.address
from customers
join orders
on customers.id = orders.customer_id
where order_date >=curdate()- interval 30 day;
```
# output:
+----+--------+------------------+---------+
| id | name   | email            | address |
+----+--------+------------------+---------+
|  2 | pravin | pravin@gmail.com | salem   |
+----+--------+------------------+---------+


# 2. Get the total amount of all orders placed by each customer.

```sql
SELECT customers.id, customers.name, customers.email, customers.address, SUM(orders.total_amount) AS Total_amount
FROM customers
JOIN orders ON customers.id = orders.customer_id
GROUP BY customers.id;
```
# output:
+----+--------+------------------+------------+--------------+
| id | name   | email            | address    | Total_amount |
+----+--------+------------------+------------+--------------+
|  2 | pravin | pravin@gmail.com | salem      |           91 |
|  3 | kumar  | kumar@gmail.com  | chennai    |          103 |
|  1 | raj    | raj@gmail.com    | coimbatore |          156 |
+----+--------+------------------+------------+--------------+

# 3. Update the price of Product C to 45.00.

```sql
select * from products;
update products set price=45 where id=3;
select * from products;
```
# output:
+----+-----------+-------+-------------+
| id | name      | price | description |
+----+-----------+-------+-------------+
|  1 | product A |   199 | electronics |
|  2 | product B |   299 | groceries   |
|  3 | product c |   399 | vegetables  |
+----+-----------+-------+-------------+
+----+-----------+-------+-------------+
| id | name      | price | description |
+----+-----------+-------+-------------+
|  1 | product A |   199 | electronics |
|  2 | product B |   299 | groceries   |
|  3 | product c |    45 | vegetables  |
+----+-----------+-------+-------------+


# 4. Add a new column discount to the products table.

```sql
alter table products add column discount int;
select * from products;
```
# output:
+----+-----------+-------+-------------+----------+
| id | name      | price | description | discount |
+----+-----------+-------+-------------+----------+
|  1 | product A |   199 | electronics |     NULL |
|  2 | product B |   299 | groceries   |     NULL |
|  3 | product c |   399 | vegetables  |     NULL |

# 5. Retrieve the top 3 products with the highest price.

```sql
select * from products;
select * from products order by price desc limit 3;
```
# output:
+----+-----------+-------+-------------+
| id | name      | price | description |
+----+-----------+-------+-------------+
|  1 | product A |   199 | electronics |
|  2 | product B |   299 | groceries   |
|  3 | product c |   399 | vegetables  |
+----+-----------+-------+-------------+
+----+-----------+-------+-------------+
| id | name      | price | description |
+----+-----------+-------+-------------+
|  3 | product c |   399 | vegetables  |
|  2 | product B |   299 | groceries   |
|  1 | product A |   199 | electronics |
+----+-----------+-------+-------------+



# 6.Get the names of customers who have ordered Product A.

```sql
-- table list
select * from customers;
select * from products;
select * from orders;


SELECT DISTINCT customers.name
FROM customers
JOIN orders ON customers.id = orders.customer_id
JOIN order_items ON orders.id = order_items.order_id
JOIN products ON products.id = order_items.product_id
WHERE products.name = 'Product a';
```
# output:
+----+--------+------------------+------------+
| id | name   | email            | address    |
+----+--------+------------------+------------+
|  1 | raj    | raj@gmail.com    | coimbatore |
|  2 | pravin | pravin@gmail.com | salem      |
|  3 | kumar  | kumar@gmail.com  | chennai    |
+----+--------+------------------+------------+
+----+-----------+-------+-------------+
| id | name      | price | description |
+----+-----------+-------+-------------+
|  1 | product A |   199 | electronics |
|  2 | product B |   299 | groceries   |
|  3 | product c |   399 | vegetables  |
+----+-----------+-------+-------------+
+----+-------------+------------+--------------+
| id | customer_id | order_date | total_amount |
+----+-------------+------------+--------------+
|  1 |           2 | 2024-12-07 |           91 |
|  2 |           3 | 2024-11-09 |          103 |
|  3 |           1 | 2024-10-01 |          156 |

# 7.Join the orders and customers tables to retrieve the customer's name and order date for each order.

```sql
 select orders.id as order_id ,customers.name as customer_Name,orders.order_date
   from orders
   join customers
   ON
   customers.id = orders.customer_id;

   ```
# output:
+----------+---------------+------------+
| order_id | customer_Name | order_date |
+----------+---------------+------------+
|        1 | pravin        | 2024-12-07 |
|        2 | kumar         | 2024-11-09 |
|        3 | raj           | 2024-10-01 |
+----------+---------------+------------+

 # 8.Retrieve the orders with a total amount greater than 150.00.

 ```sql
select * from orders
where total_amount >150;
 ```
 # output:
 +----+-------------+------------+--------------+
| id | customer_id | order_date | total_amount |
+----+-------------+------------+--------------+
|  3 |           1 | 2024-10-01 |          156 |
+----+-------------+------------+--------------+

# 9.Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.

   ```sql
   CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    foreign key (order_id) references orders(id),
    foreign key (product_id) references products(id)
);
desc order_items;
select * from order_items;
```
# output:
+------------+------+------+-----+---------+----------------+
| Field      | Type | Null | Key | Default | Extra          |
+------------+------+------+-----+---------+----------------+
| id         | int  | NO   | PRI | NULL    | auto_increment |
| order_id   | int  | YES  | MUL | NULL    |                |
| product_id | int  | YES  | MUL | NULL    |                |
+------------+------+------+-----+---------+----------------+


 # 10.Retrieve the average total of all orders.

   ```sql
   SELECT AVG(total_amount) as average_total
FROM orders;
```

# output
+---------------+
| average_total |
+---------------+
|      116.6667 |
+---------------+




