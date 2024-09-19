# Istanbul Customer Shopping Analysis



## Summary
The analysis of shopping data from Istanbul’s malls provides valuable insights for potential investors or businesses looking to enter the retail market. The Mall of Istanbul emerged as the highest revenue generator, with total sales surpassing 50 million. It is closely followed by Kanyon, while Metrocity, Metropol AVM, and Istinye Park also ranked among the top-performing malls in terms of revenue. These malls are evidently prime locations with strong consumer activity.

In terms of product categories, Clothing stands out as the most popular, with over 100,000 units sold. Other high-demand categories include Cosmetics, Food & Beverage, and Toys. The dominance of clothing sales indicates that fashion remains a primary focus for shoppers across the city’s malls.

Regarding payment methods, cash is the most frequently used, accounting for 44.86% of transactions. This is followed by credit cards at 35.02% and debit cards at 20.12%. These figures suggest that businesses should ensure the availability of multiple payment options to meet customer preferences.

Demographic data reveals that female customers account for 59.72% of the shopper base, with male customers comprising 40.28%. This demographic insight can guide targeted marketing strategies and product offerings, ensuring they align with the predominant customer base.

Lastly, the busiest shopping months in Istanbul were July, October, and March, with these months showing the highest number of orders. This highlights potential periods of increased shopping activity, which businesses can capitalize on through promotions or targeted campaigns.

## Recomendations
- Choose a High-Traffic Location: Consider opening in areas near successful malls like Mall of Istanbul or Kanyon, as they have the highest revenues and foot traffic.

- Focus on Clothing and Fashion: Clothing is the most popular product category. Stock up on apparel and related items to attract the largest share of shoppers.

- Offer Flexible Payment Options: Although many customers prefer cash, ensure you also provide card and mobile payment options to cater to all preferences.

- Cater to Female Shoppers: With women making up nearly 60% of customers, tailor your product offerings and marketing strategies toward female shoppers.

- Plan for Peak Seasons: July, October, and March are the busiest shopping months. Plan promotions and stock inventory to take advantage of these high-traffic periods.


## Table of Contents

