
# JSON in SQL

## What is JSON?

JSON (JavaScript Object Notation) is a lightweight format for exchanging data between applications. For instance, consider the data exchange process between a user application and a server, such as in an e-commerce platform like Tokopaedi.

### Example:

- **Search Request**: A user sends a search query to the server:
  ```json
  { "product": "Keyboard" }
  ```

- **Search Result**: The server responds with search results:
  ```json
  {
    "product": "Keyboard A21X",
    "merek": "Delta",
    "harga": 200000
  }
  ```

## JSON Structure

JSON stores data in key-value pairs enclosed within curly braces `{}`.

- **Key**: Must be a string.
- **Value**: Can be of various types such as:
  - String
  - Number
  - Object
  - Array
  - Boolean
  - Null

### Example:

```json
{
  "key": "value"
}
```

## JSON in PostgreSQL

PostgreSQL supports JSON as a data type. JSON data can be stored in table columns.

### Advantages of JSON:
- Flexibility: Allows storing diverse data without altering the table structure.
- Easy and quick data access.

## Types of JSON in PostgreSQL

1. **JSON**: Stored in text format.
2. **JSONB**: Stored in binary format, optimized for storage and querying.

### JSONB: Pros and Cons

#### Advantages:
- More efficient internal storage.
- Faster for storage and retrieval operations.

#### Disadvantages:
- Slower data input due to conversion during storage.

## Using JSONB in PostgreSQL

### Creating a Table with JSONB Column

We can define a table with a `JSONB` column to store structured data.

#### SQL Syntax:
```sql
CREATE TABLE produk_elektronik(
  produk_id SERIAL PRIMARY KEY,
  jenis TEXT,
  details JSONB
);
```

### Inserting Data into JSONB Column

#### SQL Syntax:
```sql
INSERT INTO produk_elektronik (jenis, details)
VALUES ('Handphone',
'{
  "products": {
    "nama": "Su 11 Ultra",
    "merek": "Su",
    "harga": 1999000,
    "warna": ["silver", "biru"]
  }
}');
```

### Explanation:
- The `details` column stores data as a JSONB object.
- JSONB allows storing nested structures like objects and arrays.

---

## Summary

JSON in PostgreSQL provides a flexible and efficient way to store and query structured data within relational databases. The choice between `JSON` and `JSONB` depends on the specific use case and performance requirements.
