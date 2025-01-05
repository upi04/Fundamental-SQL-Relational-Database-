# User Analysis: Identifying and Measuring Repeat Visitors

Below are some SQL query examples and explanations on how to identify **repeat visitors**, calculate **conversion rates** for repeat sessions, and analyze time-to-repeat for users. We assume you have at least two relevant tables:

- **`website_sessions`** (records each visit/session, with columns like:  
  - `website_session_id` (PK)  
  - `user_id`  
  - `created_at`  
  - `is_repeat_session` (indicator if the session is new or repeat)  
  - … other columns such as `utm_source`, `device_type`, etc.

- **`orders`** (records completed transactions, with columns like:  
  - `order_id` (PK)  
  - `website_session_id` (FK referencing `website_sessions`)  
  - `user_id`  
  - `created_at` (time of purchase)  
  - … other columns for items purchased, total amount, etc.)

> **Note**: The table/column names and data structures may differ in your actual schema, so adjust the queries accordingly.

---

## 1. Identifying Repeat Visitors

### 1.1. Counting Sessions per User

First, let’s see how many sessions each user has, which will indicate whether or not they visited multiple times (repeat visitors).

```sql
SELECT
    user_id,
    COUNT(*) AS total_sessions
FROM website_sessions
GROUP BY user_id
ORDER BY total_sessions DESC;

```
Explanation:

Groups all sessions by user_id and counts them.
A user with total_sessions > 1 indicates a repeat visitor.

###1.2. Distinguishing New vs. Repeat Sessions
If your data includes a column like is_repeat_session (0 = new, 1 = repeat), we can split the counts:
---sql
SELECT
    user_id,
    SUM(CASE WHEN is_repeat_session = 0 THEN 1 ELSE 0 END) AS total_new_sessions,
    SUM(CASE WHEN is_repeat_session = 1 THEN 1 ELSE 0 END) AS total_repeat_sessions
FROM website_sessions
GROUP BY user_id
ORDER BY total_repeat_sessions DESC;
---
Explanation:

Uses a conditional sum to separately count new vs. repeat sessions for each user.
A high number of repeat sessions suggests the user frequently revisits the site.

## 2. Measuring Conversion Rate for Repeat Sessions
To understand if repeat visitors are more likely to make a purchase, we can calculate the conversion rate for new vs. repeat sessions. We assume:

website_sessions.website_session_id matches orders.website_session_id (one-to-many or one-to-one relationship).
is_repeat_session = 0 means new session, = 1 means repeat session.
---sql
SELECT
    ws.is_repeat_session,
    COUNT(DISTINCT ws.website_session_id) AS total_sessions,
    COUNT(DISTINCT o.order_id) AS total_orders,
    ROUND(
        (COUNT(DISTINCT o.order_id) * 100.0 
         / COUNT(DISTINCT ws.website_session_id)),
        2
    ) AS conversion_rate_percentage
FROM website_sessions ws
LEFT JOIN orders o 
    ON ws.website_session_id = o.website_session_id
GROUP BY ws.is_repeat_session;
---
Explanation
ws.is_repeat_session: Groups by whether the session was repeat or new.
COUNT(DISTINCT ws.website_session_id): Number of (unique) sessions in that group.
COUNT(DISTINCT o.order_id): Number of (unique) orders tied to those sessions.
Conversion Rate = \frac{\text{total_orders}}{\text{total_sessions}} \times 100\%.

## 3. Identifying Time Between Visits (Time to Repeat)
To understand how quickly users come back, we can measure the time gap between a user’s first session and their subsequent sessions. This often involves a self-join or window function.

### 3.1. Example with Window Functions (PostgreSQL/SQL Server)
---sql
WITH session_times AS (
    SELECT
        user_id,
        created_at,
        ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY created_at) AS session_index
    FROM website_sessions
)
SELECT
    st1.user_id,
    st1.created_at AS current_session_time,
    st2.created_at AS next_session_time,
    EXTRACT(EPOCH FROM (st2.created_at - st1.created_at)) / 3600 AS gap_in_hours
FROM session_times st1
JOIN session_times st2 
    ON st1.user_id = st2.user_id
   AND st1.session_index + 1 = st2.session_index
ORDER BY st1.user_id, st1.created_at;
---
Explanation
ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at): Assigns a sequential number to each session per user, based on the session’s time.
st1 is the “current session,” and st2 is the “next session.”
EXTRACT(EPOCH FROM (st2.created_at - st1.created_at)) / 3600: Calculates the difference in hours between the two sessions. (Adjust units as needed: hours, days, etc.)
This shows how long it takes before each user returns, highlighting the time to repeat.
