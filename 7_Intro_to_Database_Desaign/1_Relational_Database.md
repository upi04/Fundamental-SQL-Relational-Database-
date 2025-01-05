# Advantages of Relational Databases

Relational databases are widely used because they provide a structured approach to storing and retrieving data. Below are some of the primary advantages, along with illustrative examples.

---

## 1. Reduced Data Duplication

By organizing data into tables with relationships, **redundant data** can be minimized. This process is usually handled through **database normalization**.

**Example:**

| teacher_id | name  | age | subject    |
|------------|-------|-----|-----------|
| 1001       | Ann   | 23  | Math      |
| 1001       | Ann   | 23  | Biology   |
| 1002       | James | 29  | History   |
| 1003       | Enola | 27  | Chemistry |
| 1003       | Enola | 27  | Physics   |

- Notice that **Ann** (teacher_id `1001`) appears multiple times for different subjects. In a properly normalized design, you’d separate teacher information into one table and subject information into another, then link them via a **relation**.

---

## 2. Organized Data in Readable Categories

Data can be **categorized** into different tables (entities), making it easier to understand and maintain.

**Example categories**:
- **Student**, **Class**, **Teacher**
- **Department**, **Subject**, **Grade**

By separating each category into its own table, you keep the data organized and more readable.

---

## 3. Consistent and Easily Navigable Data

When a piece of data changes in a relational database, the changes can cascade to all related records. This ensures data consistency across the system.

- **Example**: If a **class** name changes, all student records that reference that class can also update automatically (if properly set up via foreign keys and cascading rules).
- Searching for students by class, or updating class information for multiple students, becomes straightforward due to the relational structure.

**Diagram (Illustrative Relational Model)**:
