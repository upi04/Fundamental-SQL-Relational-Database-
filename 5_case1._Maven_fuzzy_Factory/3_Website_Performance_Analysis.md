# Website Performance Analysis


## Key Metrics to Analyze

1. **Top Entry Page**: Identifies which page is the first entry point for most users.  
2. **Bounce Rate**: Measures the percentage of visitors who leave after viewing only one page.  
3. **Conversion Funnel**: Calculates the percentage of users moving from one stage to the next (e.g., Home → Product → Cart → Sale).

## SQL Queries for Analysis

Below are some example queries using the provided `website_sessions` and `website_pageviews` tables. These queries help measure performance at different stages of the funnel.

---

### 1. Identifying the Top Entry Page

Counts how many sessions begin on each page (the page visited first in a session).

```sql
-- Step 1: Find the earliest pageview time for each session
WITH first_pageview AS (
    SELECT 
        website_session_id,
        MIN(created_at) AS first_time
    FROM website_pageviews
    GROUP BY website_session_id
)

-- Step 2: Join with website_pageviews to see which page was visited first
SELECT 
    wp.pageview_url AS first_visited_page,
    COUNT(*) AS total_sessions
FROM first_pageview fp
JOIN website_pageviews wp 
    ON fp.website_session_id = wp.website_session_id
    AND fp.first_time = wp.created_at
GROUP BY wp.pageview_url
ORDER BY total_sessions DESC;
```
### 2. Calculating the Bounce Rate for a Specific Page
In this example, we assume a bounce occurs if a session has only 1 pageview (i.e., the user exits after viewing just one page).
```sql

-- Count how many pageviews each session has
WITH session_pageview_count AS (
    SELECT
        website_session_id,
        COUNT(*) AS total_pageviews
    FROM website_pageviews
    GROUP BY website_session_id
)

-- Calculate bounce rate (sessions with 1 pageview / total sessions)
SELECT
    ROUND(
        (SUM(CASE WHEN total_pageviews = 1 THEN 1 ELSE 0 END) 
        * 100.0 / COUNT(*)), 
        2
    ) AS bounce_rate_percentage
FROM session_pageview_count;
```
### 3. Conversion Funnel Analysis
Determines the number of sessions that visit each funnel stage (e.g., home, product, cart, sale) and calculates the click-through rate from one page to the next.
```sql
-- Example: checking whether each session visited a particular funnel page
WITH funnel AS (
    SELECT 
        website_session_id,
        MAX(CASE WHEN pageview_url = 'home' THEN 1 ELSE 0 END) AS visited_home,
        MAX(CASE WHEN pageview_url = 'product' THEN 1 ELSE 0 END) AS visited_product,
        MAX(CASE WHEN pageview_url = 'cart' THEN 1 ELSE 0 END) AS visited_cart,
        MAX(CASE WHEN pageview_url = 'sale' THEN 1 ELSE 0 END) AS visited_sale
    FROM website_pageviews
    GROUP BY website_session_id
)

SELECT
    COUNT(*) AS total_sessions,
    SUM(visited_home) AS home_visitors,
    SUM(visited_product) AS product_visitors,
    SUM(visited_cart) AS cart_visitors,
    SUM(visited_sale) AS sale_visitors
FROM funnel;
```
Conclusion
By using queries that identify the Top Entry Page, calculate the Bounce Rate, and assess the Conversion Funnel, a company can pinpoint where users drop off and experiment (A/B Testing) to optimize their user journey. These analytics are crucial for improving website performance and ultimately increasing sales.

```





