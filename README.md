
Transforming Atliq Hardwares Sales with SQL Proficiency and Visual Finesse”

[SQL project PDF]()

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



                 
