# Database Design (Perancangan Database)

This guide discusses the **database design process**, from defining a mission statement to creating views. A well-structured database design can save time, ensure high performance, and simplify core data operations (querying, inserting, updating, deleting).

---

## 1. Purpose of Database Design

A good database design yields multiple benefits:
1. **Time-Efficient**  
   - Reduces the need to continually change the structure later on.
2. **High Performance**  
   - A more efficient design can help maintain system speed as data grows.
3. **Simplified Data Operations**  
   - Structured tables and relationships make queries (insert, update, delete) simpler.

---

## 2. Database Design Stages

+------------------+          +---------------------+          +------------------+
|    customer      |          |       order         |          |   order_detail   |
+------------------+          +---------------------+          +------------------+
| customer_id (PK) |          | order_id (PK)       |          | order_detail_id  |
| username         |          | customer_id (FK) -->|          | (PK)             |
| firstName        |          | status             |          | order_id (FK) -->|
| lastName         |          | created            |          | product_id (FK) ->|
| email            |          | total_price        |          | quantity          |
| phone            |          +---------------------+          +------------------+
+------------------+                  |
                                     +---------------------+
                                     |   order_shipment    |
                                     +---------------------+
                                     | shipment_id (PK)    |
                                     | order_id (FK) ----->|
                                     | tracking_number     |
                                     | provider            |
                                     | date                |
                                     | status              |
                                     +---------------------+

Below is a general **diagram** or **workflow** for database design:

1. **Mission Statement**  
2. **Create Table Structure**  
3. **Determine Table Relationship**  
4. **Determine Business Rule**  
5. **Determine Views**  
6. **Review Data Integrity** (Advanced Topic)

### 2.1. Mission Statement

The **mission statement** clarifies the goal of the database and the problem it intends to solve.

**Example**:  
> "The XYZ Minimarket database aims to store purchase transaction data, which can be used to analyze sales results."

### 2.2. Creating Table Structures

Identify the **tables**, **fields**, and **primary keys**, as well as the **purpose** of each table.

| Tabel Transaksi  | Tabel Pembeli       | Tabel Kasir       | Tabel Produk                |
|-------------------|---------------------|--------------------|-----------------------------|
| Menyimpan transaksi penjualan | Menyimpan detail pembeli | Menyimpan info kasir | Menyimpan detail produk/barang |
| - transaksi_id   | - pembeli_id        | - kasir_id         | - produk_id                 |
| - nama_pembeli   | - nama             | - name             | - nama                      |
| - nama_kasir     | - alamat           | - phone            | - jenis                     |
| - tanggal        |                     |                    | - harga                     |
| - produk         |                     |                    |                             |
| - total_harga    |                     |                    |                             |



### 2.3. Determine Table Relationships

Establish **logical relationships** (e.g., **primary keys** and **foreign keys**) among tables.

**Example Relationship Diagram**:

- **Pembeli**: `pembeli_id (PK)`
- **Transaksi**: `transaksi_id (PK)`, `pembeli_id (FK)`, `kasir_id (FK)`, `tanggal`
- **Kasir**: `kasir_id (PK)`, `name`, `phone`
- **Detail Transaksi**: `transaksi_id (CPK/FK)`, `produk_id (CPK/FK)`, `qty`, `total_harga`
- **Produk**: `produk_id (PK)`, `nama`, `jenis`, `harga`

*(CPK = Composite Primary Key, FK = Foreign Key, PK = Primary Key.)*

### 2.4. Determine Business Rules

**Business rules** are constraints reflecting how the organization operates. They can be **field-specific** or **relationship-specific**.

- **Field-Specific Rules**: Constraints on individual columns.  
  *Example*: `kode_pos` must have 9 characters.
- **Relationship-Specific Rules**: Constraints across multiple tables.  
  *Example*: `kode_pos` in `karyawan` table must match existing postal codes in the system.

**Applying Business Rules via Database Constraints**

| Tabel Transaksi  | Tabel Pembeli | Tabel Kasir | Tabel Produk  | Tabel Detail Transaksi |
|-------------------|--------------|------------|--------------|------------------------|
| Field            | - transaksi_id | - pembeli_id | - kasir_id  | - produk_id           |
|                  | - pembeli_id  | - nama       | - name       | - qty                 |
|                  | - kasir_id    | - alamat     | - phone      | - total_harga         |
|                  | - tanggal     |             |              |                        |
|                  | - produk      |             |              |                        |
|                  | - total_harga |             |              |                        |
| **Rule**         | Setiap kolom tidak boleh kosong | Kolom ID & nama pembeli tidak boleh kosong | Setiap kolom tidak boleh kosong | Setiap kolom tidak boleh kosong |

---

## 3. Determining Views

A **view** is a virtual table providing an easy way to retrieve combined or aggregated data from multiple tables (or even other views). 

- **Views** can save you from writing the same complex query repeatedly.
- **Example**: A manager needing monthly revenue data.

```sql
CREATE VIEW total_transaksi_bulanan AS
SELECT 
    MONTH(tgl_transaksi) AS bulan, 
    SUM(jumlah_pembayaran) AS total_pendapatan
FROM sales
GROUP BY bulan;
View Name: total_transaksi_bulanan
MONTH(tgl_transaksi): Extracts the month from the tgl_transaksi column.
SUM(jumlah_pembayaran): Aggregates total payment amounts.
GROUP BY bulan: Groups the data by month.

```

Thought for a second
markdown
Copy code
# Database Design (Perancangan Database)

This guide discusses the **database design process**, from defining a mission statement to creating views. A well-structured database design can save time, ensure high performance, and simplify core data operations (querying, inserting, updating, deleting).

---


## 4. Summary
Database Design involves:

Defining the Mission: What the database is for and what problem it solves.
Creating Table Structures: Designing tables with proper fields and keys.
Establishing Relationships: Setting up PK-FK links, composite keys, etc.
Applying Business Rules: Ensuring real-world constraints (field and relationship rules).
Creating Views: Simplifying complex queries and providing easy reporting tools.
Reviewing Data Integrity (Advanced): Enforcing strict rules (constraints, triggers) to maintain data consistency.
A thoughtful database design ensures scalability, maintainability, and optimized performance for the long term.
