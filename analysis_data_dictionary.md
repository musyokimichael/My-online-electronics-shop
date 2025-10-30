# 📘 E-Commerce Online Shop — Data Dictionary

This document describes the structure, purpose, and relationships of all tables used in the **E-Commerce Online Shop** project.

---

## **Customers Table**
| Column Name | Data Type | Description |
|--------------|------------|--------------|
| customer_id  | SERIAL PRIMARY KEY | Unique identifier for each customer |
| first_name   | VARCHAR(50) | Customer's first name |
| last_name    | VARCHAR(50) | Customer's last name |
| email        | VARCHAR(100) UNIQUE | Customer email address |
| signup_date  | DATE | Date the customer registered |
| country      | VARCHAR(50) | Customer's country |

**Usage in R:**  
Used to identify top customers, regions, and registration trends.

---

## **Products Table**
| Column Name | Data Type | Description |
|--------------|------------|--------------|
| product_id   | SERIAL PRIMARY KEY | Unique product identifier |
| product_name | VARCHAR(100) | Product name |
| category     | VARCHAR(50) | Product category (e.g., phones, laptops) |
| price        | DECIMAL(10,2) | Unit price of the product |
| stock        | INT | Current stock quantity |

**Usage in R:**  
Used to analyze inventory, pricing, and top-selling product categories.

---

## **Orders Table**
| Column Name | Data Type | Description |
|--------------|------------|--------------|
| order_id     | SERIAL PRIMARY KEY | Unique identifier for each order |
| customer_id  | INT REFERENCES customers(customer_id) | Customer who placed the order |
| product_id   | INT REFERENCES products(product_id) | Product purchased |
| quantity     | INT | Quantity purchased |
| total_amount | DECIMAL(10,2) | Total amount for the order |
| order_date   | DATE | Date when the order was placed |

**Usage in R:**  
Used for calculating total revenue, sales trends, and product performance.

---

## **Relationships**
- **customers → orders:** One-to-many  
- **products → orders:** One-to-many  
- Enables analysis of purchase behavior and revenue across product categories and regions.

---

## **ERD Summary**
```
customers (1) ───< orders >───(1) products
```

---

**Data Source:** Supabase PostgreSQL  
**Analysis Environment:** RStudio (Posit)  
**Visualization Tools:** ggplot2, Power BI

---
