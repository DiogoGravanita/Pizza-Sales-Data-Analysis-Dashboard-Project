# Pizza Sales Data Analysis + Dashboard Project

<br/><br/>

## Introduction:

Welcome to the Pizza Sales Data Cleaning and Dashboard Creation Project! In this project, we'll be exploring and analyzing sales data from a fictional pizza restaurant chain. The goal is to perform data cleaning tasks using SQL in Microsoft SQL Server Management Studio (MSSMS) to prepare the raw data for visualization. Subsequently, we'll create insightful and interactive dashboards using Power BI to visualize key performance indicators (KPIs) and trends.

<br/><br/>

## Project Overview

### Data Source
The dataset we'll be working with contains detailed information about pizza sales transactions. It includes data points such as pizza ID, order ID, pizza name, quantity, order date, order time, unit price, total price, pizza category, pizza ingredients, and pizza size.

### Objectives

 1. Data Cleaning: We'll start by cleaning the raw data using SQL in MSSMS. This involves tasks such as handling missing values, correcting data types, and transforming data to make it suitable for analysis.

 2. Dashboard Creation: Once the data is cleaned, we'll create dynamic dashboards using Power BI to visualize various KPIs and trends related to pizza sales. These dashboards will provide valuable insights into the performance of the pizza restaurant chain.

<br/><br/>


## Key Performance Indicators (KPIs)

We'll focus on the following KPIs to evaluate the performance of the pizza sales:

<br/><br/>

1. Total Revenue

2. Average Order value

3. Total Pizzas Sold

4. Total Orders

5. Average Pizzas Per Order

<br/><br/>

## Charts and Visualizations:

We'll create a variety of charts and visualizations to present the data effectively:

<br/><br/>

1. Daily Trend for Total Orders
2. Monthly Trend for Total Orders
3. Percentage of Sales by Pizza Category
4. Percentage of Sales by Pizza Size
5. Total Pizzas Sold by Pizza Category
6. Top 5 Best Sellers by Revenue, Total Quantity and Total Orders
7. Bottom 5 Worst Sellers by Revenue, Total Quantity and Total Orders




<br/><br/>
## Data sources:

The dataset used for this analysis is the "pizza_sales.xlsx" file.

<br/><br/>

## Tools and Technologies:

 - Microsoft SQL Server Management Studio for data manipulation and analysis.
 - PowerBI for data visualization and statistical analysis.
 - Obsidian for documentation purposes.

<br/><br/>

## Data cleaning and preparation:
<br/><br/>



### Changing column Data Types:

Upon inspecting the column data types using the query:

```sql
SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'PizzaSales';
```

I observed the following structure:

```sql
COLUMN_NAME          DATA_TYPE
-------------------------------------
pizza_id             float
order_id             float
pizza_name_id        nvarchar
quantity             float
order_date           datetime
order_time           datetime
unit_price           float
total_price          float
pizza_size           nvarchar
pizza_category       nvarchar
pizza_ingredients    nvarchar
pizza_name           nvarchar
```


Notably, the order_date and order_time columns were in datetime format, which did not align with our intended data representation. The date format was '2015-01-01 00:00:00.000', while the time format was '1899-12-30 11:38:36.000'. To rectify this, I adjusted their data types accordingly.

```sql
ALTER TABLE PizzaSales
ALTER COLUMN order_date DATE;

ALTER TABLE PizzaSales 
ALTER COLUMN order_time TIME;
```



<br/><br/>



### Null Values Inspection:

To ensure data integrity, I conducted a thorough check for null values across all columns using the query:



```sql
Select *
from PizzaSales
WHERE pizza_id IS NULL OR 
order_id IS NULL OR 
order_date IS NULL OR
pizza_name_id IS NULL OR
quantity IS NULL OR
order_time IS NULL OR
unit_price IS NULL OR
total_price IS NULL OR
pizza_size IS NULL OR
pizza_category IS NULL OR
pizza_ingredients IS NULL OR
pizza_name IS NULL
```


My analysis revealed no null or duplicated values within the dataset.

<br/><br/>

### Standardizing Pizza Sizes:


Recognizing the importance of standardizing pizza sizes for better visualization, I undertook the task of converting size abbreviations ('S', 'M', 'L', 'XL', 'XXL') into descriptive terms ('Small', 'Medium', 'Large', 'Extra Large', 'Extra Extra Large').

Initially, I inspected all sizes:

```sql
Select DISTINCT (pizza_size)
from PizzaSales
```

Subsequently, I introduced a new column and applied the necessary transformations:


```sql
ALTER TABLE PizzaSales
ADD pizza_sizeslong nvarchar(max);

UPDATE PizzaSales
SET pizza_sizeslong = 
    CASE
        WHEN pizza_size = 'S' THEN 'Small'
        WHEN pizza_size = 'M' THEN 'Medium'
        WHEN pizza_size = 'L' THEN 'Large'
        WHEN pizza_size = 'XL' THEN 'Extra Large'
        WHEN pizza_size = 'XXL' THEN 'Extra Extra Large'
    END;
```




<br/><br/>

Conclusion of Data Cleaning:
Having meticulously refined the dataset, I ensured its cleanliness and robustness, priming it for seamless integration into Power BI for further analysis.


<br/><br/>

## Power BI Enhancements:


To complement the data cleaning efforts, I introduced new measures to fulfill key performance indicators (KPIs):

