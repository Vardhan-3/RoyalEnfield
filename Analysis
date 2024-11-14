-- 1. Find the date of the first purchase for each customer.


select min(saledate) as first_purchase,concat(firstname,lastname) as fullname
from sales as s
join customers as c
on s.customerid = c.customerid
group by fullname
order by first_purchase;

-- 2. Write a SQL  query to find the top 3 most expensive bikes (based on price) for each year and model.

with price_rnk_cte as 
(
select model,price, year,
RANK() OVER (PARTITION BY year,model order by price desc) as price_rnk
from bikes
group by year,model,price
order by price_rnk asc
)

select model,max(price) as max_price, year,price_rnk from price_rnk_cte
where price_rnk<=3
group by year,model,price,price_rnk
order by price_rnk asc,max_price desc;


-- 3. Retrieve the Latest Service for Each Bike.

select bikeid,max(servicedate) as latest_service,servicedescription,servicecost
from servicerecords 
group by bikeid,servicedescription,servicecost
order by latest_service asc;

-- 4. Find the Price difference between the current bike and the  next bike in the same year.


select bikeid,model,year,price,
LEAD(price) over(partition by year order by price) - price as price_diff
from bikes;

-- 5. Find the Maximum sale amount for each month.

select extract(month from saledate) as month,
extract(year from saledate) as year,
max(saleamount) as maxsale_amt
from sales
group by month,year
order by year;


-- 6. Concatenate the first and last names of customers, and display them in upper case.

select upper(concat(firstname,' ',lastname))as fullname
from customers;


-- 7. Determine the quarter in which each sale occurred.


select saleid,extract(quarter from saledate) as quarter, saledate
from sales
group by saleid
order by saledate;

-- 8. Calculate the running total of service costs for each bike.

select bikeid,servicedate,servicecost,
sum(servicecost) over (partition by bikeid order by servicedate) as runningtotal_cost
from servicerecords;


-- 9. Find the top dealers based on the total sales amount across the bikes.

select s.bikeid,d.dealername, 
sum(s.saleamount) over (partition by d.dealerid) as totalsale_amt
from dealers as d
join sales as s
on d.dealerid = s.dealerid

-- 10. Find the count of bikes sold each year and categorize them into three groups: ‘Low’,’ Medium’, and ‘High’ based on their prices.

SELECT
    EXTRACT(year FROM s.saledate) AS year,
    COUNT(b.bikeid) AS total_bike_sold,
    SUM(CASE WHEN b.price < 200000 THEN 1 ELSE 0 END) AS "Low",
    SUM(CASE WHEN b.price >= 200000 AND b.price <= 300000 THEN 1 ELSE 0 END) AS "Medium",
    SUM(CASE WHEN b.price > 300000 THEN 1 ELSE 0 END) AS "High"
FROM
    sales AS s
JOIN
    bikes AS b ON s.bikeid = b.bikeid
GROUP BY EXTRACT(year FROM s.saledate)
order by year;

-- 11. Find the Top 5 Bike models with the highest cost.

select model, max(price) as highest_cost
from bikes
group by model
order by highest_cost
limit 5;

-- 12. Write a query to compare a bike model price in years 2022 and 2023. retrieve in two different column for 2022 and 2023.

select model,
   max(case when year = 2022 then price end) as price_2022,
   max(case when year = 2023 then price end) as price_2023
from bikes
group by model;

-- 13. Retrieve the count of highly sold bike model in both the year with its SaleAmount.

select count(*) as count,model,sum(price) as totalsale_amt from bikes
group by model
order by count desc;

-- 14. Write a query to retrieve how much bikes arw sold by each dealer in year 2023(dealer name, bike sales count, total sales amount)

SELECT
    d.dealername,
    COUNT(s.bikeid) AS bikesalecount,
    SUM(s.saleamount) AS totalsale_amt
FROM
    sales AS s
JOIN
    bikes AS b ON s.bikeid = b.bikeid
JOIN
    dealers AS d ON s.dealerid = d.dealerid 
WHERE
    EXTRACT(year FROM s.saledate) = 2023
GROUP BY
    d.dealername;

-- 15. From the above Dealers table Retrieve the count of dealers in each location.

select count(*) as dealercount,location
from dealers
group by location;

-- 16. Retrieve the top 5 models from the bikes table and the max service cost of each bike  with its description from servicerecord table.

select b.model,max(s.servicecost) as maxservicecost, s.servicedescription
from bikes as b
join servicerecords as s
on b.bikeid = s.bikeid
group by b.model,s.servicedescription
order by maxservicecost desc
limit 5;
