Project : Pizza Sales Analysis
Table of Contents:
•	Problem Statement
•	Data Analysis using MySQL
•	Data Cleaning
•	Build Dashboard Or a Report using Power BI
•	Tools, Software and Libraries
•	References

Problem Statement
KPI’s REQUIREMENT
We need to analyze key indicators for our pizza sales data to gain insights into our business performance. Specifically, we want to calculate the following metrics:
1.	Total Revenue: The sum of the total price of all pizza orders.
2.	Average Order Value: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
3.	Total Pizzas Sold: The sum of the quantities of all pizzas sold.
4.	Total Orders: The total number of orders placed.
5.	Average Pizzas Per Order: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.
CHARTS REQUIREMENT
We would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. We have identified the following requirements for creating charts:
1. What days and times do we tend to be busiest: Based on the bar chart for Top 5 Busiest Hours and the line chart for Top 10 Busiest Days, it shows peak orders around midday and evening, with a spike on November 27th.
2. How many pizzas are we making during peak periods: The bar chart of hour-wise order counts indicates production needs of around 2,000–2,500 pizzas during peak hours in per month.
3. What are our best and worst-selling pizzas: The bar charts for Top and Bottom 5 Pizzas by Quantity and Revenue identify the most and least popular pizzas based on quantity sold and total sales revenue.
4. What's our average order value: The card visual at the top shows an Average Order Value of ₹38.31 calculated from total revenue and order count.
5. How well are we utilizing our seating capacity? (we have 15 tables and 60 seats): The Table Utilization matrix calculates daily seating usage percentages, with figures well above 100%, pointing to overcapacity and suggesting an urgent need for expansion.

Data Analysis using MySQL
Utilized MySQL for data extraction and calculation of key metrics such as Total Revenue, Average Order Value, Total Pizzas Sold, Total Orders, and Average Pizzas Per Order.
DATA IMPORT
 
 
 
 
ANALYSIS OF DIFFERENT SQL STATEMENT ON DATA BASE
A.	KPI’s
1.	Total revenue
select sum(total_price) as total_revenue from pizza_sales;
 
2.	Average order value
select sum(total_price) / count(distinct order_id) as avg_order_value from pizza_sales;

 

3.	Total pizza sold
select sum(quantity) as total_pizza_sold from pizza_sales;
 

4.	Total orders
select count(distinct order_id) as total_order from pizza_sales;
 

5.	Average pizzas per order
select round(sum(quantity) / count(distinct order_id), 2) as avg_pizza_per_order from pizza_sales;
 

6.	What days and times do we tend to be busiest?

a.	Top 10 busiest hours (across all days)
select 
    date_format(str_to_date(order_time, '%H:%i:%s'), '%H:00') as hour_slot,
    count(*) as orders_count
from pizza_sales
group by hour_slot
order by orders_count desc
limit 10;
 

b.	 Busiest days
select 
    str_to_date(order_date, '%d-%m-%Y') as order_day,
    count(*) as orders_count
from pizza_sales
group by order_day
order by orders_count desc
limit 10;
 


7.	How many pizzas are we making during peak periods?
select 
    date_format(str_to_date(order_time, '%H:%i:%s'), '%H:00') as hour_slot,
    sum(quantity) as total_pizzas
from pizza_sales
where date_format(str_to_date(order_time, '%H:%i:%s'), '%H') in ('11', '12', '13') 
group by hour_slot;
 

8.	What are our best and worst-selling pizzas?

a.	Best-selling pizzas
select 
    pizza_name,
    sum(quantity) as total_sold
from pizza_sales
group by pizza_name
order by total_sold desc
limit 5;
 

b.	Worst-selling pizzas
select 
    pizza_name,
    sum(quantity) as total_sold
from pizza_sales
group by pizza_name
order by total_sold asc
limit 5;
 

9.	What's our average order value?
Average order value (assuming each order_id is a separate order)
select 
    avg(order_total) as avg_order_value
from (
    select 
        order_id,
        sum(total_price) as order_total
    from pizza_sales
    group by order_id
) as order_summary;
 
10.	How well are we utilizing our seating capacity? (we have 15 tables and 60 seats)
Peak hourly table usage
select 
    str_to_date(order_date, '%d-%m-%Y') as order_day,
    date_format(str_to_date(order_time, '%H:%i:%s'), '%H:00') as hour_slot,
    count(distinct order_id) as tables_occupied,
    round((count(distinct order_id) / 15) * 100, 2) as table_utilization_percent
from pizza_sales
group by order_day, hour_slot
order by table_utilization_percent desc
limit 10;
 

NOTE
If you want to apply the pizza_category or pizza_size filters to the above queries you can use WHERE clause. Follow some of below examples
SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales
WHERE pizza_category = 'Classic'  GROUP BY pizza_name ORDER BY Total_Orders ASC LIMIT 5;
Data Cleaning 
Pizza size category we have in our database is abbreviated and for dashboard we need it in full expanded form. For eg. L= large, M= medium etc, so we will create an alias to temporary change its name in required format. 
 
Build Dashboard or a Report using Power BI
Created a comprehensive dashboard in PowerBI featuring key metrics and charts, including Hourly Trend, Weekly Trend, Sales by Category, Sales by Size, Total Pizzas Sold by Category, Top 5 Best Sellers, and Bottom 5 Worst Sellers.
KPI’S
•	Total Revenue sum([order id])
•	Total Orders countdistinct([order id])
•	Average Order Value [total revenue] / [total orders]
•	Total Pizzas Sold SUM([quantity])
•	Average Pizzas Per Order [total pizzas sold] / [total orders]
 

KEY INSIGHTS
  


  

DASHBOARD
 

 
Tools, Software, and Libraries
•	MySQL Workbench 8.0.36 
for data analysis and storage
•	Power BI Desktop 2025
for dashboard creation and visualization
•	Excel version 2021
for initial data exploration and manipulation

References
•	https://www.youtube.com/@datatutorials1
•	https://topmate.io/data_tutorial
# Pizza_sales-Analysis
