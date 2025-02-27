# North America Sales Retail Optimization Analysis
## Project Overview
North America Retail is a large retail company with stores in many locations, selling a wide variety of products to different customers. They aim to provide good customer service and an easy shopping experience. 
This analysis will examine their current operations to find areas where they can improve and suggest ways to increase efficiency and profits.
## Data Source
The Data used is a Retail Supply Chain Sales Analysis.CSV and Calender Date.CSV
## Tool Used
- SQL
## Data Cleaning and Preparation
1. Data importation and Cleaning
2. Spliting into Facts and Dimention Tables, the Dimention table includes;
   - DimCustomer
   - DimProduct
   - DimLocation
3. Creating an ERD
   ## Objectives
1. What was the Average delivery days for different product subcategory?
2. What was the Average delivery days for each segment?
3.What are the Top 5 Fastest delivered products and Top 5
slowest delivered products?
4. Which product Subcategory generate most profit?
5. Which segment generates the most profit?
6. Which Top 5 customers made the most profit?
7. What is the total number of products by Subcategory
## Data Analysis
 1. What was the Average delivery days for different product subcategory?
```sql
SELECT dp.Sub_Category,AVG( DATEDIFF(DAY,srf.Order_Date,srf.Ship_Date)) AS AvgDiliveryDays
FROM SalesRetailFact AS srf
LEFT JOIN DimProduct AS dp
ON
srf.ProductKey = dp.ProductKey
GROUP BY Sub_Category
ORDER BY AvgDiliveryDays DESC
/* It takes Averagely 36 Days to get Table Products deliverd to Customers
35 Days for Furnishings and 32 Days for both Chairs and Bookcases Product respectively to be deliverd to Customer*/```




