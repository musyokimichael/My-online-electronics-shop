# My-Online-Electronics-Shop SQL
This is an SQL relational database detailing customers ,orders/sales and products for my online electronics shop
Student_ID	Gender	Income_Source	Savings_Frequency	Uses_KCB_App	Monthly_Savings_Ksh	Financial_Wellbeing_Score	Semester

# My Bookstore SQL Project

<a name="readme-top"></a>

<!-- TABLE OF CONTENTS -->

# 📗 Table of Contents

- [My SQL Project](#about-project)
- [📗 Table of Contents](#-table-of-contents)
- [📖 My SQL Project](#about-project)
  - [🛠 Built With ](#-built-with-)
    - [Tech Stack ](#tech-stack-)
    - [Key Features ](#key-features-)
  - [💻 Getting Started ](#-getting-started-)
    - [Prerequisites](#prerequisites)
    - [Setup](#setup)
    - [Usage](#usage)
  - [👥 Authors ](#-authors-)
  - [🔭 Future Features ](#-future-features-)
  - [🤝 Contributing ](#-contributing-)

<!-- PROJECT DESCRIPTION -->

# 📖 My SQL Project <a name="about-project"></a>

**My SQL Project** is a simple Database that uses SQL, Postgres via Supabase and R to create, query and secure a **Bookstore** database.

## 🛠 Built With <a name="built-with"></a>

### Tech Stack <a name="tech-stack"></a>
- SQL
- Postgres DB

<!-- Features -->

### Key Features <a name="key-features"></a>

- [ ] **Tables**
- [ ] **Schema**
- [ ] **Access control**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## 💻 Getting Started <a name="getting-started"></a>

To rebuild this DB, follow these steps.

### Prerequisites

To run this project, you need:
- [A Supabase account](https://supabase.com/)
- [Knowledge on SQL](https://www.w3schools.com/sql/)
- A schema for creating your tables in the DB

<!-- ### Setup -->
### Setup

Copy the contents of this Readme.md to your Project's file

OR

Clone this repository to your desired folder:

```sh
  git clone https://github.com/joyapisi/readme-template-data
  cd budget-app
```

<!-- ### DB Creation -->

### DB Schema

- The DB is made up of 3 tables. Eaach table has 5 entries.
- To create the table, you will need a schema as shown below:

```sql

-- Drop tables if they exist
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS customers;

-- Customers Table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT NOW()
);
-- Products Table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock_quantity INT,
    created_at TIMESTAMP DEFAULT NOW()
);
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    product_id INT REFERENCES products(product_id),
    order_date TIMESTAMP DEFAULT NOW(),
    quantity INT CHECK (quantity > 0),
    total_amount DECIMAL(10,2)
);
-- Insert sample data Customers
INSERT INTO customers (first_name, last_name, email, phone) VALUES
('Alice', 'Wanjiku', 'alice@example.com', '0722000001'),
('Brian', 'Otieno', 'brian@example.com', '0722000002'),
('Clara', 'Mutua', 'clara@example.com', '0722000003'),
('Daniel', 'Mwangi', 'daniel@example.com', '0722000004'),
('Eva', 'Kariuki', 'eva@example.com', '0722000005');

-- Insert sample data Products
INSERT INTO products (product_name, category, price, stock_quantity) VALUES
('Laptop', 'Electronics', 85000.00, 10),
('Wireless Mouse', 'Accessories', 1500.00, 50),
('Smartphone', 'Electronics', 60000.00, 20),
('Headphones', 'Accessories', 3500.00, 30),
('Office Chair', 'Furniture', 12000.00, 15);

-- Insert sample data Orders
INSERT INTO orders (customer_id, product_id, quantity, total_amount) VALUES
(1, 1, 1, 85000.00),
(2, 2, 2, 3000.00),
(3, 3, 1, 60000.00),
(4, 5, 1, 12000.00),
(5, 4, 3, 10500.00);



```

- The Tables should look like this in Supabase:
CUSTOMERS
<img width="1398" height="307" alt="image" src="https://github.com/user-attachments/assets/826e7b13-1a0a-49aa-b5a1-84b7ca2c4c50" />

ORDERS
<img width="1438" height="377" alt="image" src="https://github.com/user-attachments/assets/1f344bf8-c3cc-4f93-86b2-632a1dd5bc46" />

PRODUCTS
<img width="1418" height="283" alt="image" src="https://github.com/user-attachments/assets/d8d9545e-5ab6-4598-9850-ab00f2894b61" />

- The ERD screenshot from Supabase looks like this: 
<img width="1152" height="715" alt="image" src="https://github.com/user-attachments/assets/d057f379-b1f2-44d9-a1df-6b49707dddf4" />

- To test the table, I used two queries: 

```sql
SELECT * FROM customers;
````

```sql
SELECT o.order_id, c.first_name, p.product_name, o.quantity, o.total_amount
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id;

````

- Here are the results of the queries:
<img width="1401" height="772" alt="image" src="https://github.com/user-attachments/assets/c7f8a166-8240-4a49-8847-99b2fe957413" />

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- AUTHORS -->

## 👥 Authors <a name="authors"></a>

👤 **Michael Mati**
- GitHub: [@musyokimichael](https://github.com/musyokimichael)
- LinkedIn: [@michaelmusyoki](https://linkedin.com/in/michaelmusyoki)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- FUTURE FEATURES -->

## 🔭 Future Features <a name="future-features"></a>

- [ ] **Add security**
- [ ] **Link DB to R for visualisation purposes and further analyses**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->

## 🤝 Contributing <a name="contributing"></a>

Contributions, issues, and feature requests are welcome!

Feel free to check the [issues page](../../issues/).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- SUPPORT -->
