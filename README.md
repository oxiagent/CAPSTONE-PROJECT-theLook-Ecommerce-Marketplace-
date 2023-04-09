# Data-Analyst_Portfolio
### CAPSTONE PROJECT 'theLook Ecommerce Marketplace'

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

The product categories generating the biggest margin are ‘Outerwear & Coats’ and ‘Jeans’: 12.1% and 10.8% in 2019, 13.6% and 10.4% in 2020, 12.8% and 10.4% in 2021, 13% and 10.2% in 2022.
![image4](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/4.png)

Countries generating the biggest margin are ‘China and United States: 32.9% and 22.6% in 2019, 34.2% and 22.9% in 2020, 33.5% and 22.8% in 2021, 34% and 22.3% in 2022.
![image5](https://github.com/oxiagent/Data-Analyst_Portfolio/blob/oxiagent-capstone-project-visualization/5.png)

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

## CUSTOMERS. Life Value















