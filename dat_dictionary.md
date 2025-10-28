# 📘 Data Dictionary — My Online Electronics Shop SQL Project

This data dictionary describes the structure, attributes, and relationships of the **Online Electronics Shop** database.

---

## 🧩 Database Overview

The database models a small **online electronics shop** that manages **customers**, **products**, and **orders**.  
It helps track sales, product inventory, and customer details.

---

## 🧱 Tables Summary

| Table Name | Description |
|-------------|-------------|
| `customers` | Stores customer details and registration data |
| `products`  | Stores information about products available in the shop |
| `orders`    | Records transactions linking customers and products |

---

## 🧍 Customers Table

| Column Name | Data Type | Constraints | Description |
|--------------|------------|--------------|--------------|
| `customer_id` | SERIAL | PRIMARY KEY | Unique identifier for each customer |
| `first_name` | VARCHAR(50) |  | Customer’s first name |
| `last_name` | VARCHAR(50) |  | Customer’s last name |
| `email` | VARCHAR(100) | UNIQUE, NOT NULL | Customer’s email address |
| `phone` | VARCHAR(20) |  | Customer’s phone number |
| `created_at` | TIMESTAMP | DEFAULT NOW() | Record creation timestamp |

---

## 🛒 Products Table

| Column Name | Data Type | Constraints | Description |
|--------------|------------|--------------|--------------|
| `product_id` | SERIAL | PRIMARY KEY | Unique identifier for each product |
| `product_name` | VARCHAR(100) | NOT NULL | Product name |
| `category` | VARCHAR(50) |  | Product category (e.g., Electronics, Accessories, Furniture) |
| `price` | DECIMAL(10,2) |  | Product price |
| `stock_quantity` | INT |  | Number of items in stock |
| `created_at` | TIMESTAMP | DEFAULT NOW() | Record creation timestamp |

---

## 📦 Orders Table

| Column Name | Data Type | Constraints | Description |
|--------------|------------|--------------|--------------|
| `order_id` | SERIAL | PRIMARY KEY | Unique identifier for each order |
| `customer_id` | INT | FOREIGN KEY → `customers(customer_id)` | References the customer who placed the order |
| `product_id` | INT | FOREIGN KEY → `products(product_id)` | References the product being purchased |
| `order_date` | TIMESTAMP | DEFAULT NOW() | The date and time when the order was placed |
| `quantity` | INT | CHECK (quantity > 0) | Number of units purchased |
| `total_amount` | DECIMAL(10,2) |  | Total price for the order (quantity × price) |

---

## 🔗 Relationships

| Relationship | Type | Description |
|---------------|------|-------------|
| `customers → orders` | One-to-Many | One customer can place many orders |
| `products → orders` | One-to-Many | One product can appear in many orders |

---

## 📊 Example ERD (Entity Relationship Diagram)
<img width="1152" height="715" alt="image" src="https://github.com/user-attachments/assets/d057f379-b1f2-44d9-a1df-6b49707dddf4" />

**Example query**
```sql
SELECT o.order_id, c.first_name, p.product_name, o.quantity, o.total_amount
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id;
```

##🧠 Notes

-Use Supabase to host and visualize this database.

-Designed for use with PostgreSQL.

-Can be extended for inventory management or customer analytics.

<p align="right">(<a href="#top">back to top</a>)</p> ```
