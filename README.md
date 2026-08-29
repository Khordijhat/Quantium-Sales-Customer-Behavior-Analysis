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
```SQL
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
```
## Data Cleaning Examples
Some product names contained inconsistent abbreviations or spelling variations.

For example:

•	GrnWves → Grain Waves

•	Infz → Infuzions

•	NNC → Natural Chip Co

•	RRD → Red Rock Deli

Standardizing these values made it possible to accurately group products and brands during analysis.
## 📊 Analysis & Visualizations
### 1. Monthly Sales Trend
A monthly revenue trend was created to understand how sales changed throughout the observation period.

#### Purpose:

•	Identify periods of high and low sales

•	Observe changes in demand over time

•	Support sales and inventory planning
### 2. Top Sales by Revenue
Products/categories were ranked according to their total revenue contribution.

####Purpose:

•	Identify major revenue contributors

•	Understand which products drive sales

•	Support product and promotional decisions
### 3. Revenue by Product Level
Revenue was analyzed across individual product categories and product lines.

#### Purpose:

•	Identify high-performing products

•	Compare product-level performance

•	Understand which products contribute most to overall revenue
### 4. Top 10 Brands by Revenue
The top 10 brands were compared based on total revenue after standardizing brand names.

#### Purpose:

•	Identify the strongest-performing brands

•	Understand consumer brand preferences

•	Highlight brands contributing significantly to overall revenue
### 5. Revenue by Product Weight
Products were grouped into three weight categories:

•	Low Weight: ≤150g

•	Medium Weight: 151–250g

•	High Weight: >250g

Revenue was then compared across the different weight categories.
####Purpose:

•	Understand preferred pack sizes

•	Identify which weight categories generate more revenue

•	Provide insights that can support packaging and pricing decisions
### 6. Revenue by Customer Lifestage
Customer spending was segmented by lifestage to understand differences in purchasing behavior.
The analysis considered customer groups such as:

•	Young Singles/Couples

•	Older Families

•	Retirees

•	Other customer lifestages
#### Purpose:
•	Identify high-value customer segments

•	Understand differences in spending behavior

•	Support more targeted marketing strategies





