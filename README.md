# CAPSTONE PROJECT 'theLook Ecommerce Marketplace'


Dataset: [theLook Ecommerce Marketplace](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=thelook_ecommerce&page=dataset&project=wide-maxim-339515&ws=!1m4!1m3!3m2!1sbigquery-public-data!2sthelook_ecommerce)

Data Recourse: [BigQuery Public Data](https://console.cloud.google.com/marketplace/product/bigquery-public-data/thelook-ecommerce?project=wide-maxim-339515)

Dataset Structure: 7 tables (‘distribution_centers’, ‘events’, ‘inventory_items’, ‘order_items’, ‘orders’, ‘products’, ‘users’)

Goal: to analyze business performance of theLook Ecommerce Market. The report consists of analysis of key business metrics, orders delivery, customer segmentation, time life value, retention and behaviour on the website.


## KEY BUSINESS METRICS

theLook is a fashion Ecommerce Market. The project was started in 2019 and since that time it shows great performance. 
In 2020 business Total Revenue increased 245.4%, and next two years growth was more smoothly: 103.4% in 2021 and 107.6% in 2022.
Growth of the Total Margin is on the same level, and general business profitability is around 52%.

![image1](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/1.png)


Such impressive performance can be explained by growth of customers numbers and orders they make. 
In 2021-2022 average purchase value on the ecommerce market place was around $97. In 2019 theLook Revenue per Customer was $95 and it growed up to $111 in 2022.

![image2](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/2.png)

Quarterly graphics of Total Revenue and Total Margin show that business does not depend on seasons. Such result is reached due to the variety of product categories of theLook.
![image3](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/3.png)

Query:
```
WITH 
t1 AS(
  SELECT*, EXTRACT(ISOYEAR FROM DATE(created_at)) as Year, FORMAT_DATE('%Y(%Q)', DATE(created_at)) as Quarter  
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE created_at BETWEEN '2019-01-01 00:00:00 UTC' AND '2022-12-31 23:59:59 UTC'
ORDER BY created_at
)
SELECT --Quarter,
      Year, 
      SUM(sale_price) as TotalRevenue,
      COUNT(DISTINCT order_id) as TotalOrders, 
      COUNT(DISTINCT user_id) as TotalUsers, 
      SUM(sale_price) / COUNT(DISTINCT order_id) as RevernuePerOrder,
      SUM(sale_price) / COUNT(DISTINCT user_id) as RevernuePerUser
FROM t1
GROUP BY 1
ORDER BY 1
```

The product categories generating the biggest margin are ‘Outerwear & Coats’ and ‘Jeans’: 12.1% and 10.8% in 2019, 13.6% and 10.4% in 2020, 12.8% and 10.4% in 2021, 13% and 10.2% in 2022.
![image4](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/4.png)

Query:
```
WITH 
order_items AS(
  SELECT*, EXTRACT(ISOYEAR FROM DATE(created_at)) as Year
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE created_at BETWEEN '2019-01-01 00:00:00 UTC' AND '2022-12-31 23:59:59 UTC'),
maine_table AS(
  SELECT DISTINCT Year, order_items.product_id,products.category,
       products.cost, products.retail_price, products.retail_price - products.cost as margin,
       products.retail_price/products.cost  as profit
FROM order_items
LEFT OUTER JOIN  `bigquery-public-data.thelook_ecommerce.products` as products
ON order_items.product_id = products.id
)
SELECT Year, category,
SUM(margin) as margin
FROM maine_table
GROUP BY Year, category
ORDER BY Year
```

Countries generating the biggest margin are ‘China and United States: 32.9% and 22.6% in 2019, 34.2% and 22.9% in 2020, 33.5% and 22.8% in 2021, 34% and 22.3% in 2022.
![image5](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/5.png)

Query:
```
WITH 
order_items AS(
  SELECT*, EXTRACT(ISOYEAR FROM DATE(created_at)) as Year, FORMAT_DATE('%Y(%Q)', DATE(created_at)) as Quarter  
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE created_at BETWEEN '2019-01-01 00:00:00 UTC' AND '2022-12-31 23:59:59 UTC'
),
main_table AS(
  SELECT order_items.Year, order_items.Quarter, order_items.product_id, order_items.order_id, order_items.user_id, order_items.created_at,sale_price,
       users.id, users.gender, users.country, users.city
FROM order_items
LEFT OUTER JOIN  `bigquery-public-data.thelook_ecommerce.users` as users
ON order_items.user_id = users.id
ORDER BY product_id
)
SELECT 
    Year, 
      country,
      SUM(sale_price) as TotalRevenue,
      COUNT(DISTINCT order_id) as TotalOrders, 
      COUNT(DISTINCT id) as TotalUsers
FROM main_table
GROUP BY 1,2
ORDER BY 1
```

## ORDERS REVIEW

Males generate 53% of Total revenue, females do 47%.

The maximum margin order level is $715.28, the minimum is $0.01.

theLook has big rate of canceled orders 15.1% and 10% of returned orders, which does not overcome the average return level in industry, 18.1% of all orders of fashion markets are returned. 

Business does not have any problem with shipping and delivery. Shipping is up to 4 days, and delivery up to 5 days. 

## ORDERS. Traffic Acquisition

TOP Channel of Traffic Acquisition of Users who make purchasing, is Email.

Adwords is at the 2d position. Search is the biggest traffic channel for attracting new Users. It is a good strategy to be focused on these two channels.
![image13](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/13.png)


## CUSTOMERS. Segmentation

Segmentation is based on RFM (recency, frequency, monetary) analysis. 

Segments’ description*:
Champions are customers, who bought most recently, most often, and are heavy spenders. 
Loyal Customers spend good money and are responsive on promotions.
Potential Loyalists are recent customers with average frequency and who spend a good amount. 
Recent Customers buy most recently but not often.
Promising Customers are recent shoppers who spend not so much.
Customers Needing Attention are those who are above average recency, frequency, and monetary.
About to sleep are those who are below average recency, frequency, and monetary, actions to attract their attention should be taken.
At Risk Customers are those who purchase often and spend big amounts, but haven’t purchased recently. 
Can’t Lose Them are customers who used to visit and purchase quite often, but haven’t been visiting recently. 
Hibernating are customers whose purchase was long back.
Lost are customers with the lowest ecency, frequency, and monetary scores.

*[Recourse](https://towardsdatascience.com/a-simple-way-to-segment-customers-using-google-bigquery-and-data-studio-f31c8896cc52)

The biggest segments are ‘Champions’ (25.5%), ‘Loyal customers’ (19.5%), ‘Potential Loyalists’ (15.6%). It is about 60.6% of all customers, which shows good business performance results and effective marketing strategy. 

The company should focus its attention to customers who Are at Risk (15.9%) and Customers Needing Attention (11.8%). It can be personalized limited time offers, good recommendations according to their interests etc.
![image8](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/8.png)

Query:
```
WITH 
order_items AS(
  SELECT*, EXTRACT(ISOYEAR FROM DATE(created_at)) as Year, FORMAT_DATE('%Y(%Q)', DATE(created_at)) as Quarter  
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE created_at BETWEEN '2019-01-01 00:00:00 UTC' AND '2022-12-31 23:59:59 UTC'
),
t1_0 AS(
  SELECT*,
       products.cost, products.retail_price, products.retail_price - products.cost as margin,
FROM order_items
LEFT OUTER JOIN  `bigquery-public-data.thelook_ecommerce.products` as products
ON order_items.product_id = products.id
),
t1 AS (
  SELECT
    user_id,
    MAX(DATE(created_at)) AS last_purchase_date,
    COUNT(DISTINCT order_id) AS frequency,
    SUM(margin) AS monetary
  FROM t1_0
  GROUP BY user_id
  ORDER BY 3 DESC 
),
t1_1 AS (
  SELECT *,
    DATE_DIFF(reference_date, last_purchase_date, DAY) AS recency
  FROM (
        SELECT  *,
        MAX(last_purchase_date) OVER () AS reference_date
        FROM t1
    )
),
t3 AS (
SELECT 
    a.*,
    b.percentiles[offset(20)] AS m20,
    b.percentiles[offset(40)] AS m40, 
    b.percentiles[offset(60)] AS m60,
    b.percentiles[offset(80)] AS m80, 
    b.percentiles[offset(100)] AS m100,    
    c.percentiles[offset(20)] AS f20,
    c.percentiles[offset(40)] AS f40, 
    c.percentiles[offset(60)] AS f60,
    c.percentiles[offset(80)] AS f80, 
    c.percentiles[offset(100)] AS f100,    
    d.percentiles[offset(20)] AS r20, 
    d.percentiles[offset(40)] AS r40, 
    d.percentiles[offset(60)] AS r60,
    d.percentiles[offset(80)] AS r80, 
    d.percentiles[offset(100)] AS r100
FROM 
    t1_1 a,
    (SELECT APPROX_QUANTILES(monetary, 100) percentiles FROM
    t1_1) b,
    (SELECT APPROX_QUANTILES(frequency, 100) percentiles FROM
    t1_1) c,
    (SELECT APPROX_QUANTILES(recency, 100) percentiles FROM
    t1_1) d
),
t4 AS (
    SELECT *, 
    CAST(ROUND((f_score + m_score) / 2, 0) AS INT64) AS fm_score
    FROM (
        SELECT *, 
        CASE WHEN monetary <= m20 THEN 1
            WHEN monetary <= m40 AND monetary > m20 THEN 2 
            WHEN monetary <= m60 AND monetary > m40 THEN 3 
            WHEN monetary <= m80 AND monetary > m60 THEN 4 
            WHEN monetary <= m100 AND monetary > m80 THEN 5
        END AS m_score,
        CASE WHEN frequency <= f20 THEN 1
            WHEN frequency <= f40 AND frequency > f20 THEN 2 
            WHEN frequency <= f60 AND frequency > f40 THEN 3 
            WHEN frequency <= f80 AND frequency > f60 THEN 4 
            WHEN frequency <= f100 AND frequency > f80 THEN 5
        END AS f_score,
        CASE WHEN recency <= r20 THEN 5
            WHEN recency <= r40 AND recency > r20 THEN 4 
            WHEN recency <= r60 AND recency > r40 THEN 3 
            WHEN recency <= r80 AND recency > r60 THEN 2 
            WHEN recency <= r100 AND recency > r80 THEN 1
        END AS r_score,
        FROM t3
        )
),
t5 AS(
SELECT 
        user_id,
        recency,
       frequency, 
        monetary,
        r_score,
        f_score,
        m_score,
        fm_score,
        CASE WHEN (r_score = 5 AND fm_score = 5) 
            OR (r_score = 5 AND fm_score = 4) 
            OR (r_score = 4 AND fm_score = 5) 
        THEN 'Champions'
        WHEN (r_score = 5 AND fm_score =3) 
            OR (r_score = 4 AND fm_score = 4)
            OR (r_score = 3 AND fm_score = 5)
            OR (r_score = 3 AND fm_score = 4)
        THEN 'Loyal Customers'
        WHEN (r_score = 5 AND fm_score = 2) 
            OR (r_score = 4 AND fm_score = 2)
            OR (r_score = 3 AND fm_score = 3)
            OR (r_score = 4 AND fm_score = 3)
        THEN 'Potential Loyalists'
        WHEN r_score = 5 AND fm_score = 1 THEN 'Recent Customers'
        WHEN (r_score = 4 AND fm_score = 1) 
            OR (r_score = 3 AND fm_score = 1)
        THEN 'Promising'
        WHEN (r_score = 3 AND fm_score = 2) 
            OR (r_score = 2 AND fm_score = 3)
            OR (r_score = 2 AND fm_score = 2)
        THEN 'Customers Needing Attention'
        WHEN r_score = 2 AND fm_score = 1 THEN 'About to Sleep'
        WHEN (r_score = 2 AND fm_score = 5) 
            OR (r_score = 2 AND fm_score = 4)
            OR (r_score = 1 AND fm_score = 3)
        THEN 'At Risk'
        WHEN (r_score = 1 AND fm_score = 5)
            OR (r_score = 1 AND fm_score = 4)        
        THEN 'Cant Lose Them'
        WHEN r_score = 1 AND fm_score = 2 THEN 'Hibernating'
        WHEN r_score = 1 AND fm_score = 1 THEN 'Lost'
        END AS rfm_segment 
    FROM t4
),
t6 AS(
SELECT
        t1.user_id,
        t1_1.recency,
        t1.frequency,
        ROUND(t1.monetary, 2) as monetary,
        t5.r_score,
        t5.f_score,
        t5.m_score,
        t5.fm_score,
        t5.rfm_segment 
FROM t1
JOIN t1_1
ON t1.user_id = t1_1.user_id
JOIN t5
ON t1.user_id = t5.user_id
)
SELECT rfm_segment, ROUND(SUM(monetary)) as CustomersInSegment
--COUNT(DISTINCT CustomerID)
FROM t6
GROUP BY 1
ORDER BY 2 DESC
```

## CUSTOMERS. Life Value

Customer life value is calculated on Margin per Customer.
One cohort = 1 month. 

Steps:

Calculate cohorts for the period Jan, 2022 - Dec, 2022: Average Margin per Customer 
![image7_1](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/7_1.png)

2. Calculate Average, Cumulative Average and Cumulative Growth
![image7_2](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/7_2.png)

3. Calculate Cumulative SUM and sales Forecast
![image7_3](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/7_3.png)

Customer Life Value (gross margin) is about $186

Query:
```
WITH 
order_items AS(
  SELECT*, EXTRACT(ISOYEAR FROM DATE(created_at)) as Year, FORMAT_DATE('%Y(%Q)', DATE(created_at)) as Quarter,
  MIN(DATE(created_at)) OVER (PARTITION BY   user_id)  AS first_purchase_date,
  MAX(DATE(created_at)) OVER (PARTITION BY   user_id) AS last_purchase_date  
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE created_at BETWEEN '2019-01-01 00:00:00 UTC' AND '2022-12-31 23:59:59 UTC'),
main_table AS(
  SELECT order_items.Year, order_items.Quarter,order_items.id, order_items.first_purchase_date, order_items.last_purchase_date, order_items.product_id, order_items.order_id, products.id, order_items.user_id, order_items.created_at,sale_price,
       products.cost, products.retail_price, products.retail_price - products.cost as margin
FROM order_items
LEFT OUTER JOIN  `bigquery-public-data.thelook_ecommerce.products` as products
ON order_items.product_id = products.id
ORDER BY product_id
),
cohorts AS(
  SELECT*, CASE 
           WHEN Quarter BETWEEN "2019(1)" AND "2019(2)" THEN 'cohort1'
           WHEN Quarter BETWEEN "2019(3)" AND "2019(4)" THEN 'cohort2'
           WHEN Quarter BETWEEN "2020(1)" AND "2020(2)" THEN 'cohort3'
           WHEN Quarter BETWEEN "2020(3)" AND "2020(4)" THEN 'cohort4'
           WHEN Quarter BETWEEN "2021(1)" AND "2021(2)" THEN 'cohort5'
           WHEN Quarter BETWEEN "2021(3)" AND "2021(4)" THEN 'cohort6'
           WHEN Quarter BETWEEN "2022(1)" AND "2022(2)" THEN 'cohort7'
           WHEN Quarter BETWEEN "2022(3)" AND "2022(4)" THEN 'cohort8'
           END as cohorts_
FROM main_table
)
SELECT 
      cohorts_,
      COUNT (DISTINCT user_id) as TotalUsers,
      SUM(margin)  / COUNT (DISTINCT user_id) as cohort_0,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 2 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 2 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_1,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 4 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 4 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_2,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 6 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 6 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_3,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 8 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 8 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_4,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 10 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 10 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_5,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 12 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 12 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_6,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 14 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 14 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_7,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 16 quarter) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 16 quarter) OR last_purchase_date IS NULL THEN margin END)  / COUNT (DISTINCT user_id) as cohort_8
FROM cohorts
GROUP BY 1
ORDER BY 1
```


## CUSTOMERS. Retention Rate

One cohort = 1 month. 

Steps:
Calculate cohorts for the whole years 2020, 2021, 2022: Number of Customers who make purchases 

Every next year situation with Retention Rate is improving. Average market Retention Rate value is around 18%. The situation of theLOOK is on the average level, which is good result.
![image6](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/6.png)

Query:
```
WITH 
order_items AS(
  SELECT*, 
  MIN(DATE(created_at)) OVER (PARTITION BY   user_id)  AS first_purchase_date,
  MAX(DATE(created_at)) OVER (PARTITION BY   user_id) AS last_purchase_date  
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE created_at BETWEEN '2022-01-01 00:00:00 UTC' AND '2022-12-31 23:59:59 UTC'),
main_table AS(
  SELECT DATE_TRUNC(first_purchase_date, month) as month,
  order_items.id, order_items.first_purchase_date, order_items.last_purchase_date, order_items.product_id, order_items.order_id, products.id, order_items.user_id, order_items.created_at,sale_price,
       products.cost, products.retail_price, products.retail_price - products.cost as margin
FROM order_items
LEFT OUTER JOIN  `bigquery-public-data.thelook_ecommerce.products` as products
ON order_items.product_id = products.id
ORDER BY product_id
)
SELECT 
      month,
      COUNT (user_id) as month_0,
      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 1 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 1 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_1,
        SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 2 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 2 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_2,
          SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 3 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 3 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_3,
            SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 4 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 4 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_4,
              SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 5 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 5 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_5,
                SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 6 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 6 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_6,
                  SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 7 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 7 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_7,
                    SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 8 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 8 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_8,
                      SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 9 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 9 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_9,
                        SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 10 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 10 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_1,
                          SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 11 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 11 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_10,
                            SUM(CASE WHEN first_purchase_date < DATE_ADD(first_purchase_date, INTERVAL 12 month) AND last_purchase_date >= DATE_ADD(first_purchase_date, INTERVAL 12 month) OR last_purchase_date IS NULL THEN 1 ELSE 0 END) as month_11,
     
FROM main_table
GROUP BY 1
ORDER BY 1
```


## CUSTOMERS 

### Demographics

Male and female have equal parts: 50% and 50%. 
About 68% of customers (male and female) are almost equally placed in age groups: 18-24y.o., 25-34y.o., 35-44y.o., 45-54y.o., 55-64y.o.

About 71% of customers (male and female) are from China, United States, and Brasil

Query:
```
WITH
t1 AS(
  SELECT*, 
      CASE 
           WHEN age < 18 THEN 'under 18y.o.'
           WHEN age BETWEEN 18 AND 24 THEN '18-24y.o.'
           WHEN age BETWEEN 25 AND 34 THEN '25-34.o.'
           WHEN age BETWEEN 35 AND 44 THEN '35-44y.o.'
           WHEN age BETWEEN 45 AND 54 THEN '45-54y.o.'
           WHEN age BETWEEN 55 AND 64 THEN '55-64.o.'
           WHEN age >= 65 THEN '65y.o. and older'
           END as age_group
FROM `bigquery-public-data.thelook_ecommerce.users` 
WHERE gender = "M"
--WHERE gender = "F"
)
SELECT 
--  traffic_source,
 --  country,
    age_group, 
     COUNT(*) as users_amount
FROM t1
GROUP BY 1
ORDER BY 1
```

### Traffic Acquisition

The biggest channel of customers acquisition is Search (for female and male), about 70%. 


![image10](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/10.png)

![image9](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/9.png)

## CUSTOMERS. Funnel. User Behaviour on the website

The Funnel of purchases consist of 3 stages:
- Users view a Product Page. 70.4% of Users add an item to a cart on this step. Drop-off rate is 29.6%.
- Users open a cart. 21.5% proceed with purchasing. Drop-off rate is 48.9%
- Purchase is made

![image11](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/11.png)

Query:
```
SELECT  event_type, COUNT(event_type) as amount
FROM `bigquery-public-data.thelook_ecommerce.events` 
GROUP BY 1
ORDER BY 2 DESC
```






















