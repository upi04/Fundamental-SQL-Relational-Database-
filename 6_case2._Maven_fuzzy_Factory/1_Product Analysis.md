# Product Analysis

## 1. Overview

The main goal of **Product Analysis** is to determine strategies to address underperforming product sales or to develop new strategies to boost sales if performance is already good. By observing **trend analysis** (daily, weekly, or monthly), a company can gauge how well a product is performing over time.

---

## 2. Trend Analysis

A product sales trend per period is a key indicator of business performance:
- **Positive Trend**: Sales volume is increasing.
- **Negative Trend**: Sales volume is decreasing.

In the case of this new product, the company wants to evaluate the monthly sales over a 3-month period to decide what actions to take.

---

## 3. Case: Product Sales Analysis
To see the total sales per transaction date (daily granularity), you can sum the orders on each date:
#Query Daily/Transaction-Level Sales
```sql
SELECT 
    tgl_transaksi,
    SUM(jumlah_pemesanan) AS total_pemesanan
FROM product_sales
WHERE ID_produk = 231  -- Filter by product if needed
GROUP BY tgl_transaksi
ORDER BY tgl_transaksi;
```
Example Output:
| ID_customer | ID_produk | tgl_transaksi | jumlah_pemesanan |
|-------------|-----------|---------------|-------------------|
| 175         | 231       | 2023-01-01    | 400000            |
| 187         | 231       | 2023-01-04    | 300000            |
| 198         | 231       | 2023-01-06    | 270000            |
| 199         | 231       | 2023-01-09    | 350000            |

From these transactions, data can be aggregated weekly or monthly to see the bigger picture.

### 3.2. Monthly Aggregation for Trend Analysis
Depending on your SQL dialect, you can group by the month. For example, in MySQL or PostgreSQL, you might use EXTRACT(MONTH FROM tgl_transaksi):
```sql
SELECT 
    EXTRACT(MONTH FROM tgl_transaksi) AS bulan,
    SUM(jumlah_pemesanan) AS total_pemesanan
FROM product_sales
WHERE ID_produk = 231
GROUP BY EXTRACT(MONTH FROM tgl_transaksi)
ORDER BY bulan;
```
Example Output:
| bulan | jumlah_pemesanan |
|-------|-------------------|
| 1     | 740000           |
| 2     | 216000           |
| 3     | 304000           |

> **Note**:  
> - "bulan" represents the month (1 = January, 2 = February, 3 = March, etc.).  
> - "jumlah_pemesanan" is the total sales amount for that month.

According to the aggregation, the sales in:
- Month 1: 740,000
- Month 2: 216,000
- Month 3: 304,000


---

## 4. Trend Observation
 ## Analyzing Trend Changes (Month-over-Month)
If you want to see how much sales changed compared to the previous month, you can use a window function such as LAG() (supported in PostgreSQL, SQL Server, Oracle, etc.). This allows you to calculate a difference between the current month’s sales and the previous month’s sales.
```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', tgl_transaksi)::DATE AS month_start,
        SUM(jumlah_pemesanan) AS total_pemesanan
    FROM product_sales
    WHERE ID_produk = 231
    GROUP BY DATE_TRUNC('month', tgl_transaksi)
)
SELECT
    month_start,
    total_pemesanan,
    LAG(total_pemesanan) OVER (ORDER BY month_start) AS prev_month_sales,
    (total_pemesanan - LAG(total_pemesanan) OVER (ORDER BY month_start)) AS diff_vs_prev
FROM monthly_sales
ORDER BY month_start;


```

## 6. Insights from SQL Analysis
- Daily/Transaction-Level Sales: Identify which specific dates have high or low sales.
- Monthly Trend: Detect an overall decline or growth over months.
- Month-over-Month Changes: Pinpoint exactly how sales are shifting from one month to the next, allowing for quick strategic decisions (promotions, campaigns, etc.).

---

## 6. Conclusion

- The **monthly sales trend** is crucial to understanding product performance.
- While Month 1 had the highest sales, Month 2 experienced a downturn, followed by a partial recovery in Month 3.
- To **increase sales**, businesses can implement targeted strategies such as promotional campaigns, improved marketing, and product enhancements. Continuous analysis of sales data will guide ongoing optimizations and help maintain a **positive sales trend** over time.
