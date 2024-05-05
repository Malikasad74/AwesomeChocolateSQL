# Awesome Chocolate SQL Project

## Introduction

Welcome to the Awesome Chocolate SQL Project, where the delectable world of chocolate meets the power of SQL analysis! This project delves into a rich dataset focused on all things chocolate, providing a comprehensive exploration of chocolate-related data using SQL queries.

The dataset used in this project offers a tantalizing glimpse into the diverse world of chocolate, encompassing information about chocolate brands, origins, types, ratings, and much more. Leveraging SQL queries, we embark on a journey to uncover hidden insights, trends, and patterns within the dataset, shedding light on the fascinating realm of chocolate production, consumption, and preferences.

Join us as we navigate through the chocolate dataset, querying, analyzing, and deriving meaningful insights that will deepen our understanding of this beloved confectionery. From identifying top-rated chocolate brands to exploring the most popular origins and types, this project promises to satisfy your curiosity and indulge your love for all things chocolate through the lens of SQL analysis. So grab a chocolate bar, sit back, and let's embark on this delicious SQL adventure together! 🍫✨


## INTERMEDIATE PROBLEMS


1. Print details of shipments (sales) where amounts are > 2,000 and boxes are <100?

2. How many shipments (sales) each of the sales persons had in the month of January 2022?

3. Which product sells more boxes? Milk Bars or Eclairs?

4. Which product sold more boxes in the first 7 days of February 2022? Milk Bars or Eclairs?

5. Which shipments had under 100 customers & under 100 boxes? Did any of them occur on Wednesday?

## HARD PROBLEMS


1. What are the names of salespersons who had at least one shipment (sale) in the first 7 days of January 2022?

2. Which salespersons did not make any shipments in the first 7 days of January 2022?

3. How many times we shipped more than 1,000 boxes in each month?

4. Did we ship at least one box of ‘After Nines’ to ‘New Zealand’ on all the months?

5. India or Australia? Who buys more chocolate boxes on a monthly basis?



INTERMEDIATE PROBLEMS:

— 1. Print details of shipments (sales) where amounts are > 2,000 and boxes are <100?

select * from sales where amount > 2000 and boxes < 100;




— 2. How many shipments (sales) each of the sales persons had in the month of January 2022?

select p.Salesperson, count(*) as ‘Shipment Count’
from sales s
join people p on s.spid = p.spid
where SaleDate between ‘2022-1-1’ and ‘2022-1-31’
group by p.Salesperson;




— 3. Which product sells more boxes? Milk Bars or Eclairs?

select pr.product, sum(boxes) as ‘Total Boxes’
from sales s
join products pr on s.pid = pr.pid
where pr.Product in (‘Milk Bars’, ‘Eclairs’)
group by pr.product;




— 4. Which product sold more boxes in the first 7 days of February 2022? Milk Bars or Eclairs?

select pr.product, sum(boxes) as ‘Total Boxes’
from sales s
join products pr on s.pid = pr.pid
where pr.Product in (‘Milk Bars’, ‘Eclairs’)
and s.saledate between ‘2022-2-1’ and ‘2022-2-7’
group by pr.product;




— 5. Which shipments had under 100 customers & under 100 boxes? Did any of them occur on Wednesday?

select * from sales
where customers < 100 and boxes < 100;

select *,
case when weekday(saledate)=2 then ‘Wednesday Shipment’
else ”
end as ‘W Shipment’
from sales
where customers < 100 and boxes < 100;


 

HARD PROBLEMS:

— What are the names of salespersons who had at least one shipment (sale) in the first 7 days of January 2022?

select distinct p.Salesperson
from sales s
join people p on p.spid = s.SPID
where s.SaleDate between ‘2022-01-01’ and ‘2022-01-07’;




— Which salespersons did not make any shipments in the first 7 days of January 2022?

select p.salesperson
from people p
where p.spid not in
(select distinct s.spid from sales s where s.SaleDate between ‘2022-01-01’ and ‘2022-01-07’);




— How many times we shipped more than 1,000 boxes in each month?

select year(saledate) ‘Year’, month(saledate) ‘Month’, count(*) ‘Times we shipped 1k boxes’
from sales
where boxes>1000
group by year(saledate), month(saledate)
order by year(saledate), month(saledate);




— Did we ship at least one box of ‘After Nines’ to ‘New Zealand’ on all the months?

set @product_name = ‘After Nines’;
set @country_name = ‘New Zealand’;

select year(saledate) ‘Year’, month(saledate) ‘Month’,
if(sum(boxes)>1, ‘Yes’,’No’) ‘Status’
from sales s
join products pr on pr.PID = s.PID
join geo g on g.GeoID=s.GeoID
where pr.Product = @product_name and g.Geo = @country_name
group by year(saledate), month(saledate)
order by year(saledate), month(saledate);



— India or Australia? Who buys more chocolate boxes on a monthly basis?

select year(saledate) ‘Year’, month(saledate) ‘Month’,
sum(CASE WHEN g.geo=’India’ = 1 THEN boxes ELSE 0 END) ‘India Boxes’,
sum(CASE WHEN g.geo=’Australia’ = 1 THEN boxes ELSE 0 END) ‘Australia Boxes’
from sales s
join geo g on g.GeoID=s.GeoID
group by year(saledate), month(saledate)
order by year(saledate), month(saledate);



## Conclusion

The Awesome Chocolate SQL Project has provided us with a delicious journey through the world of chocolate, offering valuable insights and solutions to various queries using SQL analysis. Let's summarize our findings based on the provided information:



### Intermediate Problems Insights:

1. We identified shipments where amounts exceeded $2,000 and boxes were fewer than 100.
2. The number of shipments each salesperson handled in January 2022 was determined.
3. The total number of boxes sold for Milk Bars and Eclairs was compared.
4. We analyzed which product, Milk Bars or Eclairs, sold more boxes in the first 7 days of February 2022.
5. Shipments with fewer than 100 customers and boxes were examined, with a focus on whether any occurred on a Wednesday.



### Hard Problems Insights:

1. Salespersons who had at least one shipment in the first 7 days of January 2022 were identified.
2. We determined which salespersons did not make any shipments during the same period.
3. The frequency of shipments with more than 1,000 boxes each month was calculated.
4. We investigated whether at least one box of 'After Nines' was shipped to 'New Zealand' every month.
5. The chocolate box purchasing trends between India and Australia on a monthly basis were compared.

By analyzing these problems, we gained valuable insights into chocolate sales, shipment patterns, and customer behavior. This information can be instrumental in making data-driven decisions to optimize chocolate production, distribution, and marketing strategies.

Through the power of SQL analysis, we've uncovered a rich tapestry of insights within the chocolate dataset, showcasing the versatility and effectiveness of SQL in extracting meaningful information from data. This project serves as a testament to the endless possibilities that arise when combining the irresistible allure of chocolate with the analytical prowess of SQL. Cheers to a world of sweet insights and endless possibilities in the realm of chocolate! 🍫✨
