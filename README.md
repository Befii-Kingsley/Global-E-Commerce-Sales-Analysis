# Global-E-Commerce-Sales-Analysis

Interactive Dashboard: https://lookerstudio.google.com/reporting/19b83267-9186-44dd-ba2c-1a53db6043d0/page/LLNHF

## Project Overview
This project delivers a comprehensive e-commerce analytics dashboard that transforms raw sales data into actionable business intelligence. By synthesizing multiple data streams from an online retail operation, I created a centralized interactive tool that reveals key insights into:
1. Revenue performance
2. Customer behavior
3. Operational efficiency
4. Geographic performance
5. Product category analysis

The dashboard includes interactive filters for:

- Date range
- Country
- Order status
- Vendor
- Delivery location

allowing stakeholders to drill down into specific segments and timeframes.

## Business Objectives
This dashboard helps stakeholders achieve the following:
1. Revenue Performance Monitoring → Track sales trends and identify growth opportunities
2. Customer Behavior Analysis → Understand purchasing patterns and customer retention
3. Operational Efficiency → Evaluate refund rates, decline rates, and delivery performance
4. Geographic Performance → Analyze sales distribution across global markets
5. Product Category Analysis → Identify high-performing and problematic product categories

## Process
This project was built using:
1. Data Storage: PostgreSQL database (online_store_sales) with e-commerce transaction data
2. Data Analysis: SQL queries with postgresql
3. Visualization: Google Looker Studio interactive dashboard
4. Approach: Descriptive & diagnostic analytics

Design Philosophy: Clean, intuitive interface with strategic metric placement & visual storytelling

## Key Metrics & Insights

1. Revenue Performance
- Total Revenue: $1,009,872.31 from 5,000 orders
- Revenue Trend: Peak between April 13-18, 2025
- Top Categories by Revenue: Beauty, Grocery, Home Decor
- Product Distribution: Revenue evenly distributed across major categories

2. Customer Analysis
- Unique Customers: 3,845
- Returning Customers: 1,155 (30% retention)
- Subscribers: 43.3% subscribers, 56.7% non-subscribers
- Loyalty: Strong repeat purchases across top product categories

3. Operations & Fulfillment
- Order Completion Rate: 88.8%
- Refund Rate: 4.72% (total refunds $35,822.18)
- Decline Rate: 10.08% (representing 68.1% of lost sales)
- Delivery Time: 5.59 days (Germany) → 6.39 days (France)

4. Geographic Performance
- Strong presence across North America, Asia, Europe
- Delivery challenges in France (6.39 days) & Brazil (6.35 days)
- Payment preferences vary by region (e.g., Crypto leads globally, Credit Card in China)

5. Product Category Issues
- Highest Refund Rates: Grocery (5.79%), Home Decor (5.33%), Books (4.99%)
- Grocery shows high revenue but high refunds → Possible quality/shipping issues

6. Vendor Performance
- Top Vendors: NovaTrade, TopVendor
- Specialists: MegaShop (Electronics), AlphaStore (Grocery)
- Balanced vendor distribution (no dangerous dependencies)

7. Payment Method Analysis
- Most used globally: Cryptocurrency

Regional variations:
- 🇺🇸 USA: Mobile Payment & Crypto
- 🇨🇳 China: Credit Card > Crypto
- 🇮🇳 India: Credit Card > PayPal > Crypto
- 🇨🇦 Canada: PayPal > Crypto

## Implementation Roadmap
Timeline	Action Item
- Immediate (1-30 days)	Address high decline rates; improve Grocery QC; optimize payment visibility
- Short-term (30-90 days)	Vendor performance program; subscription campaign; review logistics in France & Brazil
- Strategic (90+ days)	Expand categories; geographic expansion; predictive inventory & demand forecasting

## Technical Execution
- Database: PostgreSQL
- Query Tool: pgAdmin
- SQL Queries:
  
