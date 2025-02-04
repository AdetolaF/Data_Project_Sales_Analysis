# Data_Project_Sales_Analysis

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiMDQ2ODVjNTMtODU1NC00OTVlLWJjYjktODgwMDQzZWY2Zjg5IiwidCI6IjE2NGU2NDhjLThmNDQtNGQzYy04YTk4LTQ5ZDIzOGNhNmM0NSJ9

## Business Request
The company wants Power BI dashboard to provide insights into product sales for the Sales Manager. The request include:
- A Power BI dashboard that updates once a day
- A detailed Sales overview, customer details, and product details
- Top 10 performing products
- Top 10 purchasing customers and their location
- Product sales trend over time
- The Power BI dashboard must allow filter for each data
- Graph and KPIs comparing sales against budget

 ## Data Cleansing & Transformation
To create the necessary data model for doing the analysis and fulfilling the business needs, the following tables were extracted and cleaning using SQL.
One data source (budget) was provided in Excel format and was connected with other tables in the data model.

Below are the SQL statements used for cleaning and transforming the necessary data.

# DIM_Calendar
-- Cleansed DIM_Date Table --
SELECT 
  [DateKey], 
  [FullDateAlternateKey] AS Date, 
   [EnglishDayNameOfWeek] AS Day, 
    [EnglishMonthName] AS Month, 
  Left([EnglishMonthName], 3) AS MonthShort, 
  -- Useful for front end date navigation and front end graphs.
  [MonthNumberOfYear] AS MonthNo, 
  [CalendarQuarter] AS Quarter, 
  [CalendarYear] AS Year 
FROM 
 [AdventureWorksDW2022].[dbo].[DimDate]
WHERE 
  CalendarYear >= 2022

# DIM_Customer
-- Cleansed DIM_Customers Table --
SELECT 
  c.customerkey AS CustomerKey, 
   c.firstname AS [First Name], 
    c.lastname AS [Last Name], 
  c.firstname + ' ' + lastname AS [Full Name], 
  -- Combined First and Last Name
 
  CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
  c.datefirstpurchase AS DateFirstPurchase, 
    g.city AS [Customer City] 
  -- Joined in Customer City from Geography Table
FROM 
  [AdventureWorksDW2022].[dbo].[DimCustomer] as c
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
ORDER BY 
  CustomerKey ASC -- Ordered List by CustomerKey

# DIM_Products
-- Cleansed DIM_Products Table --
SELECT 
  p.[ProductKey], 
  p.[ProductAlternateKey] AS ProductItemCode, 
   p.[EnglishProductName] AS [Product Name], 
  ps.EnglishProductSubcategoryName AS [Sub Category], -- Joined in from Sub Category Table
  pc.EnglishProductCategoryName AS [Product Category], -- Joined in from Category Table
  p.[Color] AS [Product Color], 
    p.[Size] AS [Product Size], 
    p.[ProductLine] AS [Product Line], 
  p.[ModelName] AS [Product Model Name], 
  p.[EnglishDescription] AS [Product Description], 
    ISNULL (p.Status, 'Outdated') AS [Product Status] 
FROM 
  [AdventureWorksDW2022].[dbo].[DimProduct] as p
  LEFT JOIN dbo.DimProductSubcategory AS ps
  ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
  LEFT JOIN dbo.DimProductCategory AS pc
  ON ps.ProductCategoryKey = pc.ProductCategoryKey 
order by 
  p.ProductKey asc

  # Fact_Sales
  -- Cleansed FACT_InternetSales Table --
SELECT 
  [ProductKey], 
  [OrderDateKey], 
  [DueDateKey], 
  [ShipDateKey], 
  [CustomerKey], 
   [SalesOrderNumber], 
  [SalesAmount] 
FROM 
  [AdventureWorksDW2022].[dbo].[FactInternetSales]
WHERE 
  LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2 -- Ensures we always only bring two years of date from extraction.
ORDER BY
  OrderDateKey ASC

  # Data Model
  Below is a screenshot of the data model after cleaning and the tables were connected together.

  The data model also shows how Fact_Budget was connected to the Fact_Sales table.

 ![image](https://github.com/user-attachments/assets/b19faefb-2a33-419b-b31a-19a7f0e25fe7)

 # Dashboard
 Sales Overview Dashboard
![image](https://github.com/user-attachments/assets/4892d4bc-c3ab-4706-8cc3-f5f525868877)

Customer Details
![image](https://github.com/user-attachments/assets/87610e17-e643-4a5e-b9dc-3e80227b818d)

Product details
![image](https://github.com/user-attachments/assets/6001d866-bbbf-48c6-93c8-e8ba1ebe2945)

 
