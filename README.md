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

# Dim Calendar
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

