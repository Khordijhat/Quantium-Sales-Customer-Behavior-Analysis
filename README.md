# Quantium-Sales-Customer-Behavior-Analysis
## Project overview
This project is an end-to-end analysis of Quantium transaction data, focused on understanding sales performance, product trends, and customer purchasing behavior.

The analysis focuses on:

•	Sales performance over time

•	Product and brand performance

•	Product weight preferences

•	Customer purchasing behavior

•	Customer lifestage segments

•	Revenue contribution across different categories
## Data Workflow
The project followed a two-stage data workflow:

### 1. SQL: Data Cleaning & Preparation

MySQL was used to prepare and transform the raw transaction data before visualization.

Key tasks included:

•	Joining transaction and customer behavior tables using loyalty_card_number

•	Standardizing inconsistent product and brand names

•	Extracting brand names from product descriptions

•	Extracting product weights from product names using regular expressions

•	Categorizing products into weight groups

•	Combining transaction information with customer attributes

### 2. Excel: Analysis & Visualization

The cleaned dataset was then analyzed in Microsoft Excel.

Key tasks included:

•	Extracting Month and Year from transaction dates

•	Cleaning and standardizing categorical values

•	Creating Pivot Tables

•	Aggregating revenue, transactions, and units sold

•	Creating charts and visual reports

•	Identifying trends and patterns across products and customer segments
## SQL Data Cleaning
The transaction and customer behavior tables were joined using the customer's loyalty card number.

SQL
SELECT 
    Transaction_date,
    product_name,
    P.loyalty_card_number,
    TXN,
    Store_nbr,
    Total_sales,
    CASE 
        WHEN product_name LIKE 'Dorit%' THEN 'Doritos'
        WHEN product_name LIKE 'GrnWves%' THEN 'Grain Waves'
        WHEN product_name LIKE 'Infz%' THEN 'Infuzions'
        WHEN product_name LIKE 'NNC%' THEN 'Natural Chip Co'
        WHEN product_name LIKE 'RRD%' THEN 'Red Rock Deli'
        WHEN product_name LIKE 'smith%' THEN 'Smiths'
        WHEN product_name LIKE 'sn%' THEN 'Sunbites'
        WHEN Product_name LIKE 'ww%' THEN 'Woolworths'
        ELSE SUBSTRING_INDEX(product_name, ' ', 1) 
    END AS Brand_name,
    CASE 
        WHEN CAST(REGEXP_SUBSTR(product_name, '[0-9]+g') AS UNSIGNED) <= 150 THEN 'Low weight'
        WHEN CAST(REGEXP_SUBSTR(product_name, '[0-9]+g') AS UNSIGNED) <= 250 THEN 'Medium weight'
        ELSE 'High weight'
    END AS Weight_category,
    lifestage,
    premium_customer
FROM Transaction_data T
JOIN purchased_behaviour P
    ON T.loyalty_card_number = P.loyalty_card_number;






