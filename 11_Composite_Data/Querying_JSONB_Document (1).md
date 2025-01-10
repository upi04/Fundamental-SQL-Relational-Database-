
# Querying JSONB Document

## Definition

Querying JSONB Document refers to the process of extracting data from a JSONB document using SQL queries.

### Example Diagram:

JSON Data:
```json
{
  "nama_produk": "Product A",
  "merek": "XYZ",
  "harga": 100000
}
```

SQL QUERY → Table Output:
```plaintext
nama_produk | merek | harga
Product A   | XYZ   | 100000
```

### Explanation:
- Querying JSONB is similar to regular SQL queries.
- The difference lies in the addition of JSONB indexing operators.

## Steps for Querying JSONB:

1. **Understand the Data Structure**:
   Familiarize yourself with the JSON structure stored in the table.

2. **Use JSONB Operators**:
   - `->`: Extracts a JSON object or array.
   - `->>`: Extracts a JSON value as text.

---

## Case Example

### Table: `produk_elektronik`

**Columns**:
- `produk_id`: integer
- `jenis`: text
- `details`: jsonb

**Example JSON**:
```json
{
  "products": {
    "nama": "Su 11 Ultra",
    "merek": "Su",
    "harga": 1999000,
    "warna": ["silver", "blue"]
  }
}
```

---

## Querying JSONB

### Case: Select Elements from `products`

**SQL Query**:
```sql
SELECT
  details -> 'products' ->> 'nama' AS nama,
  details -> 'products' ->> 'merek' AS merek,
  details -> 'products' ->> 'harga' AS harga,
  details -> 'products' ->> 'warna' AS warna
FROM produk_elektronik;
```

**Output**:
```plaintext
nama          | merek | harga   | warna
--------------|-------|---------|----------------
Su 11 Ultra   | Su    | 1999000 | ["silver", "blue"]
Su Mini       | Su    | 1500000 | ["silver", "black"]
Blu Note 9    | Blu   | 1800000 | ["blue", "black"]
Blu Note 9 Pro| Blu   | 1999000 | ["silver", "blue"]
```

---

### Case: Filter Products by Price and Brand

**Scenario**:
Display products with:
- Price >= 1,900,000
- Brand = 'Su'

**Note**:
The `harga` (price) field is stored as text, so it needs to be cast into an integer using `CAST`.

**SQL Query**:
```sql
SELECT
  details -> 'products' ->> 'nama' AS nama,
  details -> 'products' ->> 'merek' AS merek,
  CAST(details -> 'products' ->> 'harga' AS INTEGER) AS harga
FROM produk_elektronik
WHERE
  CAST(details -> 'products' ->> 'harga' AS INTEGER) >= 1900000
  AND (details -> 'products' ->> 'merek') = 'Su';
```

**Output**:
```plaintext
nama          | merek | harga
--------------|-------|--------
Su 11 Ultra   | Su    | 1999000
```

---

## Summary of JSONB Operators

- `->`: Retrieves JSON objects or arrays.
- `->>`: Retrieves JSON values as text.
- Use `CAST` to convert data types for specific operations.

---

## Additional Notes:
- JSONB is highly flexible for storing semi-structured data.
- Queries can efficiently extract, filter, and transform JSON data using PostgreSQL's JSONB functions and operators.
