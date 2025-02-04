# data-engineering-1

This repository contains our work for the ECBS 5146 Data Engineering 1: SQL and Different Shapes of Data course.
The Term Project 2 uses data from the Brazilian E-Commerce Public Dataset by Olist.
Project Title: Brazilian E-Commerce Public Dataset Analysis

## Usage Instructions

0. Prerequisites:

   - Knime

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ayazhantn/data-engineering-1

 # Table of Contents
1. [Repository Structure](#repository-structure)
2. [Project Overview](#project-overview)
3. [Data Description](#data-description)
4. [Research Questions](#research-questions)
5. [ETL Pipeline Design](#etl-pipeline-design)
6. [Visualizations and Answers to Research Questions](#visualizations-and-answers-to-research-questions)
7. [Conclusion and Future Work](#conclusion-and-future-work)

# Repository Structure

The repository is organized as follows:

**`data-engineering-1/`**
- **`Term2/`**: Main project folder.
  - **`Knime_TermProject/`**: Contains all pipelines and required files to reproduce the project. 
  - **`data/`**: Contains CSV files with raw data.
    - `olist_customers_dataset.csv`: Data related to clients.
    - `olist_geolocation_dataset.csv`: Data related to geolocation.
    - `olist_order_items_dataset.csv`: Data related to ordered items.
    - `olist_order_payments_dataset.csv`: Data related to payments.
    - `olist_order_reviews_dataset.csv`: Data related to reviews on orders.
    - `olist_orders_dataset.csv`: Data related to orders.
    - `olist_products_dataset.csv`: Data related to products.
    - `olist_sellers_dataset.csv`: Data related to sellers on the e-commerce platform.
    - `product_category_name_translation.csv`: English translations of product categories.
  - **`script/`**: Contains Python code and Knime workflow used in the project.
    - `mongo.ipynb`: Python script for importing data.
  - **`EER.jpeg`**: EER.
  - **`Knime_workflow.jpeg`**: ETL Pipeline in Knime.
  - **`visuals/`**: Contains visualisations answering Research Questions.
    - `Q1Chart.jpeg`: Chart for Q1.
    - `Q2Chart.jpeg`: Chart for Q2.
    - `Q3ChartA.jpeg`: Chart A for Q3.
    - `Q3ChartB.jpeg`: Chart B for Q3.
    - `Q4Chart.jpeg`: Chart for Q4.
  - **`Report_Team1.pdf`**: The documentation file.
  - **`Presentation_Team1.pdf`**: Presentation file.

- **`README.md`**: This file contains more expanded version of the documentation for the project. 

 # Project Overview

The goal of this project is to apply knowledge and skills learned during the Data Engineering 1: SQL and Different Shapes of Data course. The project involves building an ETL pipeline in Knime, setting research questions and analyzing them to get insights about customer behavior and sales trends from the Brazilian E-Commerce Dataset.

 # Data Description
Source: Kaggle - [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

The dataset contains details of 100,000 orders placed across various marketplaces in Brazil between 2016 and 2018.
 
Tables:

olist_customers_dataset.csv table contains information on clients:
| Column                   | Description                                             |
|--------------------------|---------------------------------------------------------|
| customer_id              | Key to the orders dataset. Each order has a unique customer_id. |
| customer_unique_id       | Unique identifier for the customer.    |
| customer_zip_code_prefix | First five digits of customer zip code.  |
| customer_city            | City associated with the customer's location.          |
| customer_state           | State associated with the customer's location.         |


olist_geolocation_dataset.csv table contains information about geolocation.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| customer_zip_code_prefix   | First five digits of customer zip code.  |
| geolocation_lat            | Latitude of the location.                        |
| geolocation_lng            | Longitude of the location.                       |
| geolocation_city           | City name.               |
| geolocation_state          | State name.              |

olist_order_items_dataset.csv table contains information about ordered items.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| order_id                   | Unique identifier for the order.                        |
| order_item_id | sequential number identifying number of items included in the same order. |
| product_id            | Unique identifier for the product.                  |
| seller_id           | Unique identifier for the seller.         |
| shipping_limit_date | The seller's shipping limit date for handling the order over to the logistic partner. |
| price          | Item price.              |
| freight_value          | Item freight value item (if an order has more than one item the freight value is splitted between items).     |

olist_order_payments_dataset.csv table contains information about payments.
| Column                     | Description                                              |
|----------------------------|----------------------------------------------------------|
| order_id                   | Unique identifier for the order.                        |
| payment_sequential         | A customer may pay an order with more than one payment method. If he does so, a sequence will be created to accommodate all payments.   |
| payment_type               | Method of payment chosen by the customer.    |
| payment_installments       | Number of installments chosen by the customer.          |
| payment_value              | Transaction value.                             |

olist_order_reviews_dataset.csv table contains reviews on orders.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| review_id   | Unique identifier for the review.   |
| order_id            | Unique identifier for the order.                          |
| review_score  | Note ranging from 1 to 5 given by the customer on a satisfaction survey.  |
| review_comment_title | Comment title from the review left by the customer, in Portuguese. |
| review_comment_message | Comment message from the review left by the customer, in Portuguese. |
| review_creation_date  | Shows the date in which the satisfaction survey was sent to the customer.  |
| review_answer_timestamp  | Shows satisfaction survey answer timestamp.  |

olist_orders_dataset.csv table contains information about orders.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| order_id            | Unique identifier for the order.                             |
| customer_id  | Key to the customer dataset. Each order has a unique customer_id. |
| order_status           | Reference to the order status (delivered, shipped, etc).  |
| order_purchase_timestamp          | Shows the purchase timestamp.    |
| order_approved_at  | Shows the payment approval timestamp.   |
| order_delivered_carrier_date     | Shows the order posting timestamp. When it was handled to the logistic partner. |
| order_delivered_customer_date  | Shows the actual order delivery date to the customer. |
| order_estimated_delivery_date| Shows the estimated delivery date that was informed to customer at the purchase moment. |

olist_products_dataset.csv table contains information about products.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| product_id            | Unique identifier for the product.                            |
| product_category_name            | Root category of product, in Portuguese.              |
| product_name_length           | Number of characters extracted from the product name.     |
| product_description_length  | Number of characters extracted from the product description. |
| product_photos_qty | Number of published photos of the product.    |
| product_weight_g     | Product weight measured in grams.              |
| product_length_cm  | Product length measured in centimeters.              |
| product_height_cm | Product height measured in centimeters.              |
| product_width_cm | Product width measured in centimeters.              |

olist_sellers_dataset.csv table contains information about sellers on the ecommerce platform.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| seller_id            | Unique identifier for the seller.                        |
| seller_zip_code_prefix          | First five digits of customer zip code.  |
| seller_city           | Seller city name.          |
| seller_state          | Seller state.        |

product_category_name_translation.csv table contains translations of product categories.
| Column                     | Description                                                  |
|----------------------------|--------------------------------------------------------------|
| product_category_name            | Category name in Portuguese.                        |
| product_category_name_english    | Category name in English.                       |

![EER](Term2/EER.jpeg)

# ETL Pipeline Design
The ETL pipeline involves different steps:

Data Import and Joining, Enrichment

- MongoDB Reader: Reads data from the MongoDB database, allowing us to retrieve information from the various collections.
- JSON to Table: Converts data in JSON format into structured tabular data for making the analysis easier.
- GeoFile Reader: Reads geojson file of Brazil's states for further visualization. Geospatial Analytics extension was installed.
- Joiner: Combines two data tables based on specified matching columns.
- String Manipulation, POST Request and JSON Path nodes were used for enriching data with Google Cloud NL API: "review_comment_messages" column is used for sentiment analysis of the customer's comments on orders. Google Cloud Natural Language API is used for sentiment scoring ranging from -1.0 (negative) to 1.0 (positive).  

Data Transformation and Cleaning

- Filtering, manipulation, grouping, sampling, sorting and difference calculation (e.g. Date&Time Difference) nodes were used depending on the Research Question analysis demand.
- Question 1: after keeping observations only with comments of customers on orders, the dataset included approximately 41,000 observations. A representative sample of 3,134 observations was obtained using a 98% confidence level and a 2% margin of error. A representative sample was used to save time and resourses in account of Google Cloud NL API.
- Question 2: duplicates were removed, date columns were formatted, and orders were aggregated by seasons based on months which helped to find seasonal differences.
- Question 3: after data transformation and cleaning, product categories were analyzed. Through sorting and sampling 10 most and 10 least popular categories were identified.
- Question 4: delivery times (days) were calculated by comparing order and delivery dates, and joined with geospatial data to examine trends. Geospatial Analytics extension was used for reading geo json files and visualization of maps.


Analysis and Visualization

- Bar Chart (JavaScript), Rule Engine, Geospatial View were used to visualize trends and geographic patterns in the data and apply logical rules for categorization. As a result, clear representations of trends and research question answers are obtained.

![Knime_workflow](Term2/Knime_workflow.jpeg)

# Research Questions
Q1. How does the delivery performance (on-time vs. delayed) affect customer sentiment in Brazilian e-commerce?

Q2. Is there a clear seasonal trend in order volume? How to promote off-season sales?

Q3. Which categories perform best and worst nationwide?

Q4. Are there noticeable regional differences in the delivery periods?


# Visualizations and Answers to Research Questions

Q1. How does the delivery performance (on-time vs. delayed) affect customer sentiment in Brazilian e-commerce?

![Q1Chart](Term2/visuals/Q1Chart.jpeg)

The bar chart for delivery performance shows that "On-Time" deliveries have higher sentiment scores compared to "Delayed" deliveries. This is indication of the on-time deliveries' positive impact on customer satisfaction. Therefore, sellers might want to avoid delays which mostly negatively affect the reviews they get. While the average sentiment of delayed orders is not strongly negative, it suggests that customers may have minor complaints, frustrations, or unmet expectations. 

Q2. Is there a clear seasonal trend in order volume? How to promote off-season sales?

![Q2Chart](Term2/visuals/Q2Chart.jpeg)

From the bar chart, we can see the differences in sales volumes across different seasons：
- Winter has the highest order volume. This might be due to holiday season's higher demand (e.g., Christmas or New Year).
- Autumn follows closely to Winter.
- Summer has a moderate order volume, lower than both Autumn and Winter but higher than Spring season. This reflects consumer demand in summertime, which might be affected by holidays, travelling, trips.
- Spring has the lowest order volume and the difference from other seasons is significant. This shows lower consumer demand.

Critical seasons for businesses, namely Autumn and Winter should probably be planned in advance, while lower orders in Spring and Summer seasons might require sellers to adopt strategies to boost sales. Among the ways of improving the sales performances there are seasonal promotions, marketing campaigns depending on seasonal differences. However, in order to identify more efficient pathways to increase order volumes, additional analytics can be carried out: checking if the seasonal differences persist over the years (2016, 2017 and 2018), finding out which product categories contribute to orders numbers more in specific seasons and seeing if seasonal differences are nationwide or vary geographically in different Brazilian regions.


Q3. Which categories perform best and worst nationwide?

![Q3ChartA](Term2/visuals/Q3ChartA.jpeg)

10 most popular product categories (starting from the most popular):

1. bed_bath_table
2. health_beauty
3. sports_leisure
4. computers_accessories
5. furniture_decor
6. housewares
7. watches_gifts
8. telephony
9. auto
10. toys

![Q3ChartB](Term2/visuals/Q3ChartB.jpeg)

10 least popular product categories (starting from the least popular):

1. security_and_services
2. fashion_childrens_clothes
3. cds_dvds_musicals
4. la_cuisine
5. arts_and_craftmanship
6. home_comfort_2
7. fashion_sport
8. diapers_and_hygiene
9. flowers
10. music

From the chart on product categories:

Best-performing categories: bed_bath_table, health_beauty, and sports_leisure have the highest total number of orders.
This suggests that nationwide consumer preferences are skewed towards household, health, and leisure products.

Worst-performing categories include security_and_services, fashion_child_clothes and CDs_DVDs_musicals. These categories can be considered as points of growth by using promotions or improvement of supplied products variety.


Q4. Are there noticeable regional differences in the delivery periods?

![Q4Chart](Term2/visuals/Q4Chart.jpeg)

The map highlights regional differences in delivery times:

Northern regions (e.g., Amazonas) have the longest delivery periods (colored in red shades). Southern and Southeastern regions (e.g., São Paulo and Rio de Janeiro) demonstrate faster delivery times (colored in blue shades). Thus, regional disparities in delivery periods might signify logistical challenges. Infrastructure and distance from distribution centers might be the reasons of longer delivery in more remote northern areas. As a result, this information might be used for improving business processes.


# Conclusion and Future Work

The project results are helpful for understanding key factors influencing customer behavior and optimizing business strategies in Brazilian e-commerce.

1. Timely Deliveries Matter (Q1): On-time deliveries lead to positive sentiment, while delays often result in negative reviews. This emphasizes the need for efficient logistics.

2. Seasonal Sales Trends (Q2): Winter and Autumn drive high order volumes due to holiday promotions, while Spring requires creative strategies to boost sales.

3. Product Performance (Q3): Least popular product categories might be a sign of low demand and the necessity to give up on them or a sign of getting new shares in the market by improving the sellers' business systems in these areas. More analysis needs to be done to decide which way to go.

4. Regional Delivery Insights (Q4): Delivery delays are more pronounced in northern regions, indicating logistical challenges that need addressing to improve satisfaction.

These insights guide logistics optimization and new marketing strategies for better customer experiences and growth of businesses (sellers).

However, this analysis needs to be deepened with future work. Namely, focus could be on expanding geospatial analytics for delivery optimization, checking time trends over the years (comparing 2016, 2017 and 2018 within each other and with more recent data) and integrating external data sources (e.g., weather, social media trends). This could provide further insights to possible improvements to the Brazilian e-commerce market.
