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
3. What are the Top 5 Fastest delivered products and Top 5 slowest delivered products?
4. Which product Subcategory generate most profit?
5. Which segment generates the most profit?
6. Which Top 5 customers made the most profit?
7. What is the total number of products by Subcategory
## Data Analysis
### 1. What was the Average delivery days for different product subcategory?
```sql
SELECT dp.Sub_Category,AVG( DATEDIFF(DAY,srf.Order_Date,srf.Ship_Date)) AS AvgDiliveryDays
FROM SalesRetailFact AS srf
LEFT JOIN DimProduct AS dp
ON
srf.ProductKey = dp.ProductKey
GROUP BY Sub_Category
ORDER BY AvgDiliveryDays DESC

/* It takes Averagely 36 Days to get Table Products deliverd to Customers
35 Days for Furnishings and 32 Days for both Chairs and Bookcases Product respectively to be deliverd to Customer*/
```

### 2. What was the Average delivery days for each segment?
```sql
SELECT dc.Segment,AVG( DATEDIFF(DAY,srf.Order_Date,srf.Ship_Date)) AS AvgDiliveryDays
FROM SalesRetailFact AS srf
LEFT JOIN DimCustomer AS dc
ON
srf.Customer_ID = dc.Customer_ID
GROUP BY Segment
ORDER BY AvgDiliveryDays DESC

/* It takes an Average delivery days of 35 days to get products delivered to the Coporate Segemrnt 
and an Average of 34 and 31 days to get products delivered to the Consumer and Home Office Segments respectively*/
```
### 3. What are the Top 5 Fastest delivered products and Top 5 slowest delivered products?
```sql
SELECT	TOP 5( dp.Product_Name), DATEDIFF(DAY,srf.Order_Date,srf.Ship_Date) AS DiliveryDays
FROM SalesRetailFact AS srf
LEFT JOIN DimProduct AS dp
ON
srf.ProductKey = dp.ProductKey
ORDER BY DiliveryDays 

/* The Top 5 Fastest delivered products are
Sauder Camden County Barrister Bookcase, Planked Cherry Finish
Sauder Inglewood Library Bookcases
O'Sullivan 2-Shelf Heavy-Duty Bookcases
O'Sullivan Plantations 2-Door Library in Landvery Oak
O'Sullivan Plantations 2-Door Library in Landvery Oak 
and are all dilivered within a day.*/
```
```sql
SELECT	TOP 5( dp.Product_Name), DATEDIFF(DAY,srf.Order_Date,srf.Ship_Date) AS DiliveryDays
FROM SalesRetailFact AS srf
LEFT JOIN DimProduct AS dp
ON
srf.ProductKey = dp.ProductKey
ORDER BY DiliveryDays DESC

/* The Top 5  products with Slow Delivery rates which took 214 days to be delivered to customers are;
Bush Mission Pointe Library
Hon Multipurpose Stacking Arm Chairs
Global Ergonomic Managers Chair
Tensor Brushed Steel Torchiere Floor Lamp
Howard Miller 11-1/2" Diameter Brentwood Wall Clock */
```
### 4.  Which product Subcategory generate most profit?
```sql
SELECT dp.Sub_Category,ROUND(SUM(srf.Profit),2) AS TotalProfit
FROM SalesRetailFact AS srf
LEFT JOIN DimProduct AS dp
ON
srf.ProductKey = dp.ProductKey
WHERE srf.Profit > 0
GROUP BY dp.Sub_Category
ORDER BY TotalProfit DESC
/* The Subcategory Chair generated the highest profit of $36,471.1 and 
the least profit comes from the  Subcategory tables */
```
### 5. Which segment generates the most profit?
```sql
SELECT dc.Segment,ROUND (SUM(srf.Profit),2) AS TotlProfit
FROM SalesRetailFact AS srf
LEFT JOIN DimCustomer AS dc
ON
srf.Customer_ID = dc.Customer_ID
WHERE srf.Profit > 0
GROUP BY Segment
ORDER BY TotlProfit DESC

/* The Consumer segment generates the hightest profit of approximatly $35,427
 and 
the least profit comes from the customers in the Home office segment */
```
### 6. Which Top 5 customers made the most profit?
```sql
SELECT	TOP 5( dc.Customer_Name), ROUND (SUM(srf.Profit),2) AS TotalProfit
FROM SalesRetailFact AS srf
LEFT JOIN DimCustomer AS dc
ON
srf.Customer_ID = dc.Customer_ID
WHERE srf.Profit > 0
GROUP BY dc.Customer_Name
ORDER BY TotalProfit DESC

/* Top 5 customers making the most profit for the firm are;
Laura Armstrong
Joe Elijah
Seth Vernon
Quincy Jones
Maria Etezadi */
```
### 7. What is the total number of products by Subcategory
```sql
SELECT Sub_Category, COUNT (Product_Name) AS TotalProduct
FROM DimProduct
GROUP BY Sub_Category
ORDER BY TotalProduct DESC

/* The Furnishing Subcategory has a total of 186 products which is the Subcategory with the highest products 
while Chaies Subcategory has 87 products, Bookcases Subcategory has 48 products and 
Tables Subcategory has a total of 34 profucts */
```




