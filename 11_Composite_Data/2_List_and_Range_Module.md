
# List and Range Data Type in SQL

## What is List?

List is used to store multiple values in a single data field.

### Example Table:

| nama_produk | merek          | harga          |
|-------------|----------------|----------------|
| Handphone   | {A, B, C}      | {100, 200, 300}|
| Laptop      | {A, B, C}      | {500, 700, 900}|
| Kamera      | {A, B, C}      | {400, 600, 800}|

**List of brands (merek) for Handphone:**  
- A  
- B  
- C

### Advantages of List:

- **Structured Data Storage:** Organizes multiple values within a single field.
- **Efficient Data Retrieval:** Facilitates searching based on specific criteria like brand or price.

---

## Types of List

### 1. Array

Array is used to store multiple values in a single row within a field.

#### Example:

| nama_produk | merek       | harga       |
|-------------|-------------|-------------|
| Handphone   | {A, B, C}   | {100, 200, 300}|
| Laptop      | {A, B, C}   | {500, 700, 900}|
| Kamera      | {A, B, C}   | {400, 600, 800}|

#### Creating Array

- **Create Table:**

```sql
CREATE TABLE produk_elektronik(
    nama_produk TEXT,
    merek TEXT[],
    harga INTEGER[]
);
```

- **Insert Data:**

```sql
INSERT INTO produk_elektronik (nama_produk, merek, harga)
VALUES
('Handphone', ARRAY['A', 'B', 'C'], ARRAY[100, 200, 300]);
```

#### Selecting Array:

- **Example Query:** Retrieve brand "A" from all products:

```sql
SELECT nama_produk, merek[1] AS merek
FROM produk_elektronik;
```

**Output:**

| nama_produk | merek |
|-------------|-------|
| Handphone   | A     |
| Laptop      | A     |
| Kamera      | A     |

---

## What is Range?

Range is used to handle a continuous range of values instead of listing them individually.

### Example:

| nama_produk | merek       | harga int4range  |
|-------------|-------------|------------------|
| Handphone   | {A, B, C}   | [100, 300]       |
| Laptop      | {A, B, C}   | [500, 700]       |
| Kamera      | {A, B, C}   | [400, 600]       |

### Creating Range:

- **Create Table:**

```sql
CREATE TABLE produk_elektronik(
    nama_produk TEXT,
    range_harga NUMRANGE
);
```

- **Insert Data:**

```sql
INSERT INTO produk_elektronik (nama_produk, range_harga)
VALUES
    ('Handphone', '[100,300]'),
    ('Kamera', '[400,800]'),
    ('Laptop', '[600,1000]');
```

### Selecting Range:

- **Example Query:** Find products where price 150 is within the price range:

```sql
SELECT *
FROM produk_elektronik
WHERE range_harga @> '150'::numeric;
```

**Output:**  
Handphone (price range [100, 300])

---

## Notes on Range Bounds:

- `[100, 300]`: Includes 100 and 300.
- `(100, 300]`: Excludes 100, includes 300.
- `[100, 300)`: Includes 100, excludes 300.

---


