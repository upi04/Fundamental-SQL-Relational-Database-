# Database Normalization: Third Normal Form (3NF)

## 1. Introduction to Normalization

**Normalization** is the process of organizing data to reduce redundancy and avoid anomalies. The third normal form (3NF) focuses on eliminating **transitive dependencies**.

### Goals of 3NF:
- Ensure all data dependencies are logical and directly related to the primary key.
- Avoid redundant data by splitting it into related tables.

---

## 2. Understanding Transitive Dependencies

### What is a Transitive Dependency?
- A situation where a non-key field depends indirectly on the primary key through another non-key field.
- Represented as:
Where:
- `X`: Primary Key
- `Y`: Non-Key Field
- `Z`: Another Non-Key Field dependent on `Y`.

### Example:
#### Initial Table:
| student_id | first_name | last_name | major                | department             |
|------------|------------|-----------|----------------------|------------------------|
| 1000       | Kira       | Granger   | Software Engineering | Informatic Engineering |
| 1001       | Katherine  | Erlich    | Data Science         | Informatic Engineering |
| 1002       | Bill       | Chamlin   | Electronic           | Electronic Engineering |
| 1003       | Shannon    | Black     | Mechatronics         | Electronic Engineering |

#### Dependencies:
1. **Functional Dependencies:**
 - `student_id → first_name, last_name, major, department`
 - `major → department`

2. **Transitive Dependency:**
 - `student_id → department` (via `major`)

### Problems Caused by Transitive Dependencies:
1. **Insert Anomaly:**
 - A new department cannot be added without adding a student.

2. **Update Anomaly:**
 - Updating a department name requires multiple updates across all rows.

3. **Delete Anomaly:**
 - Deleting a student could result in the loss of important department information.

---

## 3. Third Normal Form (3NF)

### Requirements for 3NF:
1. **Meet 2NF Requirements:**
 - No multivalued fields.
 - No partial dependencies.

2. **Eliminate Transitive Dependencies:**
 - All non-key fields should depend only on the primary key.

---

## 4. Case Study: Applying 3NF

### Initial Table:
| student_id | first_name | last_name | major                | department             |
|------------|------------|-----------|----------------------|------------------------|
| 1000       | Kira       | Granger   | Software Engineering | Informatic Engineering |
| 1001       | Katherine  | Erlich    | Data Science         | Informatic Engineering |
| 1002       | Bill       | Chamlin   | Electronic           | Electronic Engineering |
| 1003       | Shannon    | Black     | Mechatronics         | Electronic Engineering |

### Functional Dependencies:
1. `student_id → first_name, last_name, major, department`
2. `major → department`

### Steps to Decompose into 3NF:
1. Identify transitive dependencies:
 - `major → department`
2. Split the table into two related tables:
 - One for student details.
 - One for major and department mapping.

### Normalized Tables:

#### **Table 1: Student Details**
| student_id | first_name | last_name | major                |
|------------|------------|-----------|----------------------|
| 1000       | Kira       | Granger   | Software Engineering |
| 1001       | Katherine  | Erlich    | Data Science         |
| 1002       | Bill       | Chamlin   | Electronic           |
| 1003       | Shannon    | Black     | Mechatronics         |

#### **Table 2: Major and Department Mapping**
| major                | department             |
|----------------------|------------------------|
| Software Engineering | Informatic Engineering |
| Data Science         | Informatic Engineering |
| Electronic           | Electronic Engineering |
| Mechatronics         | Electronic Engineering |

---

## 5. Benefits of 3NF

### Eliminates Data Anomalies:
1. **Insert Anomaly:**
 - New departments can be added without requiring student data.
2. **Update Anomaly:**
 - Department updates only need to be done in the `Major and Department` table.
3. **Delete Anomaly:**
 - Deleting students does not remove department information.

### Improves Data Integrity:
- Each table is focused on a single purpose, reducing redundancy.

---

## 6. SQL Implementation of 3NF

```sql
-- Create Table 1: Student Details
CREATE TABLE student (
  student_id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  major VARCHAR(100)
);

-- Create Table 2: Major and Department Mapping
CREATE TABLE major_department (
  major VARCHAR(100) PRIMARY KEY,
  department VARCHAR(100)
);

-- Insert Data into Student Table
INSERT INTO student (student_id, first_name, last_name, major)
VALUES
(1000, 'Kira', 'Granger', 'Software Engineering'),
(1001, 'Katherine', 'Erlich', 'Data Science'),
(1002, 'Bill', 'Chamlin', 'Electronic'),
(1003, 'Shannon', 'Black', 'Mechatronics');

-- Insert Data into Major and Department Table
INSERT INTO major_department (major, department)
VALUES
('Software Engineering', 'Informatic Engineering'),
('Data Science', 'Informatic Engineering'),
('Electronic', 'Electronic Engineering'),
('Mechatronics', 'Electronic Engineering');
```
## 7. Summary
Key Points:
3NF eliminates transitive dependencies, improving database efficiency and integrity.
It splits data into logical tables based on dependencies.
Benefits of 3NF:
Prevents data anomalies.
Reduces redundancy.
Maintains data consistency.
## 8. Resources
PostgreSQL Documentation
Normalization Principles
