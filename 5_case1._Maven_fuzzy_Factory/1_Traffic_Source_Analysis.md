
# Traffic Source Analysis for E-Commerce Website

## Introduction

As an e-commerce company, increasing website traffic is essential for improving sales. In this analysis, we focus on identifying the top traffic sources and analyzing their effectiveness by evaluating session counts and conversion rates. The data comes from the `website_sessions` and `orders` tables, which record user sessions and transactions on the website.

## Traffic Sources

The traffic to the website can come from various sources such as:

- **Direct Click**: Visitors who directly visit the website.
- **Search Engines (gsearch)**: Visitors who arrive from search engine results.
- **Email**: Visitors who come from email campaigns.
- **Social Media**: Visitors who come from social media platforms.

We are interested in analyzing which traffic sources drive the most visitors to the website and their effectiveness in generating orders.

## SQL Queries for Analysis

### 1. Identifying Top Traffic Source Based on Number of Sessions

The first step in our analysis is to calculate the total number of sessions for each traffic source and sort them in descending order to find the sources driving the most traffic.

#### SQL Query:

```sql
SELECT utm_source, COUNT(website_session_id) AS total_sessions
FROM website_sessions
GROUP BY utm_source
ORDER BY total_sessions DESC;
```

#### Explanation:

- **utm_source**: The source of the traffic (e.g., gsearch, email, etc.).
- **COUNT(website_session_id)**: The number of sessions from each traffic source.
- **GROUP BY utm_source**: Groups the data by traffic source.
- **ORDER BY total_sessions DESC**: Sorts the results in descending order to identify the traffic sources with the highest number of sessions.

### 2. Analyzing Conversion Rate of Top Traffic Source

In addition to counting sessions, we can further analyze the effectiveness of each traffic source by calculating the conversion rate. This is done by comparing the number of sessions that resulted in an order to the total number of sessions for each source.

#### SQL Query:

```sql
SELECT utm_source,
       COUNT(DISTINCT ws.website_session_id) AS total_sessions,
       COUNT(DISTINCT o.website_session_id) AS total_orders,
       (COUNT(DISTINCT o.website_session_id) / COUNT(DISTINCT ws.website_session_id)) * 100 AS conversion_rate
FROM website_sessions ws
LEFT JOIN orders o ON ws.website_session_id = o.website_session_id
GROUP BY utm_source
ORDER BY conversion_rate DESC;
```

#### Explanation:

- **utm_source**: The source of the traffic.
- **COUNT(DISTINCT ws.website_session_id)**: The total number of unique sessions from each traffic source.
- **COUNT(DISTINCT o.website_session_id)**: The total number of unique sessions that resulted in an order.
- **conversion_rate**: The percentage of sessions that resulted in an order, calculated by dividing the total number of orders by the total number of sessions, multiplied by 100.
- **LEFT JOIN orders o ON ws.website_session_id = o.website_session_id**: Joins the `website_sessions` table with the `orders` table to link sessions with orders.
- **GROUP BY utm_source**: Groups the data by traffic source.
- **ORDER BY conversion_rate DESC**: Sorts the results by conversion rate in descending order to identify the most effective traffic sources.

## Conclusion

By executing the above queries, we can identify which traffic sources are driving the most sessions to the website and how effective each source is in converting those sessions into actual orders. The conversion rate helps us understand the impact of each traffic source on sales performance.