``` sql
-- global_ecommerce_sales_analysis.sql
-- Author: Success Kingsley
-- Project: Global E-Commerce Sales Analysis
-- Description: SQL queries used for analyzing online store sales data
-- Database: PostgreSQL
-- Table: online_store_sales

STEP A: I CREATED TABLE

CREATE TABLE online_store_sales (
order_id SERIAL PRIMARY KEY,
order_date DATE,
shipping_date DATE,
delivery_date DATE,
order_country VARCHAR(100),
delivery_location VARCHAR(100),
item VARCHAR(255),
category VARCHAR(100),
vendor VARCHAR(100),
cost NUMERIC(10,2),
price_usd NUMERIC(10,2),
discount NUMERIC(5,2),
quantity INT,
total_amount NUMERIC(12,2),
customer_id INT,
subscriber BOOLEAN,
order_status VARCHAR(50),
refunded_amount NUMERIC(10,2),
payment_method VARCHAR(50),
delivery_status VARCHAR(50)
);

STEP B: I IMPORTED DATA FROM CSV


STEP C: ANALYSIS QUERIES

------TOTAL REVENUE
SELECT
ROUND(SUM(total_amount), 2) AS total_revenue
FROM online_store_sales;

------TOTAL ORDERS
SELECT
COUNT(order_id) AS total_orders
FROM online_store_sales;

....... UNIQUE CUSTOMERS & RETURNING CUSTOMERS
SELECT
COUNT(DISTINCT customer_id) AS unique_customers,
COUNT(customer_id) - COUNT(DISTINCT customer_id) AS returning_customers
FROM online_store_sales;

------ ORDERS BY SUBSCRIPTION STATUS
SELECT
subscription_status,
COUNT(order_id) AS order_count
FROM online_store_sales
GROUP BY subscription_status;

------ REVENUE BY PRODUCT CATEGORY
SELECT
product_category,
ROUND(SUM(total_amount), 2) AS revenue
FROM online_store_sales
GROUP BY product_category
ORDER BY revenue DESC;

------ REFUND RATE BY PRODUCT CATEGORY
SELECT
product_category,
COUNT(CASE WHEN order_status = 'Refunded' THEN 1 END)::decimal / COUNT(*) * 100 AS refund_rate_percentage
FROM online_store_sales
GROUP BY product_category
ORDER BY refund_rate_percentage DESC;

--------- DECLINE RATE
SELECT
COUNT(CASE WHEN order_status = 'Declined' THEN 1 END)::decimal / COUNT(*) * 100 AS decline_rate_percentage
FROM online_store_sales;

------ REVENUE TREND OVER TIME
SELECT
order_date::date AS date,
ROUND(SUM(total_amount), 2) AS daily_revenue
FROM online_store_sales
GROUP BY date
ORDER BY date;

--------- AVERAGE DELIVERY TIME BY COUNTRY
SELECT
delivery_country,
ROUND(AVG(delivery_time_days), 2) AS avg_delivery_time_days
FROM online_store_sales
GROUP BY delivery_country
ORDER BY avg_delivery_time_days DESC;

--------- PAYMENT METHODS ANALYSIS
SELECT
payment_method,
COUNT(order_id) AS total_orders,
ROUND(SUM(total_amount), 2) AS total_revenue
FROM online_store_sales
GROUP BY payment_method
ORDER BY total_orders DESC;

-------- VENDOR PERFORMANCE
SELECT
vendor_name,
product_category,
COUNT(order_id) AS total_orders,
ROUND(SUM(total_amount), 2) AS total_revenue
FROM online_store_sales
GROUP BY vendor_name, product_category
ORDER BY vendor_name, total_revenue DESC;

---------- SALES BY GEOGRAPHIC LOCATION
SELECT
delivery_country,
COUNT(order_id) AS total_orders,
ROUND(SUM(total_amount), 2) AS total_revenue
FROM online_store_sales
GROUP BY delivery_country
ORDER BY total_revenue DESC;

```
- Interactive Dashboard: https://lookerstudio.google.com/reporting/19b83267-9186-44dd-ba2c-1a53db6043d0/page/LLNHF

## Business Impact
- Identified 10.08% declined orders = revenue leakage
- Pinpointed categories & regions needing improvement
- Provided insights to reduce refund rates & delivery times
- Empowered stakeholders with data-driven decisions instead of assumptions


👤 Author
Success Baribefii Kingsley | Data Analyst 😊 🙏 💛
