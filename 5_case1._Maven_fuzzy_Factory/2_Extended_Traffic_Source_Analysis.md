
# Hybrid Sentiment Analysis

## Draft Proposal: Hybrid Sentiment Analysis tentang Pergantian Kurikulum Pembelajaran Menggunakan Artificial Neural Network (ANN) dan Topic Modeling

### Introduction

As an e-commerce company, increasing website traffic is essential for improving sales. In this analysis, we focus on identifying the top traffic sources and analyzing their effectiveness by evaluating session counts and conversion rates. The data comes from the `website_sessions` and `orders` tables, which record user sessions and transactions on the website.

### Traffic Sources

The traffic to the website can come from various sources such as:

- **Direct Click**: Visitors who directly visit the website.
- **Search Engines (gsearch)**: Visitors who arrive from search engine results.
- **Email**: Visitors who come from email campaigns.
- **Social Media**: Visitors who come from social media platforms.

We are interested in analyzing which traffic sources drive the most visitors to the website and their effectiveness in generating orders.

### SQL Queries for Analysis

#### 1. Identifying Top Traffic Source Based on Number of Sessions

The first step in our analysis is to calculate the total number of sessions for each traffic source and sort them in descending order to find the sources driving the most traffic.

##### SQL Query:

```sql
SELECT utm_source, COUNT(website_session_id) AS total_sessions
FROM website_sessions
GROUP BY utm_source
ORDER BY total_sessions DESC;
```

##### Explanation:

- **utm_source**: The source of the traffic (e.g., gsearch, email, etc.).
- **COUNT(website_session_id)**: The number of sessions for each traffic source.
- **GROUP BY utm_source**: Groups the data by traffic source.
- **ORDER BY total_sessions DESC**: Sorts the results to identify the sources that drive the most sessions.

#### 2. Analyzing Conversion Rate of Top Traffic Source

In addition to counting sessions, we analyze the conversion rate for each traffic source. The conversion rate is the percentage of sessions that resulted in an order, helping to assess the effectiveness of each traffic source.

##### SQL Query:

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

##### Explanation:

- **utm_source**: The source of the traffic.
- **COUNT(DISTINCT ws.website_session_id)**: Total unique sessions from each traffic source.
- **COUNT(DISTINCT o.website_session_id)**: Total unique sessions that resulted in orders.
- **conversion_rate**: The ratio of sessions resulting in orders, calculated as the total orders divided by the total sessions, multiplied by 100.
- **LEFT JOIN orders o ON ws.website_session_id = o.website_session_id**: Joins the `website_sessions` table with the `orders` table to track orders associated with sessions.
- **GROUP BY utm_source**: Groups the data by traffic source.
- **ORDER BY conversion_rate DESC**: Sorts the results to identify the traffic sources with the highest conversion rates.

### Bid Optimization Analysis

Companies often create campaigns on multiple platforms (e.g., Instagram, YouTube) to drive paid traffic. However, not all campaigns will generate substantial traffic. To optimize their marketing budget, companies need to assess which campaigns generate the most traffic and which ones perform poorly.

#### Case: Bid Optimization Analysis

The company wants to optimize its marketing budget by:

- **Reducing** or **stopping** campaigns on platforms with low traffic.
- **Increasing** the campaigns on platforms with high traffic.

**Paid Traffic Sources:**
- **Source A**: 600 sessions
- **Source B**: 150 sessions
- **Source C**: 700 sessions

#### What do we do in class?

- **Bid Up**: Increase campaigns on platforms generating high traffic (Source A and Source C).
- **Bid Down**: Reduce or stop campaigns on platforms with low traffic (Source B).

### SQL Query for Bid Optimization:

```sql
SELECT utm_source,
       COUNT(website_session_id) AS total_sessions
FROM website_sessions
WHERE utm_source IN ('Source A', 'Source B', 'Source C')
GROUP BY utm_source
ORDER BY total_sessions DESC;
```

#### Explanation:

This query helps to assess the traffic performance of different paid sources by calculating the number of sessions for each source, which allows us to decide on optimizing the campaigns.

### What do we do in Class?

- **Analyzing Budget Impact**: We analyze the effects of reducing the marketing budget on daily, weekly, and monthly website traffic trends.
- **Device-Specific Conversion Rates**: We analyze conversion rates based on device type (mobile vs desktop) to see if there are opportunities to optimize campaigns.
- **Weekly Traffic Trends**: We examine the effect of increasing campaigns on certain devices (e.g., mobile or desktop) on weekly traffic trends.
