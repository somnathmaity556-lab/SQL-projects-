# SQL-projects- Book Store data Analysis
```
Create table Books ( 
			Book_ID int Generated always as identity primary key,
			Title varchar(60),
			Author Varchar(30),
			Genre Varchar(15),
			Published_Year Date,
			Price  Decimal (6,2),
			Stock Int
);
```
```
select * from books 
alter table books
alter column published_year  type Int
using published_year:: INT
```
``
alter table books 
drop column Published_year, 
add Column Published_Year int
```
Create Table Customers (
			Customer_ID	int Generated always as identity primary key,
			Name	Varchar(30),
			Email	Varchar(40),
			Phone	Int,
			City	Varchar(10),
			Country	Varchar(21)
);

Alter table Customers
alter column Country type Varchar(60) using Country :: Varchar(60) 

select * from customers 

Create table Orders (
				Order_ID	int Generated always as identity primary key, 
				Customer_ID	int,
				Book_ID	int,
				Order_Date	date, 
				Quantity	int ,
				Total_Amount	decimal(5,2)
);

SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;

-- 1) Retrieve all books in the "Fiction" genre:
Select * from Books
where Genre like 'Fiction%';

-- 2) Find books published after the year 1950:
Select * from books 
where Published_year > 1950;

-- 3) List all customers from the Canada:
select * from Customers
where country like 'Canada%'

-- 4) Show orders placed in November 2023:
Select * from Orders 
where Order_date between '2023-11-01' and '2023-11-30';

-- 5) Retrieve the total stock of books available:
Select Sum (Stock ) as Total_stock from books 


-- 6) Find the details of the most expensive book:
select * from books 
order by price desc limit 1 

-- 7) Show all customers who ordered more than 1 quantity of a book:
select o.customer_id,
		C.name, 
		Count (distinct book_id) as no_of_books,
		sum(o.quantity) as Total_ordered_quantity
from Orders o
Left Join Customers c
on c.customer_id=o.Customer_id
Group by o.customer_id , c.name
Having sum(o.quantity)>1
Order By o.customer_id

-- 8) Retrieve all orders where the total amount exceeds $20:
SELECT * FROM Orders
where total_amount > 20

-- 9) List all genres available in the Books table:
select distinct genre from Books

-- 10) Find the book with the lowest stock:

SELECT title, stock
FROM Books
WHERE stock = (SELECT MIN(stock) FROM Books);

SELECT title, stock
FROM (
    SELECT 
        title,
        stock,
        DENSE_RANK() OVER (ORDER BY stock ASC) AS rnk
    FROM Books
) t
WHERE rnk = 1;

SELECT title, stock
FROM Books
WHERE stock = (SELECT MIN(stock) FROM Books);
-- 11) Calculate the total revenue generated from all orders:
select sum(total_amount) as Total_Revenue
	From orders


-- Advance Questions : 

-- 1) Retrieve the total number of books sold for each genre:

select b.genre,sum(o.quantity)as Total_sold 
	from Books b
Left Join Orders o 
on b.book_id=o.book_id
Group by 1

-- 2) Find the average price of books in the "Fantasy" genre:
select distinct genre , avg(price) as avg_price 
	from Books
group by 1

-- 3) List customers who have placed at least 2 orders:
select * from orders 
Select c.customer_id,c.name,count(o.order_id) as order_count   from customers c
left join orders o
on c.customer_id = o.customer_id
Group by 1,2
having Count(o.order_id)>=2 

-- 4) Find the most frequently ordered book:
SELECT * FROM public.orders
ORDER BY book_id ASC LIMIT 100

select o.Book_id, b.title,count(o.book_id) as no_of_Orders from Orders o
left join books b
on o.book_id=b.book_id
Group by 1,2
order by 3 desc

-- 3) List customers who have placed at least 2 orders:
select * from orders 
Select c.customer_id,c.name,count(o.order_id) as order_count   from customers c
left join orders o
on c.customer_id = o.customer_id
Group by 1,2
having Count(o.order_id)>=2 

-- 4) Find the most frequently ordered book:
SELECT * FROM public.orders
ORDER BY book_id ASC LIMIT 100

select o.Book_id, b.title,count(o.book_id) as no_of_Orders from Orders o
left join books b
on o.book_id=b.book_id
Group by 1,2
order by 3 desc

-- 5) Show the top 3 most expensive books of 'Fantasy' Genre :
SELECT * FROM books
where Genre = 'Fantasy' 
Order by price desc
limit 3

-- 6) Retrieve the total quantity of books sold by each author:
SELECT b.author,sum(o.quantity) as total_quantity 
	FROM books b
	left join orders o
	on b.book_id = o.book_id
	Group by 1

-- 7) List the cities where customers who spent over $30 are located:

select c.city, sum(o.total_amount) as _total_spent
	from orders o
	left join customers c
	on o.customer_id=c.customer_id
	Group by 1
	Having sum(o.total_amount) >30
	order by 2 desc

-- 8) Find the customer who spent the most on orders:
select c.name, sum(o.total_amount) as _total_spent
	from orders o
	left join customers c
	on o.customer_id=c.customer_id
	Group by 1
	order by 2 desc limit 1


SELECT b.book_id, b.title, b.stock, COALESCE(SUM(o.quantity),0) AS Order_quantity,  
	b.stock- COALESCE(SUM(o.quantity),0) AS Remaining_Quantity
FROM books b
LEFT JOIN orders o ON b.book_id=o.book_id
GROUP BY b.book_id ORDER BY b.book_id;


SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;