- [Objective](#objective)
- [Data source](#data-source)
- [Tools](#tools)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Findings](#findings)
- [Key Takeaways for New Entrants](#key-takeaways-for-new-entrants)


### Objective
The objective of this project is to analyze shopping trends across 10 major malls in Istanbul from 2021 to 2023. By examining key factors such as sales revenue, product preferences, payment methods, customer demographics, and seasonal shopping patterns, the goal is to provide valuable insights that will assist potential investors and businesses in making informed decisions about entering Istanbul’s retail market. The analysis aims to guide new entrants in selecting the best locations, optimizing product offerings, targeting the right customer demographics, and planning for peak shopping periods.

### Data source
Customer Shopping Dataset - Retail Sales Data.
- [Download here](https://www.kaggle.com/datasets/mehmettahiraslan/customer-shopping-dataset).

This dataset contains shopping information from 10 different shopping malls between 2021 and 2023. The data was gathered from various age groups and genders to provide a comprehensive view of shopping habits in Istanbul. The dataset includes essential information such as invoice numbers, customer IDs, age, gender, payment methods, product categories, quantity, price, order dates, and shopping mall locations.


### Tools
- Excel - Data validation
  - [Download here](https://www.kaggle.com/datasets/mehmettahiraslan/customer-shopping-dataset)
- Postgresql - Data analysis
- Tableau - Creating Reports

### Data Validation
1. Chekening for duplicates
2. Checking date format

### Exploratory Data Analysis
- Which shopping mall has the highest sales revenue?
- What are the most popular Product categories by quantity?
- What are the most popular product categories across different malls?
- Which age_range are the most active shoppers?
- Which payment methods are most preferred by customers?
- What were the busiest Months for shopping in Istanbul malls (2021-2022)
- What is the average spending per customer, and how does it vary by mall and product category?
- Which product categories should new malls prioritize based on total customer spending?

```sql
-- Which shopping mall has the highest sales revenue?
SELECT 
    shopping_mall, 
    SUM(quantity * price) AS total_revenue
FROM 
    customer_shopping_data
GROUP BY 
    shopping_mall
ORDER BY 
    total_revenue DESC;

-- Most popular Product categories by quantity
SELECT
    category,
    SUM (quantity) AS total_quantity_sold
FROM
    customer_shopping_data
GROUP BY
    category
ORDER BY
    total_quantity_sold DESC;

-- What are the most popular product categories across different malls?
SELECT 
    shopping_mall,
    category,
    SUM (quantity) AS total_quantity_sold
FROM
    customer_shopping_data
GROUP BY
    shopping_mall,
    category        
ORDER BY
    total_quantity_sold DESC;

-- Which age_range are the most active shoppers 
SELECT 
    CASE
        WHEN age BETWEEN 0 AND 17 THEN '0-17'
        WHEN age BETWEEN 18 AND 29 THEN '18-29'
        WHEN age BETWEEN 30 AND 44 THEN '30-44'
        WHEN age BETWEEN 45 AND 59 THEN '45-59'
        ELSE '60+'
    END AS age_range,
    SUM (quantity) as quantity_bought
FROM customer_shopping_data
GROUP BY
    age_range
ORDER BY
    quantity_bought DESC;

 -- Which payment methods are most preferred by customers?
SELECT 
    payment_method, 
    COUNT(*) AS method_usage
FROM 
    customer_shopping_data
GROUP BY 
    payment_method
ORDER BY 
    method_usage DESC;

-- What were the busiest months for shopping in Istanbul malls (2021-2022)?
WITH orders_by_month AS
(
SELECT
   EXTRACT (month from invoice_date) AS MONTHS,
   COUNT (*) as No_of_orders
FROM
    customer_shopping_data
WHERE
    EXTRACT (YEAR FROM invoice_date) BETWEEN 2021 and 2022     
GROUP BY
    MONTHS
ORDER BY
    No_of_orders DESC
)

SELECT
    CASE
        WHEN months = 1 THEN 'January'
        WHEN months = 2 THEN 'February'
        WHEN months = 3 THEN 'March'
        WHEN months = 4 THEN 'April'
        WHEN months = 5 THEN 'May'
        WHEN months = 6 THEN 'June'
        WHEN months = 7 THEN 'July'
        WHEN months = 8 THEN 'August'
        WHEN months = 9 THEN 'September'
        WHEN months = 10 THEN 'October'
        WHEN months = 11 THEN 'November'
        WHEN months = 12 THEN 'December'
    END AS MONTH_,
    No_of_orders
FROM
    orders_by_month
ORDER BY
    No_of_orders DESC;

-- What is the average spending per customer, and how does it vary by mall and product category?
SELECT 
    shopping_mall,
    category, 
    AVG(quantity * price) AS avg_spending_per_customer
FROM 
    customer_shopping_data
GROUP BY 
    shopping_mall, category
ORDER BY 
    avg_spending_per_customer DESC;

-- Which product categories should new malls prioritize based on total customer spending?
SELECT 
    category, 
    SUM(quantity * price) AS total_spending
FROM 
    customer_shopping_data
GROUP BY 
    category
ORDER BY 
    total_spending DESC;
```

### Findings
#### 1. Top-Performing Shopping Malls by Revenue
The Mall of Istanbul leads in terms of total sales revenue, generating over 50 million during the analyzed period. Following closely is Kanyon, with revenues just slightly behind. Other malls like Metrocity, Metropol AVM, and Istinye Park also show strong performance, with significant revenues ranging from 24 to 37 million. These results indicate that these malls are the most successful in attracting customers and driving sales, making them key locations for businesses looking to establish a presence in Istanbul.

These high-revenue malls are likely benefiting from a combination of factors, such as prime location, diverse product offerings, and effective marketing strategies. Their performance also suggests that consumer spending in these malls is relatively high, making them potential benchmarks for any new mall or retail business in Istanbul.

#### 2. Dominant Product Categories
The data clearly shows that Clothing is the most purchased product category, with over 100,000 units sold. This is far ahead of the next most popular categories, Cosmetics, Food & Beverage, and Toys, each of which ranges between 30,000 to 45,000 units sold. The strong demand for clothing reflects the city’s fashion-forward consumer base and indicates that apparel should be a focus for any new retail ventures.

Other product categories such as Shoes, Technology, Books, and Souvenirs also see steady sales, but the significant lead held by clothing suggests that businesses catering to fashion-conscious consumers will have the most success.

#### 3. Customer Preferences for Payment Methods
In terms of payment preferences, cash is still the most widely used payment method, accounting for 44.86% of all transactions. However, credit card usage is also substantial, representing 35.02% of payments, followed by debit cards at 20.12%. These figures suggest that while a large portion of consumers prefers the convenience of cash, there is also a significant number of customers who use digital or card-based payments.

For businesses looking to enter this market, it is essential to ensure the availability of flexible payment options to accommodate different customer preferences. Offering multiple payment methods, including contactless and mobile payments, could enhance the shopping experience and capture a broader customer base.

#### 4. Customer Demographics: Gender Insights
A significant finding is that 59.72% of shoppers in these malls are female, while 40.28% are male. This demographic split indicates that women represent a larger share of the consumer base, which could influence product offerings, marketing strategies, and store design.

Understanding the preferences and behavior of this majority group is key to success. Retailers may want to focus on product categories that appeal more to female consumers, such as clothing, cosmetics, and accessories, and tailor marketing campaigns to attract this demographic.

#### 5. Busiest Months for Shopping Activity
Seasonal trends also play a critical role in shopping behaviors. The months of July, October, and March were the busiest, with the highest number of transactions. These months represent peak shopping periods, likely driven by a combination of factors such as sales events, holidays, and seasonal promotions. Other months such as May, January, and December also saw substantial shopping activity.

For businesses, understanding these peak shopping periods is vital for planning inventory, staffing, and marketing campaigns. Targeting promotions and special offers around these busy months can help maximize sales and capture more consumer attention during high-traffic times.

### Key Takeaways for New Entrants:
- Location: Malls like the Mall of Istanbul and Kanyon are prime locations due to their high sales revenues. Any new venture should consider proximity to these high-performing areas or adopt similar strategies.

- Product Mix: Clothing is by far the most popular product category, so businesses should consider focusing on fashion and apparel. However, there are also opportunities in cosmetics, food, and toys, which see strong demand.

- Customer Preferences: While cash is still the dominant payment method, nearly 55% of customers prefer card-based payments, making it essential for businesses to offer flexible payment options.

- Target Demographic: With women making up the majority of shoppers, businesses should ensure their offerings are appealing to this demographic. Fashion, beauty, and lifestyle products may perform particularly well.

- Seasonal Planning: The peak shopping months of July, October, and March should be leveraged for marketing campaigns, product launches, and promotions. These are the best times to drive higher sales and attract more customers.

By focusing on these insights, new retail ventures in Istanbul can position themselves for success in a competitive market, ensuring they cater to the right audience, at the right time, with the right products.















