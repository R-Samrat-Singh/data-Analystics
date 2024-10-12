use obdc_connected;

create table df_orders(
order_id int primary key,
order_date date,
ship_mode varchar(20),
segment varchar(20),
country varchar(20),
city varchar(20),
state varchar(20),
postal_code varchar(20),
region varchar(20),
category varchar(20),
sub_category varchar(20),
product_id varchar(50),
quantity int,
discount decimal(7,2),
sale_price decimal(7,2),
profit decimal(7,2)
);

select *
from df_orders;


---find top 10 revenue generating products

select product_id , Round(sum(sale_price)) as sales 
from df_orders
group by product_id
order by sales desc
limit 10;

 ---find top 5 revenue generating products in each region
 
 with cte as (
 select region, product_id , Round(sum(sale_price)) as sales 
from df_orders
group by product_id , region)
select *
from 
  (select *
   , row_number() over(partition by region order by sales desc)as rn
   from cte) as ranked
  where rn<=5; 
 
 
-- find month over month growth growth comparison for 2022 and 2023
with Cte as (
 SELECT  distinct CAST(YEAR(order_date) AS CHAR) AS order_year , month(order_date) as order_month , round(sum(sale_price)) as sales
FROM df_orders
group by order_year,order_month
-- order by order_year , order_month;
        )
     select order_month
    , sum(case when order_year='2022' then sales else 0 end) as sale_2022
    , sum(case when order_year='2023' then sales else 0 end) as sale_2023
     from cte
     group by order_month
     order by order_month;
    

    
  -- each category which month has highest sales
    WITH cte AS 
(
    SELECT 
        DATE_FORMAT(order_date, '%Y%m') AS orders_date_month, 
        ROUND(SUM(sale_price)) AS sales, 
        category
    FROM df_orders
    GROUP BY category, orders_date_month
)
select *
from (
      select * 
       , row_number() over (partition by category order by sales desc) as rnk
       from cte
                ) as ranked1
        where rnk <2;
       
 
-- which sub-category has the highest growth by profit in 2023 compared to 2022

 with Cte as (
 SELECT sub_category,  CAST(YEAR(order_date) AS CHAR) AS order_year , round(sum(sale_price)) as sales
FROM df_orders
group by order_year , sub_category
-- order by order_year , order_month
        )
,  cte2 as 
      (
     select sub_category
    , sum(case when order_year='2022' then sales else 0 end) as sale_2022
    , sum(case when order_year='2023' then sales else 0 end) as sale_2023
     from cte
     group by sub_category
     )
    select * 
     ,( sale_2023 - sale_2022)/sale_2022*100 as sales_difference  
     from cte2
     order by sales_difference desc 
     limit 1;
    
    drop table netflix_raw;
   
  
    
     
     
          

     
     



