# andmebaasid sales

```sql
--1.categories
create table categories(
category_id int PRIMARY KEY identity(1,1),
category_name varchar(25) UNIQUE);

INSERT INTO categories(category_name)
VALUES ('Arvuti');
```
<img width="195" height="84" alt="{9C9A6F6D-A56C-44BB-8E5E-3A2BB4BF43C8}" src="https://github.com/user-attachments/assets/95d54245-59ad-4711-8c02-c33400f7b545" />


```sql
CREATE TABLE brands(
brand_id int PRIMARY KEY identity(1,1),
brand_name varchar(15) UNIQUE);

INSERT INTO brands(brand_name)
VALUES ('Samsung');
```
<img width="172" height="82" alt="{DD7F7A3F-A6E3-4BFE-8FA4-DEAF5B089B38}" src="https://github.com/user-attachments/assets/04a686c9-b5c0-44da-93f3-3c5c63ba5603" />

```sql
Create TABLE products(
product_id int PRIMARY KEY identity(1,1),
product_name varchar(50) not null,
brand_id int,
FOREIGN KEY (brand_id) references  brands(brand_id),
category_id int,
FOREIGN KEY (category_id) references categories(category_id),
model_year int,
list_price money);
```
<img width="474" height="117" alt="{8CC8FB45-9D09-4B61-9106-925F7CFFC7AD}" src="https://github.com/user-attachments/assets/9a1fe327-2c61-4fef-b924-54e4192246fa" />

```sql
CREATE TABLE stores(
store_id int PRIMARY KEY identity (1,1),
store_name varchar(20) not null,
phone varchar(13),
email varchar(40),
street varchar(20),
city varchar(10),
state varchar(10),
zip_code char(5));

SELECT * FROM stores;

INSERT INTO stores
VALUES ('Kristiine','56475824', 'kristiine@gmail.com', 'Endla', 'Tallinn', 'Harjumaa','13425');
```
<img width="544" height="99" alt="{8210B3B6-FD18-48D2-8BA1-BF00317F7109}" src="https://github.com/user-attachments/assets/2fe019f3-f97d-4027-8c60-eba491b1cbac" />

```sql
CREATE TABLE stocks(
store_id int,
product_id int,
PRIMARY KEY(store_id, product_id),
FOREIGN KEY (store_id) references stores(store_id),
FOREIGN KEY (product_id) references products(product_id),
quantity int);
```
<img width="215" height="89" alt="{38FAA47E-DD35-4FB9-A0FE-93B1C17F90FD}" src="https://github.com/user-attachments/assets/8a2a2f69-5d27-48fc-b6da-b6902b93c77c" />


```sql
CREATE TABLE customers(
customer_id int PRIMARY KEY identity (1,1),
first_name varchar(20) not null,
last_name varchar(20) not null,
phone varchar(10),
email varchar(30),
street varchar(30),
city varchar(15) check (city='Tallinn' or city='Narva'),
state varchar(15),
zip_code char(5));

SELECT * FROM customers;

INSERT INTO customers
VALUES ('Oleg','Uustal','56574857','uustal@gmail.com', 'Läänemere tee','Narva','Ida-Virumaa','13457');
```
<img width="548" height="113" alt="{679162A8-2FA7-4E6F-AACC-2E01C76491F5}" src="https://github.com/user-attachments/assets/40a55a11-c45f-4d59-8df8-1c5967fb52e0" />

```sql
CREATE TABLE staffs(
staff_id int PRIMARY KEY identity (1,1),
first_name varchar(20) not null,
last_name varchar(20) not null,
phone varchar(10),
active bit,
store_id int,
FOREIGN KEY (store_id) references stores(store_id),
manager bit);
SELECT * FROM staffs;
INSERT INTO staffs
VALUES ('Kirill','Leht', '56245878', 0,1,0);
```
<img width="439" height="115" alt="{CD826E1D-A593-43C8-8F4C-22CD153F53AC}" src="https://github.com/user-attachments/assets/20f14492-d348-419d-9739-e9acf90f7231" />

```sql
CREATE TABLE orders(
order_id int PRIMARY KEY identity (1,1),
customer_id int,
foreign key (customer_id) references customers(customer_id),
order_status varchar(15) check (order_status='complete' or order_status='incomplete'),
order_date date,
required_date date,
shipped_date date,
store_id int,
FOREIGN KEY (store_id) references stores(store_id),
staff_id int,
FOREIGN KEY (staff_id) references staffs(staff_id));

SELECT * FROM orders;

INSERT INTO orders
VALUES (3, 'incomplete', '2026-04-25','2026-05-18','2026-04-29',2,1);
```
<img width="550" height="80" alt="{1D6A34CE-6256-477D-A54C-EFEB9C137F52}" src="https://github.com/user-attachments/assets/0df0bdbf-3e9f-4d7b-b707-d160bc408e87" />

```sql
CREATE TABLE order_items(
order_id int,
item_id int, 
PRIMARY KEY (order_id,item_id),
product_id int,
FOREIGN KEY (product_id) references products(product_id),
quantity int,
list_price money,
discount int,
FOREIGN KEY (order_id) references orders(order_id));

SELECT * FROM order_items;

INSERT INTO order_items
VALUES (4, 2, 3, 50,500,10);
```
<img width="403" height="104" alt="{46D6396C-D5A8-4DAF-9DE6-8145D49D2277}" src="https://github.com/user-attachments/assets/acfa2ecf-3b1f-4e65-ae07-4d66c738a4c7" />

<img width="1103" height="658" alt="{51A071BE-97DC-42BF-A120-9BEBC1FFE994}" src="https://github.com/user-attachments/assets/dfadb5a7-8d70-4cdf-aa7d-d63d6f01a15d" />













