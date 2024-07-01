
Transforming Atliq Hardwares Sales with SQL Proficiency and Visual Finesse”

[SQL project PDF](https://github.com/Janaki6/SQL_project/blob/main/Resume%20Challenge%204.pdf)

**Problem Statement**

Atliq Hardware’s, a leading computer hardware manufacturer based in India with a global presence, is seeking to enhance its data analytics capabilities. The management has observed a lack of sufficient insights for making swift, data-driven decisions. To address this, they plan to expand their data analytics team by recruiting several junior data analysts. Tony Sharma, the Data Analytics Director, aims to find candidates proficient in both technical and interpersonal skills. To evaluate these abilities, he has organized a SQL challenge.

**Data:**

[Resume Project Challenge 04] The public dataset is completely available on the Code basis website platform where it stores and consolidates all available datasets for analysis. The specific individual datasets at hand can be obtained at this link below: (https://codebasics.io/challenge/codebasics-resume-project-challenge)

**Tools Used:**

1.	MySQL
2.	Power BI
   
**Tables:**

dim_customers, dim_product, fact_gross_price, fact_manufacturing_cost, fact_pre_invoice_deductions, fact_sales_monthly tables

1.	dim_customer: contains customer-related data
2.	dim_product: contains product-related data
3.	fact_gross_price: contains gross price information for each product
4.	fact_manufacturing_cost: contains the cost incurred in the production of each product
5.	fact_pre_invoice_deductions: contains pre-invoice deductions information for each product
6.	fact_sales_monthly: contains monthly sales data for each product.

**Requests:**

1. Provide the list of markets in which customer "Atliq Exclusive" operates its business in the APAC region.
   
**Query**: Select distinct(market) from dim_customer where region = "APAC" and customer = "Atliq Exclusive";

![image](https://github.com/Janaki6/SQL_project/assets/168548897/31fca8a3-6093-489c-88ba-7a8f8b0974bc)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/8bbc0387-5198-4908-903a-faa2a3615037)

**Insights**: 

In APAC region, Atliq Exclusive customer operates in 8 major countries they are India, Indonesia, Japan, Philiphines, South Korea, Australia, Newzealand and Bangladesh

--------------------------------------------------------------------------------------------------------------------------------------------------------

2. What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields, unique_products_2020 unique_products_2021 percentage_chg

WITH unique_products AS (
SELECT
(SELECT COUNT (DISTINCT product_code) FROM fact_gross_price WHERE fiscal_year = 2020) AS unique_products_2020,
(SELECT COUNT (DISTINCT product_code) FROM fact_gross_price WHERE fiscal_year = 2021) AS unique_products_2021)

SELECT
unique_products_2020,
unique_products_2021,

concat (ROUND (((unique_products_2021 - unique_products_2020) / unique_products_2020) * 100, 2), " %") AS percentage_chg
FROM
unique_products; 
    

![image](https://github.com/Janaki6/SQL_project/assets/168548897/bc004e99-e420-4ac1-a51e-afb97aaba3c0)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/a60c2e9e-6cbb-45d3-8e3c-00b8fc8e386f)

**Insights**:

with 36.33% increase in unique product, company is meeting strong position in market by addressing changing needs of the customer.

------------------------------------------------------------------------------------------------------------------------------------------------------------

3. Provide a report with all the unique product counts for each segment and sort them in descending order of product counts. The final output contains 2 fields, segment product_count
   
SELECT segment, count (DISTINCT (product_code)) as product_count FROM dim_product group by segment order by product_count desc;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/98b84c1d-9994-4854-99b2-58842e01ac47)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/7277bc90-543f-4a96-8e71-34adc8aefd2a)

**Insights**: 

Segments such as notebooks, accessories, and peripherals are showing significant manufacturing growth as compared to desktops, storage, and networking this could be due to greater demand or innovation in notebooks, accessories, and peripheral and less with others.

-----------------------------------------------------------------------------------------------------------------------------------------------------------

 4. Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields, segment product_count_2020 product_count_2021 difference
    
    with cte1 as (
    select p.segment, count(DISTINCT(f.product_code)) as product_count_2020, f.fiscal_year from fact_gross_price f
    , dim_product p where f.product_code = p.product_code 
    group by f.fiscal_year, p.segment having f.fiscal_year = 2020 ),
   
    cte2 as (
    select p.segment, count(DISTINCT(f.product_code)) as product_count_2021, f.fiscal_year from fact_gross_price f
    , dim_product p where f.product_code = p.product_code 
    group by f.fiscal_year, p.segment having f.fiscal_year = 2021 )
    
    select cte1.segment, product_count_2020, product_count_2021, (product_count_2021 - product_count_2020) 
    as difference from cte1, cte2 where cte1.segment = cte2.segment order by difference desc;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/91fd7ae0-bfc9-4c69-b042-64187e21406f)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/04c9f792-c0d8-4b72-be4b-adb4729215da)


