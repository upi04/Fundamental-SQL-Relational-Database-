
# Composite Data

## What is Composite Data?

Composite Data is a combination of multiple pieces of data within a single field that provides detailed information without the need to add new columns.

### Example Scenario

#### Tabel Orders

A Data Engineer has created the `orders` table for use by a Data Analyst:

| order_id | order_date  | order_details |
|----------|-------------|---------------|
| 1        | 09-03-2023  | Baju          |
| 2        | 09-03-2023  | Celana        |
| 3        | 09-03-2023  | Jaket         |

#### Detail Orders

The Data Analyst says: 
> "I need additional information about `order_details` for analysis."

Details about `order_details` might include:
- Product name
- Quantity
- Price per quantity

However, rather than adding new columns, detailed information can be represented within the `order_details` field itself.

### Representing Composite Data

#### Initial Data
| order_id | order_date  | order_details |
|----------|-------------|---------------|
| 1        | 09-03-2023  | Baju          |

#### Detailed Representation

The `order_details` field can be updated to include more information as composite data:

| order_id | order_date  | order_details             |
|----------|-------------|---------------------------|
| 1        | 09-03-2023  | {“Baju”, 10, 15000}       |

#### Full Composite Data Example
| order_id | order_date  | order_details             |
|----------|-------------|---------------------------|
| 1        | 09-03-2023  | {“Baju”, 10, 15000}       |
| 2        | 09-03-2023  | {“Celana”, 15, 25000}     |
| 3        | 09-03-2023  | {“Jaket”, 30, 100000}     |

### Benefits of Composite Data
- Allows storing complex, structured data within a single field.
- Avoids adding multiple new columns to the table.

Composite Data provides a more detailed way of storing information while maintaining a clean database schema.

---
