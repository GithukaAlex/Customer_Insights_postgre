# Customer_Insights_postgre

### 1. What are the names of all the countries in the country table?
<pre>
select Distinct country_name from country </pre>

### 2. What is the total number of customers in the customers table?
<pre> select count(customer_id) as Total_Customers from customers </pre>

</pre>

### 3. What is the average age of customers who can receive marketing emails (can_email is set to 'yes')?
<pre>
select round(avg(age),0) as Average_Age from customers where can_email = 'yes'
</pre>

### 4. How many orders were made by customers aged 30 or older?
<pre?
select count(order_id) from orders o left join customers c on o.customer_id = c.customer_id where c.age >= 30
</pre>

### 5. What is the total revenue generated by each product category?
#### Option 1
<pre>
select category, (count(b.product_id) * p.price)as Revenue from orders o
left join baskets b on o.order_id = b.order_id
left join products p on b.product_id = p.product_id group by category,b.product_id,p.price
</pre>
#### option 2
<pre>
select p.category, sum(p.price) 
from orders o 
left join baskets b on b.order_id = o.order_id 
left join products p on p.product_id = b.product_id group by 1 
</pre>
#### option 3
<pre>
with q as
(select product_id, count(*) as quantity from baskets b group by product_id)
select category, quantity, price, quantity*price as revenue from q 
left join products on products.product_id = q.product_id
</pre>
### 6. What is the average price of products in the 'food' category?
<pre> select round(AVG(price),2) from products where category = 'food' </pre>

### 7. How many orders were made in each sales channel (sales_channel column) in the orders table?
<pre> select sales_channel, count(order_id) as num_orders from orders group by sales_channel </pre>

### 8.What is the date of the latest order made by a customer who can receive marketing emails?
<pre> select c.customer_id,to_char(date_shop,'dd/mm/yyyy') as date from orders o 
left join customers c on o.customer_id = c.customer_id 
where can_email = 'yes' order by date_shop Desc limit 1 </pre>

### 9. What is the name of the country with the highest number of orders?
<pre> select country_name from country 
c left join orders o on c.country_id = o.country_id 
group by country_name order by count(order_id) desc limit 1 </pre>

### 10. What is the average age of customers who made orders in the 'vitamins' product category?
<pre> select round(avg(age)) Average_age from orders o
left join baskets b on o.order_id = b.order_id
left join products p on b.product_id = 'Vitamins' 
</pre>
