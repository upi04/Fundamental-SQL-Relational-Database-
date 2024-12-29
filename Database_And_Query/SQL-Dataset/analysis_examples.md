# **Basic SQL Queries**

## **1. Basic SELECT Statement**

### **Basic Structure**
```sql
SELECT column1, column2
FROM table_name;
```

### **Example 1**: Display `ShoeName` and `Stocks` from the `shoes` table
```sql
SELECT ShoeName, Stocks
FROM shoes;
```
**Output**: This query retrieves the names of the shoes and their stock counts from the `shoes` table.

---

## **2. Basic Filtering Data**

### **Using the WHERE Clause**

#### **Example 1**: Display shoes with `Profit` greater than 5000
```sql
SELECT ShoeName, Profit
FROM shoes
WHERE Profit > 5000;
```
**Output**: This query retrieves the names and profits of shoes where the profit is greater than 5000.

---

#### **Example 2**: Search for `ShoeName` that starts with the letter 'F'
```sql
SELECT ShoeName, Stocks
FROM shoes
WHERE ShoeName LIKE 'F%';
```
**Output**: This query retrieves the names and stock counts of shoes where the name starts with the letter 'F'.

---

#### **Example 3**: Combining Conditions
Find all shoes with `Profit > 3000` and `Stocks > 3000`:
```sql
SELECT ShoeName, Profit, Stocks
FROM shoes
WHERE Profit > 3000 AND Stocks > 3000;
```
**Output**: This query retrieves the names, profits, and stock counts of shoes where the profit is greater than 3000 and the stock count is also greater than 3000.

---

## **3. Example with Another Table**

### **From the `medals` Table**

#### **Example 1**: Display all records where `Medal` count is greater than 3
```sql
SELECT Name, Sport, Medal
FROM medals
WHERE Medal > 3;
```

#### **Example 2**: Filter for athletes whose `Sport` is 'Basketball' and their `Country` is 'Philippine'
```sql
SELECT Name, Sport, Country, Medal
FROM medals
WHERE Sport = 'Basketball' AND Country = 'Philippine';
```

---

