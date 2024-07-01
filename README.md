
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

Insights: 
In APAC region, Atliq Exclusive customer operates in 8 major countries they are India, Indonesia, Japan, Philiphines, South Korea, Australia, Newzealand and Bangladesh

--------------------------------------------------------------------------------------------------------------------------------------------------------

2. What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields, unique_products_2020 unique_products_2021 percentage_chg

WITH unique_products AS (
    SELECT
        (SELECT COUNT (DISTINCT product_code) FROM fact_gross_price WHERE fiscal_year = 2020) AS unique_products_2020,
        (SELECT COUNT (DISTINCT product_code) FROM fact_gross_price WHERE fiscal_year = 2021) AS unique_products_2021
)
SELECT
    unique_products_2020,
    unique_products_2021,
    concat (ROUND (((unique_products_2021 - unique_products_2020) / unique_products_2020) * 100, 2), " %") AS percentage_chg
FROM
    unique_products; 

![image](https://github.com/Janaki6/SQL_project/assets/168548897/bc004e99-e420-4ac1-a51e-afb97aaba3c0)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/a60c2e9e-6cbb-45d3-8e3c-00b8fc8e386f)

Insights: with 36.33% increase in unique product, company is meeting strong position in market by addressing changing needs of the customer.

------------------------------------------------------------------------------------------------------------------------------------------------------------

3. Provide a report with all the unique product counts for each segment and sort them in descending order of product counts. The final output contains 2 fields, segment product_count
   
SELECT segment, count (DISTINCT (product_code)) as product_count FROM dim_product group by segment order by product_count desc;

![image](https://github.com/Janaki6/SQL_project/assets/168548897/98b84c1d-9994-4854-99b2-58842e01ac47)

![image](https://github.com/Janaki6/SQL_project/assets/168548897/7277bc90-543f-4a96-8e71-34adc8aefd2a)

Insights: Segments such as notebooks, accessories, and peripherals are showing significant manufacturing growth as compared to desktops, storage, and networking this could be due to greater demand or innovation in notebooks, accessories, and peripheral and less with others.

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






                 
