
# Text Search

## Introduction

Text search is a process of finding specific words or phrases within a database. For example, consider the following table:

| Nama Produk     | Harga |
|------------------|-------|
| Kipas Angin      | 200   |
| Kipas Laptop     | 100   |
| Keyboard Laptop  | 150   |

To find product names containing the word "Kipas," the result would be:

| Nama Produk     | Harga |
|------------------|-------|
| Kipas Angin      | 200   |
| Kipas Laptop     | 100   |

This process is called **Text Search**.

---

## Text Search Techniques

### What Text Search Can Find:
1. **Words**: Searching for individual words.
2. **Phrases**: Searching for sequences of words.

---

## Text Search Methods

Text search can be performed using several techniques:
1. **Pattern Matching**
2. **Trigram Index**
3. **Full Text Search**

---

## Pattern Matching

### Definition
Pattern matching involves finding values that match a specific pattern within text data.

#### Example:
Searching for product names containing the word "Kipas."

| Nama Produk     | Harga |
|------------------|-------|
| Kipas Angin      | 200   |
| Kipas Laptop     | 100   |
| Keyboard Laptop  | 150   |

Search Result:

| Nama Produk     | Harga |
|------------------|-------|
| Kipas Angin      | 200   |
| Kipas Laptop     | 100   |

Pattern matching is useful for identifying matches based on defined patterns.

---

## Pattern Matching in PostgreSQL

In PostgreSQL, pattern matching can be performed using:
- **`LIKE`**: Case-sensitive pattern matching.
- **`ILIKE`**: Case-insensitive pattern matching.

### Example:

| Text  | LIKE "Kipas" | ILIKE "Kipas" |
|-------|--------------|---------------|
| KipAs | NO           | YES           |

---

### Pattern Matching Syntax

#### `LIKE`
- **Match Conditions**:
  - `LIKE 'key%'`: Matches values starting with `key` (e.g., `keynote`).
  - `LIKE '%key'`: Matches values ending with `key` (e.g., `Monkey`).
  - `LIKE '%key%'`: Matches values containing `key` anywhere (e.g., `DiscJockeyFams`).

#### Example Query:
```sql
SELECT *
FROM products
WHERE nama_produk LIKE 'Kip%';
```

#### Output:
| Nama Produk     | Harga |
|------------------|-------|
| Kipas Angin      | 200   |
| Kipas Laptop     | 100   |

---

#### `ILIKE`
- **Match Conditions**:
  - `ILIKE 'key%'`: Matches values starting with `key` (case-insensitive).
  - `ILIKE '%key'`: Matches values ending with `key` (case-insensitive).
  - `ILIKE '%key%'`: Matches values containing `key` anywhere (case-insensitive).

#### Example Query:
```sql
SELECT *
FROM products
WHERE nama_produk ILIKE '%PAS%';
```

#### Output:
| Nama Produk     | Harga |
|------------------|-------|
| Kipas Angin      | 200   |
| Kipas Laptop     | 100   |

---

## Summary

- Text search allows for efficient searching of words or phrases within a database.
- Pattern matching can be case-sensitive (`LIKE`) or case-insensitive (`ILIKE`).
- Pattern matching queries help locate data based on specific patterns in text.

