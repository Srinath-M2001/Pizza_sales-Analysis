Here’s the updated `README.md` content with **all SQL queries in lowercase**, as requested:

---

# 🍕 Pizza Sales Analysis

A comprehensive end-to-end data analysis project to explore and visualize pizza sales data using **MySQL**, **Power BI**, and **Excel**. This project focuses on extracting key business insights, building performance dashboards, and optimizing operational strategies.

---

## 📋 Table of Contents

* [Problem Statement](#problem-statement)
* [Data Analysis using MySQL](#data-analysis-using-mysql)
* [Data Cleaning](#data-cleaning)
* [Power BI Dashboard](#power-bi-dashboard)
* [Tools, Software, and Libraries](#tools-software-and-libraries)
* [References](#references)

---

## 📌 Problem Statement

The goal is to analyze key performance indicators (KPIs) for pizza sales to assess business health and optimize performance. The following KPIs were identified:

### 🔑 KPI Requirements:

1. **Total Revenue** – Sum of total price of all pizza orders
2. **Average Order Value** – Total revenue divided by number of orders
3. **Total Pizzas Sold** – Sum of all pizza quantities
4. **Total Orders** – Count of distinct orders
5. **Average Pizzas Per Order** – Total pizzas sold divided by total orders

### 📊 Chart Requirements:

1. **Peak Days and Times** – Bar chart for Top 5 Busiest Hours and line chart for Top 10 Busiest Days indicate peak activity during midday and evenings, especially on November 27th
2. **Production During Peak Periods** – Hourly order count bar chart shows 2,000–2,500 pizzas during peak hours
3. **Best/Worst-Selling Pizzas** – Charts display top and bottom 5 pizzas by quantity and revenue
4. **Average Order Value** – Card visual displays ₹38.31
5. **Seating Capacity Utilization** – Matrix chart shows usage exceeds 100%, indicating overcapacity and need for expansion

---

## 🔍 Data Analysis using MySQL

SQL queries were used to compute KPIs and discover behavioral trends.

### 🗃️ Sample Queries:

* **Total Revenue**

  ```sql
  select sum(total_price) as total_revenue from pizza_sales;
  ```

* **Average Order Value**

  ```sql
  select sum(total_price) / count(distinct order_id) as avg_order_value from pizza_sales;
  ```

* **Total Pizzas Sold**

  ```sql
  select sum(quantity) as total_pizza_sold from pizza_sales;
  ```

* **Total Orders**

  ```sql
  select count(distinct order_id) as total_order from pizza_sales;
  ```

* **Average Pizzas Per Order**

  ```sql
  select round(sum(quantity) / count(distinct order_id), 2) as avg_pizza_per_order from pizza_sales;
  ```

* **Top 10 Busiest Hours**

  ```sql
  select 
      date_format(str_to_date(order_time, '%h:%i:%s'), '%h:00') as hour_slot,
      count(*) as orders_count
  from pizza_sales
  group by hour_slot
  order by orders_count desc
  limit 10;
  ```

* **Top 10 Busiest Days**

  ```sql
  select 
      str_to_date(order_date, '%d-%m-%y') as order_day,
      count(*) as orders_count
  from pizza_sales
  group by order_day
  order by orders_count desc
  limit 10;
  ```

* **Pizzas Made During Peak Hours**

  ```sql
  select 
      date_format(str_to_date(order_time, '%h:%i:%s'), '%h:00') as hour_slot,
      sum(quantity) as total_pizzas
  from pizza_sales
  where date_format(str_to_date(order_time, '%h:%i:%s'), '%h') in ('11', '12', '13')
  group by hour_slot;
  ```

* **Top 5 Best-Selling Pizzas**

  ```sql
  select 
      pizza_name,
      sum(quantity) as total_sold
  from pizza_sales
  group by pizza_name
  order by total_sold desc
  limit 5;
  ```

* **Top 5 Worst-Selling Pizzas**

  ```sql
  select 
      pizza_name,
      sum(quantity) as total_sold
  from pizza_sales
  group by pizza_name
  order by total_sold asc
  limit 5;
  ```

* **Average Order Value by Order ID**

  ```sql
  select 
      avg(order_total) as avg_order_value
  from (
      select 
          order_id,
          sum(total_price) as order_total
      from pizza_sales
      group by order_id
  ) as order_summary;
  ```

* **Seating Capacity Utilization**

  ```sql
  select 
      str_to_date(order_date, '%d-%m-%y') as order_day,
      date_format(str_to_date(order_time, '%h:%i:%s'), '%h:00') as hour_slot,
      count(distinct order_id) as tables_occupied,
      round((count(distinct order_id) / 15) * 100, 2) as table_utilization_percent
  from pizza_sales
  group by order_day, hour_slot
  order by table_utilization_percent desc
  limit 10;
  ```

> *Note: You can apply filters using `where` clause as shown below:*

```sql
select pizza_name, count(distinct order_id) as total_orders 
from pizza_sales 
where pizza_category = 'classic'  
group by pizza_name 
order by total_orders asc 
limit 5;
```

---

## 🧹 Data Cleaning

Pizza size categories in the dataset are abbreviated (e.g., `s`, `m`, `l`). Aliases were used to expand these values to full names like "Small", "Medium", and "Large" for better clarity in visuals.

![image](https://github.com/user-attachments/assets/bc6469c5-d5ef-4d87-9742-0577478e93a5)

---

## 📈 Power BI Dashboard

An interactive Power BI report was developed featuring:

### ✅ KPIs:

* **Total Revenue**
* **Total Orders**
* **Average Order Value**
* **Total Pizzas Sold**
* **Average Pizzas Per Order**
![image](https://github.com/user-attachments/assets/a05ba18b-bfb5-4c0d-ae47-1a50d82b6bf5)

### 📌 Key Insights:
![image](https://github.com/user-attachments/assets/cbb36a09-b908-4d4e-87b6-6e2fd5e85a74)![image](https://github.com/user-attachments/assets/46b1cf03-5133-4380-ba79-4252f3d53b35)
![image](https://github.com/user-attachments/assets/16cdfd23-201e-4f65-8022-d0419d512150)![image](https://github.com/user-attachments/assets/5fc8324b-8658-4196-8199-2b361af15176)


### 📌 Key Visuals:

* Hourly and Weekly Trends
* Sales by Pizza Category & Size
* Top 5 Best-Selling Pizzas
* Bottom 5 Worst-Selling Pizzas
* Table Utilization Matrix

![image](https://github.com/user-attachments/assets/687015ef-560d-4bc7-8544-98d0f55d0f5c)

![image](https://github.com/user-attachments/assets/d6fba67d-01ac-4ba7-a25d-1e10493dbecb)

---

## 🛠️ Tools, Software, and Libraries

* **mysql workbench 8.0.36** – for data analysis
* **power bi desktop 2025** – for dashboard creation
* **excel 2021** – for data exploration

---

## 🔗 References

* [Data Tutorials YouTube Channel](https://www.youtube.com/@datatutorials1)
* [Data Tutorials on Topmate](https://topmate.io/data_tutorial)

---

Let me know if you'd like this saved as a downloadable `.md` file or want help publishing it to your GitHub repo.


