## Customer Segmentation for a Subscription Service
### Project Summary
This project analyzes customer data for a subscription service to identify key customer segments, understand subscription patterns, and analyze trends in cancellations and renewals. The goal is to provide actionable insights into customer behavior and subscription types. The final deliverable is a Power BI dashboard presenting the analysis results.
### Objectives
- Identify Customer Segments
- Classify customers based on subscription types, duration, and region to gain insights into different customer groups.
- Understand Subscription Patterns
- Analyze subscription trends, including the average duration of subscriptions and the most popular subscription types, to determine customer preferences.
- Analyze Cancellations and Renewals
- Investigate cancellation rates by region, subscription type, and duration, identifying factors that influence cancellations and renewals.
- Monitor Revenue by Subscription Type
- Calculate total revenue generated from each subscription type to determine which plans are most profitable.
- Develop Interactive Data Visualizations
- Create a Power BI dashboard with interactive elements to allow stakeholders to explore and analyze customer segments, trends, and behaviors dynamically.
- Provide Actionable Insights
- Deliver insights that can guide decision-making for customer retention strategies, targeted marketing, and product offerings based on customer segment analysis.
### Project Structure
The project consists of three main components: Excel Data Exploration, SQL Query Analysis, and Power BI Dashboard.
###### Folders and Files
- /data: Contains raw sales data used for this analysis.
- sql/scripts: SQL queries and scripts.
- /Excel Reports: Contains Excel summary tables and initial analysis.
- /PowerBI: Contains Power BI files for the dashboard.
### Tools Used
The following applications was used for data cleaning, analysis, manipulation and visualizations:
- Microsoft Excel: Data cleaning, exploration and initial metrics calculation.
- SQL Server DB: for running SQL queries
- PowerBI: for interactive data visualization dashboard
- Github to document my portfolio

### Data Cleaning & Preprocessing
The preprocessing stage took the following shape:
1. Data loading and inspection
2. Removing duplicates and blanks (null vales)
3. Data cleaning and formatting
4. Transformation of the variables and datatypes
### Exploraroty Data Analysis 
The EDA will be use to explore the data while providing answer to the following questions about subscription pattern such as;
- Retrieve the total number of customers from each region.
- Find the most popular subscription type by the number of customers.
- Find customers who canceled their subscription within 6 months.
- Calculate the average subscription duration for all customers.
- Find customers with subscriptions longer than 12 months.
- Calculate total revenue by subscription type.
- Find the top 3 regions by subscription cancellations.
- Find the total number of active and canceled subscriptions.
### Data analysis
Descriptive Statistics: Initial insights, like average sales, top-selling products, and monthly trends using
Pivot Tables, Excel formula and SQL queries.
```Excel
=DATEDIF(E2,F2,"d")
=AVERAGE(I:I)
=RANK.EQ(COUNTIF(D:D,D2), COUNTIF(D:D,D:D),0)
```
##### Pivot Tables

#### SQL Queries
```
select * from SALESDATA$
---DESKTOP-194OHGQ\SQLEXPRESS---
---to remove duplicates from salesdata set---

WITH DuplicateRows AS (
select
ROW_NUMBER() OVER (PARTITION BY Product_type, Region, OrderDate, Quantity, UnitPrice, Revenue ORDER BY OrderID, CustomerId) AS RowNum
FROM Salesdata$
)
DELETE FROM DuplicateRows
WHERE RowNum > 1;

---retrieve the total sales for each product category.----

select Product_type, sum(revenue) as Total_Sales from salesdata$ group by Product_type order by Total_Sales asc

---Find the number of sales transactions in each region.----

select Region, count(OrderDate) as Number_of_Sales_Transactions from salesdata$ group by Region


----- calculate monthly sales totals for the current year.---

select 
year(orderdate) as Year,
month(orderDate) as Month,
sum(revenue) as Totalsales
from salesdata$
where year(orderDate)=year(GETDATE())
GROUP BY 
YEAR(orderdate), MONTH(orderDate)
ORDER BY
YEAR, month


---Find the top 5 customers by total purchase amount.----

select top 5
Customerid,
Product_type,
sum(revenue) as Totalpurchase
from SalesData$
group by customerid, product_type
order by Totalpurchase desc;


---Calculate the percentage of total sales contributed by each region---

SELECT Region, SUM(Revenue) AS Total_Sales, (SUM(Revenue) / (SELECT SUM(Revenue) FROM Salesdata$) * 100) AS Percentage
FROM Salesdata$
GROUP BY Region
ORDER BY SUM(Revenue) DESC;


---Identify products with no sales in the last quarter---

select Orderid, customerId, Product_type
from Salesdata$
where Orderid NOT IN (
select orderid
from Salesdata$
where Orderdate>=dateadd(day, -90, getdate())
)


---Find the highest-selling product by total sales value---

 select top 1
 orderid,
 product_type,
 sum(revenue) as TotalSales from salesdata$
 group by orderid, product_type
 order by
 Totalsales desc;

 ---Calculate total revenue per product---

 select product_type,
 sum(revenue) as TotalRevenue from salesdata$
 group by product_type
 order by TotalRevenue desc;
```
### Data Visualization
![Salesdata3](https://github.com/user-attachments/assets/c3d5b47e-e2cf-49cb-8d11-92f9a3c80c20)
![Salesdata4](https://github.com/user-attachments/assets/68229dce-702f-475d-8d3f-c645c69b845a)
![salesdata5](https://github.com/user-attachments/assets/1e13e631-4c23-46dd-87ba-64fb5199d7bc)
#### PowerBI Visuals
![salesdata](https://github.com/user-attachments/assets/1d2874ea-5171-4a99-8438-dbb2ab510897)
### Key Findings & Insights
##### Summary of Results: 
- The number of sales transactions is evenly distributed between the four Regions (equal number of sales transactions per Region).
- The top five customers from previous transactions revealed that the customer with ID 1001 had the highest purchase having cut across all products.
- The highest selling product from the list is Shoes followed by Shirts, Socks is the least on the list.
- All products has no sales in the last quarter.
- The total sales by region reveal the South having the highest number of revenue followed by the East and next to this is the North with West having the lowest.
- The average sales on each product is #211.75
- Total monthly sales reveals February as the peak of sales, having a total of #1,100 and next to this is january. Over 50% more sales than the closest month.
- 
##### Implications: 
The trend revealed by this findings is that Shoes and Shirts should always take the utmost priority during the peak periods which falls between January and February being the time when major sales are made. For the other months with low sales, discount and promotions would enhance and motivate customers to patronise the store.
### Recommendations
Inventory level and restock (Procurement) should focus on Shoes and Shirts by doubling the stocks to avoid turning down request from customers in January and February respectively. 
### Project Summary
The summary of this research and analysis shows that the store should focus it strenght in the sales of shoes, shirts, hat and glove in descending order. Jackets and socks should receive less attention.
### References
Data Source(s): Data authored by The incubator Hub