**Insights**: 

Company has introduced 34 new unique products in Accessories category while storage and networking category has lowest production growth. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

5. Get the products that have the highest and lowest manufacturing costs. The final output should contain these fields, product_code product manufacturing_cost codebasics.io
   
select p.product_code, p.product, concat("$ ",round(f.manufacturing_cost,2)) as manufacturing_cost
 from fact_manufacturing_cost f join dim_product p
    using (product_code) 
where manufacturing_cost in (
    (Select max(manufacturing_cost) from fact_manufacturing_cost),
    (Select min(manufacturing_cost) from fact_manufacturing_cost))
 Order by manufacturing_cost desc;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/2d019e71-3f6c-4737-a969-a86dfe96c360)

$240.54

A6120110206	

Personal Desktop

![image](https://github.com/Janaki6/SQL_project/assets/168548897/da4c0cdc-8cf0-488c-96ca-994ccb159f62)

$0.89 

A2118150101

Mouse

**Insights**: 

Among our products, Personal Desktop with product code: A6120110206 is having highest manufacturing cost: - 240.54 dollars, whereas Mouse with product code: A2118150101 is having the lowest production cost.

---------------------------------------------------------------------------------------------------------------------------------------------------------

 6. Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct for the fiscal year 2021 and in the Indian market. The final output contains these fields, customer_code customer average_discount_percentage
    
select f.customer_code, c.customer, concat(round(avg(f.pre_invoice_discount_pct)*100,2), "%") as average_discount_percentage 
 from fact_pre_invoice_deductions f join dim_customer c 
 using(customer_code)
 where f.fiscal_year = 2021 and c.market = "India"
 group by c.customer_code, c.customer
 order by 
 avg(pre_invoice_discount_pct) desc limit 5;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/67923293-b2ee-4909-a08e-11c95f8ed834)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/f40a58db-4499-4fc0-9765-8530d3d8f25c)

**Insights**: 

Top 5 customers have average of 30.21% discount percentage.
Flipkart has received the highest pre invoice discount percent i.e., 30.83%.

-------------------------------------------------------------------------------------------------------------------------------------------------------

7. Get the complete report of the Gross sales amount for the customer “Atliq Exclusive” for each month. This analysis helps to get an idea of low and high-performing months and take strategic decisions. The final report contains these columns: Month Year Gross sales Amount
   
