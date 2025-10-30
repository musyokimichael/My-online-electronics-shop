# 📖 E-Commerce Online Shop (Posit + Supabase)

This project documents an end-to-end analytics workflow using **R (Posit)** and **Supabase** for an online electronics shop database.  
It demonstrates how to connect, query, and visualize data directly from a PostgreSQL database hosted on Supabase.

---

## **📊 Project Overview**

The database consists of three main tables:

- **customers** — customer details and registration info  
- **orders** — sales and transactions data  
- **products** — catalog of electronic products  

These datasets together form the foundation for analyzing business performance, sales trends, and customer behavior.

---

## **🔗 Connecting Posit (RStudio) to Supabase**

### **1. Install Required R Packages**
```r
install.packages("DBI")
install.packages("RPostgres")
install.packages("dplyr")
install.packages("ggplot2")
```

### **2. Create a Connection Script**
```r
library(DBI)
library(RPostgres)

connect_db <- function() {
  con <- dbConnect(
    RPostgres::Postgres(),
    dbname = "postgres",
    host = "your-instance-id.aws-1-region.supabase.com",
    port = 6543,
    user = "postgres",
    password = "your_database_password",
    sslmode = "require"
  )
  return(con)
}
```

### **3. Test the Connection**
```r
con <- connect_db()
dbListTables(con)
head(dbGetQuery(con, "SELECT * FROM customers LIMIT 5;"))
```

---
## output for the successful connection in posit

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/05c638b3-ff3e-4004-befb-be3072823c21" />


## **📈 Data Analysis in R**

### **1. Load and Preview Tables**

## Output for all customers

```r
dbGetQuery(con, "SELECT * FROM customers LIMIT 5;")
```
<img width="1366" height="630" alt="image" src="https://github.com/user-attachments/assets/cff32592-9669-45d6-8558-2066d28b8a48" />
##output for all products
```r
dbGetQuery(con, "SELECT * FROM products LIMIT 5;")
```
<img width="1366" height="630" alt="image" src="https://github.com/user-attachments/assets/5fdcb824-7e0b-4125-9129-91a15fdf873f" />

##output for all orders

```r
dbGetQuery(con, "SELECT * FROM orders LIMIT 5;")
```

<img width="1366" height="627" alt="image" src="https://github.com/user-attachments/assets/ad74b893-0cab-462c-bdcc-0a06b067c8d9" />

### **2. Visualization: Revenue by Category**

```r
revenue_by_category <- dbGetQuery(con, "
  SELECT 
    p.category,
    SUM(o.total_amount) AS total_revenue
  FROM orders o
  JOIN products p ON o.product_id = p.product_id
  GROUP BY p.category;
")

ggplot(revenue_by_category, aes(x = category, y = total_revenue, fill = category)) +
  geom_col() +
  labs(title = '📦 Revenue by Category',
       x = 'Category', y = 'Revenue (KES)') +
  theme_minimal()

```
## Output for revenue by category

<img width="1366" height="692" alt="image" src="https://github.com/user-attachments/assets/3536c6c7-a147-4d91-887e-1dc19efd8ef0" />

---

### **Current Inventory Levels by Product**

```r
products <- dbGetQuery(con, "SELECT * FROM products;")

ggplot(products, aes(x = reorder(product_name, stock_quantity), 
                     y = stock_quantity, 
                     fill = category)) +
  geom_col() +
  coord_flip() +
  labs(title = "Current Stock Levels by Product",
       x = "Product Name",
       y = "Stock Quantity") +
  theme_minimal()
```
## Visuals for Inventory Levels by Product

<img width="1366" height="634" alt="image" src="https://github.com/user-attachments/assets/95fee0b1-6a6b-42e4-bdc8-1c8abe71c0cc" />

---

## **💡 Why Use Posit with Supabase**
- Seamless cloud database integration  
- Reproducible R scripts and workflows  
- Flexible visualizations with `ggplot2`  
- Scalable, real-time analysis from live data  

---
