# 🛡️ Data Fundamentals: Admin & User Roles in My Online Electronics Shop DB

<div align="center">
  <img width="320" height="260" alt="Supabase Logo" src="https://github.com/user-attachments/assets/20661293-a214-4004-9042-657102fb0710" />
  <br/>
  <h3><b>Security & Role-Based Access Setup</b></h3>
</div>

---

## 📗 Table of Contents

* [📖 About the Security Setup](#about-security)
* [🧩 Role Overview](#role-overview)
* [🔐 Policy Implementation](#policy-implementation)
* [🧮 Example Role Queries](#example-queries)
* [🧠 Expected Outputs](#expected-outputs)
* [💾 Testing Roles in Supabase](#testing-roles)
* [👥 Author](#author)
* [🔭 Future Security Improvements](#future-security)
* [❓ FAQ](#faq)
* [📝 License](#license)

---

# 📖 About the Security Setup <a name="about-security"></a>

This file details the **Row Level Security (RLS)** setup for the **Online Electronics Shop SQL Database** built on **Supabase**.  
It defines how **Admins** and **Users** interact with the database safely and ensures that only authorized actions are performed.

The main goals are to:
- ✅ Enforce **least privilege** access control  
- ✅ Restrict **data visibility** to each user's records  
- ✅ Allow **Admins** to manage all records  
- ✅ Prevent **unauthorized data modification**

---

## 🧩 Role Overview <a name="role-overview"></a>

| Role | Description | Access Scope |
|------|--------------|--------------|
| **Admin** | Full access to all data (customers, products, orders) | Can read, insert, update, and delete any record |
| **User** | Restricted to their own customer data and orders | Can only view and manage their own records |

**Roles are stored in a `users` table** linked to Supabase Auth via `user_uuid`.

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE users (
  user_uuid UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  username TEXT NOT NULL,
  role TEXT CHECK (role IN ('admin', 'user')) DEFAULT 'user'
);

```

---

## 🔐 Policy Implementation <a name="policy-implementation"></a>

### Enable RLS on Main Tables

```sql
-- Enable Row Level Security (RLS) on all major tables
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE customers ENABLE ROW LEVEL SECURITY;
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;
ALTER TABLE products ENABLE ROW LEVEL SECURITY;

```

---

### User Policies

#### 1️⃣ Allow users to view only their own data
```sql
DROP POLICY IF EXISTS "Users can view their own customer data" ON customers;

CREATE POLICY "Users can view their own customer data"
ON customers
FOR SELECT
USING (user_uuid = auth.uid());

```

#### 2️⃣ Allow users to view their own orders
```sql
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

DROP POLICY IF EXISTS "Users can view their own orders" ON orders;

CREATE POLICY "Users can view their own orders"
ON orders
FOR SELECT
USING (user_uuid = auth.uid());

```

#### 3️⃣ Allow users to add their own orders
```sql
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

DROP POLICY IF EXISTS "Users can insert their own orders" ON orders;

CREATE POLICY "Users can insert their own orders"
ON orders
FOR INSERT
WITH CHECK (user_uuid = auth.uid());

```

#### 4️⃣ Allow users to read all products (no restrictions)
```sql
ALTER TABLE products ENABLE ROW LEVEL SECURITY;

DROP POLICY IF EXISTS "Users can view all products" ON products;

CREATE POLICY "Users can view all products"
ON products
FOR SELECT
USING (true);

```

---

### Admin Policies

#### 1️⃣ Admins can manage all customers
```sql
CREATE POLICY "Admins can manage all customers"
ON customers
FOR ALL
USING (EXISTS (SELECT 1 FROM users WHERE user_uuid = auth.uid() AND role = 'admin'));
```

#### 2️⃣ Admins can manage all orders
```sql
CREATE POLICY "Admins can manage all orders"
ON orders
FOR ALL
USING (EXISTS (SELECT 1 FROM users WHERE user_uuid = auth.uid() AND role = 'admin'));
```

#### 3️⃣ Admins can manage all products
```sql
CREATE POLICY "Admins can manage all products"
ON products
FOR ALL
USING (EXISTS (SELECT 1 FROM users WHERE user_uuid = auth.uid() AND role = 'admin'));
```

---

## 🧮 Example Role Queries <a name="example-queries"></a>

### 🧑‍💻 User Role Examples

**View own orders**
```sql

select * from orders;
```

**Add a new order**
```sql
INSERT INTO orders (user_uuid, customer_id, product_id, quantity, total_amount)
VALUES (auth.uid(), 1, 3, 1, 60000.00);
```

**Delete own order**
```sql
DELETE FROM orders
WHERE order_id = 3 AND user_uuid = auth.uid();
```

---

### 🧑‍🏫 Admin Role Examples

**View all orders**
```sql
SELECT * FROM orders;
```

**Update product stock**
```sql
UPDATE products SET stock_quantity = 8 WHERE product_id = 3;
```

**Delete any record**
```sql
DELETE FROM customers WHERE customer_id = 5;
```

---

## 🧠 Expected Outputs <a name="expected-outputs"></a>

### User Role:
✅ Can read their own data only  
🚫 Cannot access other users’ data  
✅ Can insert orders for their account  
🚫 Cannot delete or modify others’ orders

### Admin Role:
✅ Full access to all tables  
✅ Can perform CRUD operations  
✅ Can manage products and customers globally  

---

## 💾 Testing Roles in Supabase <a name="testing-roles"></a>

1. Go to **Supabase Dashboard → Authentication → Users**  
2. Create two test accounts:
   - **Admin account** (role = ‘admin’)
   - **User account** (role = ‘user’)
3. Sign in as each and run CRUD operations to verify policy behavior  
4. Observe Supabase’s console output for denied operations  

---

## 👥 Author <a name="author"></a>

👤 **Michael Musyoki**  
- GitHub: [@musyokimichael](https://github.com/musyokimichael)  
- LinkedIn: [@michaelmusyoki](https://linkedin.com/in/michaelmusyoki)

---

## 🔭 Future Security Improvements <a name="future-security"></a>

- Add **inventory manager** role with limited stock access  
- Implement **audit logs** for admin actions  
- Add **encryption** for sensitive customer data  

---

## ❓ FAQ <a name="faq"></a>

**Q: Can users see other customers’ orders?**  
A: No. RLS ensures users can only access their own data via `auth.uid()` filter.

**Q: Can admins override RLS?**  
A: Only via Supabase SQL editor or admin policies.

**Q: Can this connect to a front-end app?**  
A: Yes — connect Supabase Auth with React, Next.js, or any dashboard UI.

---

## 📝 License <a name="license"></a>

This project’s security configuration follows the **MIT License**, consistent with the main project.