```DAX
Total Orders = DISTINCTCOUNT(PizzaSales[order_id])
Total Revenue = SUM(PizzaSales[total_price])
Avg Order Value = [Total Revenue] / [Total Orders]
Total Pizzas Sold = SUM(PizzaSales[quantity])
Avg Pizzas Per Order = [Total Pizzas Sold] / [Total Orders]
```


Furthermore, I enriched the data by appending a column to represent the day of the week for each entry, facilitating enhanced visualization.


Additionally, I constructed a conditional column mapping each day name to a corresponding numeric value, aiding in graphically depicting order trends by weekday.

```DAX
Order Day = UPPER(LEFT(PizzaSales[Day Name], 3))
```

This will take the first three letters in the day name column and Upper case them so we can use the days of the week better on our visualizations.

Further transformations included the creation of month and month number columns from the date column, and a month abbreviated column for improved readability.

Lastly, a column was introduced to numerically order pizza sizes for visualization purposes.

This comprehensive approach ensured our dataset was not only clean and structured but also tailored to meet the diverse visualization needs within Power BI.

## Data visualization:

### Summary of Dashboard Insights

#### First Dashboard Page:

1. KPI Cards:

 - Total Revenue
 - Average Order Value
 - Total Pizzas Sold
 - Total Orders
 - Average Pizzas Per Order

2. Area Chart:

 - An area chart illustrating orders over months, providing insights into monthly trends in sales.

3. Stacked Column Chart:

 - A stacked column chart depicting orders by weekday, aiding in understanding weekly sales patterns.

4. Donut Charts:

 - Two donut charts showcasing:
 - Revenue distribution by Pizza Category
 - Revenue distribution by Pizza Size
 - Offering a visual breakdown of revenue sources by category and size.

5. Funnel Chart:

 - A funnel chart representing the total pizzas sold by pizza category, providing a clear visualization of sales funnel dynamics.

6. Interactive Filters and Slicers:

 - Time Slicer: Allows users to select specific time periods for analysis.
 - Filters for Pizza Category and Pizza Size: Enables sorting and filtering data based on pizza categories and sizes.


<br/><br/>

#### Second Dashboard Page:

1. Stacked Column Charts:

- Six stacked column charts presenting:
- Top 5 and bottom 5 performers by Revenue
- Top 5 and bottom 5 performers by Quantity
- Top 5 and bottom 5 performers by Orders
- Offering comparative insights into performance metrics across different categories.

2. Interactive Filters and Slicers:

 - Time Slicer: Continues to provide users with the flexibility to analyze data over specific time periods.
 - Filters for Pizza Category and Pizza Size: Maintains the ability to filter and sort data based on pizza categories and sizes.



<br/><br/>

<br/><br/>

## Dashboard image:


<br/><br/>
![image](https://github.com/DiogoGravanita/Pizza-Sales-Data-Analysis-Dashboard-Project/assets/163042130/b51c81d9-a6f2-458a-b681-30a830be9341)
<br/><br/>
![image](https://github.com/DiogoGravanita/Pizza-Sales-Data-Analysis-Dashboard-Project/assets/163042130/6b49d460-5223-4aee-85f0-2960c4c3c50d)






<br/><br/>
<br/><br/>


## Key Performance Indicators (KPIs):

1. Total Revenue: $817,860
2. Average Order Value: $38.31
3. Total Pizzas Sold: 49,574
4. Total Orders: 21,350
5. Average Pizzas Per Order: 2.32

<br/><br/>

## Charts and Visualizations Findings:


<br/><br/>

### Daily Trend for Total Orders:
Orders peak on Thursdays, Fridays, and Saturdays, averaging around 3.5K. The lowest orders are on Sundays (2.6K) and Saturdays (3.2K).
<br/><br/>

### Monthly Trend for Total Orders:
Peak months are January and March to August, with July recording the highest orders (1,935) and October the lowest (1,646).
<br/><br/>

### Percentage of Sales by Pizza Category:
Classic pizzas lead with 26.91%, followed closely by Supreme (25.46%), Chicken (23.96%), and Veggie (23.68%).
Large pizzas dominate at 46%, followed by Medium (30.5%) and Small (21.7%).
<br/><br/>

### Percentage of Sales by Pizza Size:
Large pizzas dominate at 46%, followed by Medium (30.5%) and Small (21.7%).
<br/><br/>

### Total Pizzas Sold by Pizza Category:
Classic pizzas are the best sellers, with almost 15K sold, followed by other categories with around 11K-12K each.

<br/><br/>

### Top 5 Best Sellers:
By Revenue: Thai Chicken, Barbecue Chicken, Classic Deluxe, Spicy Italian.
By Total Quantity: Classic Deluxe, Barbecue Chicken, Hawaiian, Pepperoni, Thai Chicken.
By Total Orders: Classic Deluxe, Hawaiian, Pepperoni, Barbecue Chicken, Thai Chicken.

<br/><br/>
### Bottom 5 Worst Sellers:
By Revenue: Spinach Pesto, Mediterranean, Spinach Supreme, Green Garden, Brie Carre.
By Total Quantity: Soppressata, Spinach Supreme, Calabrese, Mediterranean, Brie Carre.
By Total Orders: Chicken Pesto, Calabrese, Spinach Supreme, Mediterranean, Brie Carre.

<br/><br/>
## Conclusion:

Overall, the analysis reveals robust sales performance, with clear trends in daily and monthly orders. Classic pizzas emerge as the top-selling category, while the Thai Chicken pizza stands out as a revenue leader. Identifying the best and worst-selling items provides valuable insights for strategic decision-making and product optimization efforts.
