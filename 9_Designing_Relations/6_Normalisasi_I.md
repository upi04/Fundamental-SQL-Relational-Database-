# Database Normalization

## 1. Introduction to Normalization

**Normalization** is the process of organizing data within a database to:
- Reduce redundancy.
- Avoid inconsistent dependency.
- Improve database efficiency.

### Benefits of Normalization:
1. Eliminate redundant data to save storage.
2. Ensure data consistency across the database.
3. Simplify updates and queries.

---

## 2. Steps of Normalization

### Levels of Normalization:
1. **First Normal Form (1NF)**
2. **Second Normal Form (2NF)**
3. **Third Normal Form (3NF)**

---

## 3. First Normal Form (1NF)

**Definition:**
- A table is in 1NF when:
  - Each column contains atomic (indivisible) values.
  - There are no multivalued fields.

### Example:
#### Unnormalized Table:
| employee_id | first_name | last_name | salary     | workshop            | finished_workshop |
|-------------|------------|-----------|------------|---------------------|--------------------|
| 1000        | Kira       | Bently    | 10,000,000 | Oracle, SQL Server  | 20-02-2022         |
| 1001        | Katherine  | Erlich    | 8,500,000  | Autocad MAP         | 27-03-2022         |
| 1002        | Bill       | Chamlin   | 9,000,000  | Autocad MAP, Photoshop | 02-02-2022      |

#### Normalized Table (1NF):
| employee_id | first_name | last_name | salary     | workshop      | finished_workshop |
|-------------|------------|-----------|------------|---------------|--------------------|
| 1000        | Kira       | Bently    | 10,000,000 | Oracle        | 20-02-2022         |
| 1000        | Kira       | Bently    | 10,000,000 | SQL Server    | 20-02-2022         |
| 1001        | Katherine  | Erlich    | 8,500,000  | Autocad MAP   | 27-03-2022         |
| 1002        | Bill       | Chamlin   | 9,000,000  | Autocad MAP   | 02-02-2022         |
| 1002        | Bill       | Chamlin   | 9,000,000  | Photoshop     | 02-02-2022         |

---

## 4. Second Normal Form (2NF)

**Definition:**
- A table is in 2NF if:
  - It is in 1NF.
  - All non-key attributes depend fully on the primary key (no partial dependencies).

### Functional Dependencies:
- **fd1:** `employee_id → first_name, last_name, salary`
- **fd2:** `employee_id, workshop → finished_workshop`

### Steps to Achieve 2NF:
1. Eliminate partial dependencies by splitting the table into multiple tables.

#### Example:
##### Original Table:
| employee_id | first_name | last_name | salary     | workshop      | finished_workshop |
|-------------|------------|-----------|------------|---------------|--------------------|
| 1000        | Kira       | Bently    | 10,000,000 | Oracle        | 20-02-2022         |
| 1000        | Kira       | Bently    | 10,000,000 | SQL Server    | 20-02-2022         |

##### Normalized Tables (2NF):
1. **Employee Table:**
   | employee_id | first_name | last_name | salary     |
   |-------------|------------|-----------|------------|
   | 1000        | Kira       | Bently    | 10,000,000 |

2. **Employee_Workshop Table:**
   | employee_id | workshop   | finished_workshop |
   |-------------|------------|--------------------|
   | 1000        | Oracle     | 20-02-2022         |
   | 1000        | SQL Server | 20-02-2022         |

---

## 5. Case Study: Transaction Normalization

### Original Table:
| kode_transaksi | pembeli_id | kasir_id | nama_produk       | jenis_produk      | harga_satuan | qty | tanggal_transaksi | total_bayar |
|----------------|------------|----------|-------------------|-------------------|--------------|-----|-------------------|-------------|
| 1A01           | 111        | 1        | Pasta Gigi X1     | Kebersihan Badan  | 20,000       | 2   | 12-01-2023        | 45,000      |
| 1A02           | 111        | 1        | Sabun Mandi P2    | Kebersihan Badan  | 5,000        | 1   | 12-01-2023        | 45,000      |

### Normalization Steps:

1. **First Normal Form (1NF):**
   - Ensure no multivalued fields exist. The table is already in 1NF.

2. **Second Normal Form (2NF):**
   - Eliminate partial dependencies.

#### Normalized Tables (2NF):
1. **Transaction Table:**
   | kode_transaksi | pembeli_id | kasir_id | tanggal_transaksi | total_bayar |
   |----------------|------------|----------|-------------------|-------------|
   | 1A01           | 111        | 1        | 12-01-2023        | 45,000      |

2. **Product Table:**
   | produk_id | nama_produk       | jenis_produk      | harga_satuan |
   |-----------|-------------------|-------------------|--------------|
   | 1         | Pasta Gigi X1     | Kebersihan Badan  | 20,000       |

3. **Transaction Details Table:**
   | kode_transaksi | produk_id | qty |
   |----------------|-----------|-----|
   | 1A01           | 1         | 2   |

---

## 6. Summary of Normalization

### **Key Points:**
1. **1NF:** Remove multivalued fields.
2. **2NF:** Ensure all non-key attributes depend fully on the primary key.
3. **Functional Dependency:** Helps identify partial dependencies.

### **Benefits:**
- Saves storage space.
- Maintains data consistency.
- Simplifies data manipulation.

---

## 7. Example SQL Commands for Normalized Tables

```sql
-- Create Employee Table
CREATE TABLE employee (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    salary NUMERIC
);

-- Create Employee_Workshop Table
CREATE TABLE employee_workshop (
    employee_id INT,
    workshop VARCHAR(100),
    finished_workshop DATE,
    PRIMARY KEY (employee_id, workshop),
    FOREIGN KEY (employee_id) REFERENCES employee(employee_id)
);

-- Create Product Table
CREATE TABLE product (
    produk_id SERIAL PRIMARY KEY,
    nama_produk VARCHAR(255) NOT NULL,
    jenis_produk VARCHAR(255),
    harga_satuan NUMERIC
);

-- Create Transaction Table
CREATE TABLE transaction (
    kode_transaksi VARCHAR(10) PRIMARY KEY,
    pembeli_id INT,
    kasir_id INT,
    tanggal_transaksi DATE,
    total_bayar NUMERIC
);

-- Create Transaction Details Table
CREATE TABLE transaction_details (
    kode_transaksi VARCHAR(10),
    produk_id INT,
    qty NUMERIC,
    PRIMARY KEY (kode_transaksi, produk_id),
    FOREIGN KEY (kode_transaksi) REFERENCES transaction(kode_transaksi),
    FOREIGN KEY (produk_id) REFERENCES product(produk_id)
);
```