select concat (MONTHNAME (date), "(", year(date), ")") as month, year(date) as calendar_year, 
 round (sum (gross_price * sold_quantity)/1000000,2) as Gross_sales_amount_mln 
 from  
 fact_gross_price gp join fact_sales_monthly fs
 on 
 gp.product_code = fs.product_code
 join dim_customer dc on 
 dc.customer_code = fs.customer_code
 where customer = "Atliq Exclusive" group by month, fs.fiscal_year order by fs.fiscal_year;

 ![image](https://github.com/Janaki6/SQL_project/assets/168548897/29d21546-9900-4398-9dc1-4a0b390f04f9)

 ![image](https://github.com/Janaki6/SQL_project/assets/168548897/b18eb75f-b932-4f7a-bacf-0e02bc9ec168)

**Insights**:

•	For Atliq Exclusive Store maximum sales were recorded in November- 2020 and lowest sales recorded in March-2020.
•	Low sales from March to August because of COVID when stores were shut.
•	Sales started improving from September-2020 onwards due to ease in lockdown restrictions and also due to festival season in India.

------------------------------------------------------------------------------------------------------------------------------------------------------

8. In which quarter of 2020, got the maximum total_sold_quantity? The final output contains these fields sorted by the total_sold_quantity, Quarter total_sold_quantity
   
SELECT 
 case
 when MONTH (date) IN (9,10,11) then "Q1"
 when MONTH (date) IN (12,1,2) then "Q2"
 when MONTH (date) IN (3,4,5) then "Q3"
 else "Q4"
 end as fiscal_quarter, 
 sum(sold_quantity)/1000000 as total_sold_quantity_mln
 from fact_sales_monthly 
 where fiscal_year = 2020
 group by fiscal_quarter 
 order by total_sold_quantity_mln desc;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/c285e2b3-6392-4e48-aa70-b68c4e1ad277)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/84778900-4a59-47c8-a8e1-7eea9b0a92c2)

**Insights**:

Total sold quantity in Q3(March, April, and May) of fiscal year 2020 was less due to COVID. Sales increased in Q4 with increased in demand of computers, notebooks and accessories for online classes.

---------------------------------------------------------------------------------------------------------------------------------------------------------

9. Which channel helped to bring more gross sales in the fiscal year 2021 and the percentage of contribution? The final output contains these fields, channel gross_sales_mln percentage
    
with output as (
SELECT dc.channel, round(sum((gp.gross_price * fs.sold_quantity)/1000000),2) as gross_sales_mln
FROM fact_gross_price gp join fact_sales_monthly fs on
gp.product_code = fs.product_code
join dim_customer dc on
fs.customer_code = dc.customer_code
where fs.fiscal_year = 2021
group by channel)

select channel, gross_sales_mln as gross_sales_mln,  
concat(round(gross_sales_mln *100 / sum(gross_sales_mln) over(),2),"%") as percentage
from output order by percentage desc;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/bd886f6a-ff80-447d-bf31-34b990142065)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/d758cc26-0dd7-44b3-8f69-e66c21885181)

**Insights**: 

Retailer channel brought most sales to the company with 73.22% contribution while Distributor contributes sales of 11.31% of total.

-------------------------------------------------------------------------------------------------------------------------------------------------------

10. Get the Top 3 products in each division that have a high total_sold_quantity in the fiscal_year 2021? The final output contains these fields, division product_code codebasics.io product total_sold_quantity rank_order

with cte1 as (
SELECT division, pp.product_code , product, sum(sold_quantity) as total_sold_quantity
FROM dim_product pp join fact_sales_monthly fs on
 pp.product_code = fs.product_code where
 fiscal_year = 2021 group by fs.product_code order by total_sold_quantity desc),
 cte2 as 
 (select *,
rank () over (partition by division order by total_sold_quantity desc) as rank_order
	from cte1)
 select * from cte2 where rank_order<=3;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/709ec7d0-5cf5-47e1-90b5-8cec651d725b)


**Insights**: 

The most popular products in N&S were pen drives, with about 700,000 sold. In P&A, the top products were mice, with around 400,000 sold. For PC, the top products were personal laptops, and about 17,000 of them were sold


**Recommendations**:

Maximize Online Discounts: Work with Flipkart, Viveks, Ezone, Croma, and Amazon for exclusive online deals.
Boost March and September: Implement special offers to improve sales during these low months.
Cost Reduction: Reduce manufacturing costs for the AQ HOME Allin1 Gen 2.
Replicate Q1 Success: Analyze and replicate strategies that boosted first-quarter sales in 2020.

