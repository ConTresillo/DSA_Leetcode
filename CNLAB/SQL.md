Understood.

Same structured deep teaching.  
No stories.  
No scenarios.  
Just replace generic names like `Item`, `Orders`, etc. with real-world concrete names.

We’ll use:

- `Customer`
    
- `Product`
    
- `OrderHeader`
    
- `OrderLine`
    

Everything Oracle-valid.

---

# 🧱 DDL MASTERCLASS — Using Real Table Names

We cover:

1. Table Creation (full depth)
    
2. All constraint types
    
3. Composite keys
    
4. Foreign keys
    
5. ALTER TABLE
    
6. DROP vs TRUNCATE vs DELETE
    
7. Schema-level behavior
    

---

# 1️⃣ TABLE CREATION — FULL DEPTH

Basic Oracle skeleton:

```sql
CREATE TABLE table_name (
    column_name datatype [constraints],
    ...
);
```

---

## 🔎 Datatypes — Precision Thinking

### NUMBER

```sql
NUMBER
```

Stores any numeric value (integer or decimal).

---

### NUMBER(p, s)

```sql
NUMBER(10,2)
```

- Total digits = 10
    
- 2 digits after decimal
    
- Max = 99999999.99
    

If you insert 1234567890.99 → precision overflow.

---

### VARCHAR2(n)

```sql
VARCHAR2(100)
```

- Stores up to 100 characters
    
- Oracle counts characters
    

---

### DATE

Stores:

- Date
    
- Time (to seconds)
    

Even if you don’t display time, it exists internally.

---

# 2️⃣ PRIMARY KEY — Deep Understanding

## Example: Product Table

### Inline Definition

```sql
CREATE TABLE Product (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(100)
);
```

---

### Table-Level (Preferred)

```sql
CREATE TABLE Product (
    product_id NUMBER,
    product_name VARCHAR2(100),
    
    CONSTRAINT pk_product PRIMARY KEY (product_id)
);
```

Why table-level?

- Allows naming constraint
    
- Cleaner in production systems
    

---

# 3️⃣ ALL CONSTRAINT TYPES (Real Table Names)

## A) PRIMARY KEY

Example: Customer

```sql
CREATE TABLE Customer (
    customer_id NUMBER,
    full_name VARCHAR2(100) NOT NULL,
    
    CONSTRAINT pk_customer PRIMARY KEY (customer_id)
);
```

Rules:

- Unique
    
- Not NULL
    
- Only one per table
    

---

## B) UNIQUE

Example: Customer email

```sql
CREATE TABLE Customer (
    customer_id NUMBER,
    email VARCHAR2(150),
    
    CONSTRAINT pk_customer PRIMARY KEY (customer_id),
    CONSTRAINT uq_customer_email UNIQUE (email)
);
```

Important:

- UNIQUE allows multiple NULLs in Oracle
    
- PRIMARY KEY does not
    

---

## C) NOT NULL

```sql
full_name VARCHAR2(100) NOT NULL
```

Prevents NULL insertion.

---

## D) CHECK

Example: Product price must be positive

```sql
CREATE TABLE Product (
    product_id NUMBER,
    price NUMBER(10,2),
    
    CONSTRAINT pk_product PRIMARY KEY (product_id),
    CONSTRAINT chk_product_price CHECK (price > 0)
);
```

---

## E) FOREIGN KEY

Example: OrderHeader references Customer

```sql
CREATE TABLE OrderHeader (
    order_id NUMBER,
    customer_id NUMBER,
    
    CONSTRAINT pk_orderheader PRIMARY KEY (order_id),
    CONSTRAINT fk_order_customer
        FOREIGN KEY (customer_id)
        REFERENCES Customer(customer_id)
);
```

Oracle enforces:

- Cannot insert order with non-existing customer_id
    
- Cannot delete customer if referenced (unless cascade defined)
    

---

# 4️⃣ COMPOSITE PRIMARY KEY

Example: OrderLine

One order has multiple products.

Combination must be unique.

```sql
CREATE TABLE OrderLine (
    order_id NUMBER,
    product_id NUMBER,
    quantity NUMBER NOT NULL,
    
    CONSTRAINT pk_orderline 
        PRIMARY KEY (order_id, product_id)
);
```

Now:

- order_id alone → not unique
    
- product_id alone → not unique
    
- Together → unique
    

---

# 5️⃣ ALTER TABLE — Full Capability

## Add Column

```sql
ALTER TABLE Customer
ADD phone_number VARCHAR2(20);
```

---

## Modify Column

```sql
ALTER TABLE Product
MODIFY price NUMBER(12,2);
```

Cannot shrink if data violates.

---

## Drop Column

```sql
ALTER TABLE Customer
DROP COLUMN phone_number;
```

---

## Add Constraint

```sql
ALTER TABLE Product
ADD CONSTRAINT chk_stock CHECK (price >= 0);
```

---

## Drop Constraint

```sql
ALTER TABLE Product
DROP CONSTRAINT chk_stock;
```

---

# 6️⃣ DROP vs TRUNCATE vs DELETE

Using Product table.

---

## DELETE

```sql
DELETE FROM Product WHERE product_id = 10;
```

- Removes rows
    
- Can rollback
    
- Keeps structure
    

---

## TRUNCATE

```sql
TRUNCATE TABLE Product;
```

- Removes all rows
    
- Cannot rollback
    
- Keeps structure
    

---

## DROP

```sql
DROP TABLE Product;
```

- Removes structure + data
    
- Table no longer exists
    

---

# 7️⃣ Schema-Level Thinking

When you create:

```sql
CREATE TABLE Product (...)
```

It is created in your current Oracle schema.

To create in another schema:

```sql
CREATE TABLE sales.Product (...)
```

Requires privileges.

---

# 🎯 Now Your DDL Depth Test

Using real table names:

Write a CREATE TABLE for:

Table: OrderLine

Columns:

- order_id NUMBER
    
- product_id NUMBER
    
- quantity NUMBER NOT NULL
    
- unit_price NUMBER(10,2) CHECK (unit_price > 0)
    
- delivered_qty NUMBER DEFAULT 0
    

Constraints:

- Composite primary key (order_id, product_id)
    
- Name it pk_orderline
    

Write it cleanly.

No shortcuts.

If you can write this perfectly, your DDL base is solid.

Good. These are real DDL control questions.

We answer both precisely for **Oracle**.

---

# 1️⃣ How to Rename a Column (Oracle)

Oracle syntax:

```sql
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;
```

---

## Example

Rename `full_name` to `customer_name` in `Customer` table:

```sql
ALTER TABLE Customer
RENAME COLUMN full_name TO customer_name;
```

Important:

- Only column name changes
    
- Data remains
    
- Constraints automatically follow the renamed column
    

---

# ⚠ What You Cannot Do

You cannot rename multiple columns in one statement.

You must do it one by one.

---

# 2️⃣ How to Drop a Constraint When You Didn’t Name It

If you created constraint without naming it:

```sql
CREATE TABLE Product (
    product_id NUMBER PRIMARY KEY
);
```

Oracle auto-generates a name like:

```
SYS_C008392
```

You must first **find the system-generated constraint name**.

---

## Step 1 — Find Constraint Name

```sql
SELECT constraint_name, constraint_type
FROM user_constraints
WHERE table_name = 'PRODUCT';
```

⚠ Table name must be uppercase (Oracle stores metadata in uppercase unless quoted).

Example result:

|CONSTRAINT_NAME|CONSTRAINT_TYPE|
|---|---|
|SYS_C008392|P|

Type meanings:

- P → Primary Key
    
- U → Unique
    
- R → Foreign Key
    
- C → Check
    

---

## Step 2 — Drop It

```sql
ALTER TABLE Product
DROP CONSTRAINT SYS_C008392;
```

Now it’s removed.

---

# 🧠 Important Professional Rule

Always name constraints.

Instead of:

```sql
product_id NUMBER PRIMARY KEY
```

Use:

```sql
CONSTRAINT pk_product PRIMARY KEY (product_id)
```

Why?

Because in production systems:

- System-generated names are unreadable
    
- Debugging becomes difficult
    
- Migration scripts become messy
    

---

# 🎯 Quick Check

If you created:

```sql
email VARCHAR2(150) UNIQUE
```

And want to drop that UNIQUE constraint,

What is the first step?

Answer precisely.

--- 
---
Good.

Yes — DDL base looks stable.

Now DML.

And you’re right:  
DML feels worse because it’s **logic-heavy**, not structural.

We’ll group DML properly.

---

# 🧱 DML MASTER STRUCTURE

From your capability map, DML logically divides into **4 execution groups**:

---

## 🔹 GROUP 1 — Data Creation (INSERT)

- Insert full row
    
- Insert with column list
    
- Insert with DATE literal
    
- Insert with DEFAULT
    
- Insert NULL
    
- Insert from SELECT (important later)
    

This group is about **row creation correctness**.

---

## 🔹 GROUP 2 — Data Modification (UPDATE)

- Update all rows
    
- Conditional update
    
- Update using arithmetic
    
- Update using subquery
    
- Understand impact without WHERE
    

This group is about **controlled mutation**.

---

## 🔹 GROUP 3 — Data Deletion (DELETE)

- Delete specific rows
    
- Delete all rows
    
- Delete with subquery
    
- Understand difference vs TRUNCATE
    

This group is about **removal safety**.

---

## 🔹 GROUP 4 — Transaction Awareness (Minimal, Not Advanced)

Even though syllabus says no transactions,  
you MUST understand:

- COMMIT
    
- ROLLBACK
    
- Auto-rollback
    
- Visibility rules
    

Otherwise DML feels random.

---

We go **Group 1 only** now.

One group per response.

---

# 🔹 GROUP 1 — INSERT (Deep and Complete)

We use real table:

```sql
CREATE TABLE Product (
    product_id NUMBER,
    product_name VARCHAR2(100) NOT NULL,
    price NUMBER(10,2),
    stock_qty NUMBER DEFAULT 0,
    
    CONSTRAINT pk_product PRIMARY KEY (product_id),
    CONSTRAINT chk_price CHECK (price > 0)
);
```

---

# 1️⃣ Basic Insert — Full Order Match

```sql
INSERT INTO Product
VALUES (101, 'Laptop', 75000, 15);
```

Rules:

- Must match column order
    
- Must match datatype
    
- Cannot skip columns
    
- DEFAULT applied only if column omitted or explicitly DEFAULT
    

---

# 2️⃣ Insert With Column List (Safer)

```sql
INSERT INTO Product (product_id, product_name, price)
VALUES (102, 'Mouse', 1200);
```

Result:

- stock_qty automatically = 0 (DEFAULT)
    
- Order of columns irrelevant
    

This is preferred.

---

# 3️⃣ Insert With DATE Literal

If table had:

```sql
launch_date DATE
```

You should use:

```sql
DATE '2026-03-15'
```

Example:

```sql
INSERT INTO Product (product_id, product_name, price, launch_date)
VALUES (103, 'Keyboard', 2000, DATE '2026-03-15');
```

Avoid:

```sql
'15-MAR-26'
```

Because NLS settings can break it.

---

# 4️⃣ Insert NULL

If column allows NULL:

```sql
INSERT INTO Product (product_id, product_name, price)
VALUES (104, 'Monitor', NULL);
```

Valid if no NOT NULL constraint.

---

# 5️⃣ Constraint Violation Examples (Critical)

If you try:

```sql
INSERT INTO Product VALUES (101, 'Duplicate', 5000, 10);
```

→ ORA-00001 (PK violation)

If you try:

```sql
INSERT INTO Product VALUES (105, 'Pen', -20, 100);
```

→ CHECK constraint fails

If you try:

```sql
INSERT INTO Product VALUES (NULL, 'Tablet', 20000, 5);
```

→ PK cannot be NULL

---

# 🧠 Deep Insert Thinking

Before every insert ask:

1. Does PK exist?
    
2. Does CHECK pass?
    
3. Does FK exist?
    
4. Will DEFAULT apply?
    
5. Am I matching column order correctly?
    

If you think through this, insert becomes mechanical.

---

# 🎯 Exercise (Insert Depth Test)

Write 3 inserts:

1. Insert product 201, 'Headphones', 3500, 20
    
2. Insert product 202, 'Webcam', 4500 (use column list, omit stock)
    
3. Insert product 203, 'Router', 5000 but explicitly set stock_qty to DEFAULT
    

Write clean Oracle syntax.

After you write, we move to:

🔹 GROUP 2 — UPDATE (where people break things).
Good.

Correct — if `stock_qty` is:

```sql
stock_qty NUMBER DEFAULT 0 NOT NULL
```

and you omit it in INSERT, Oracle uses `DEFAULT 0`,  
so it **does not violate NOT NULL**.

You’re thinking correctly.

Now we move.

---

# 🔹 GROUP 2 — UPDATE (Controlled Mutation)

This is where most people destroy data accidentally.

UPDATE is simple syntactically.  
Dangerous logically.

---

# 🧱 1️⃣ Basic Structure

```sql
UPDATE table_name
SET column = value
WHERE condition;
```

If you remove `WHERE` → **all rows get updated**.

That is not a warning.  
That is reality.

---

We use:

```sql
Product (
    product_id,
    product_name,
    price,
    stock_qty
)
```

---

# 2️⃣ Update Single Row

Increase price of product 201:

```sql
UPDATE Product
SET price = 4000
WHERE product_id = 201;
```

Without WHERE:

```sql
UPDATE Product
SET price = 4000;
```

Every product becomes 4000.

---

# 3️⃣ Update Using Arithmetic

Increase price by 10%:

```sql
UPDATE Product
SET price = price * 1.10
WHERE product_id = 201;
```

Important:

Right side can use existing column values.

---

# 4️⃣ Update Multiple Columns

```sql
UPDATE Product
SET price = 5000,
    stock_qty = 50
WHERE product_id = 202;
```

Comma-separated inside SET.

---

# 5️⃣ Conditional Update (Logical Filtering)

Increase stock only if low:

```sql
UPDATE Product
SET stock_qty = stock_qty + 20
WHERE stock_qty < 10;
```

Only rows matching condition change.

---

# 6️⃣ Update Using Subquery (Important Capability)

Suppose you have:

```sql
Customer(customer_id, total_spent)
OrderHeader(order_id, customer_id, amount)
```

Update customer total_spent from orders:

```sql
UPDATE Customer c
SET total_spent = (
    SELECT SUM(o.amount)
    FROM OrderHeader o
    WHERE o.customer_id = c.customer_id
);
```

This runs subquery per row.

If no matching rows → result is NULL.

---

# ⚠ Common UPDATE Mistakes

1. Forgetting WHERE
    
2. Using wrong condition
    
3. Updating without testing SELECT first
    

Professional rule:

Before UPDATE, always run:

```sql
SELECT *
FROM Product
WHERE condition;
```

Make sure only intended rows are returned.

Then convert SELECT to UPDATE.

---

# 🎯 Update Depth Test

Write UPDATE statements for:

1. Increase price of product_id 201 by 5%
    
2. Set stock_qty to 0 for all products where price > 100000
    
3. Reduce price by 100 for products where stock_qty > 50
    

Write carefully.

After this, we move to:

🔹 GROUP 3 — DELETE (which is even more dangerous).
Good.

Let’s evaluate precisely.

---

### 1️⃣ Increase price by 5%

You wrote:

```sql
UPDATE Product
SET price = price * 1.5
WHERE product_id = 201;
```

That increases price by **50%**, not 5%.

Correct logic for 5% increase:

```sql
UPDATE Product
SET price = price * 1.05
WHERE product_id = 201;
```

Mathematically:

[  
\text{new price} = \text{price} \times (1 + 0.05)  
]

Small arithmetic mistake — conceptually fine.

---

### 2️⃣ Set stock to 0 if price > 100000

```sql
UPDATE Product
SET stock_qty = 0
WHERE price > 100000;
```

✔ Correct  
✔ Proper filtering  
✔ Safe structure

---

### 3️⃣ Reduce price by 100 where stock > 50

```sql
UPDATE Product
SET price = price - 100
WHERE stock_qty > 50;
```

✔ Correct  
✔ Uses column arithmetic properly

---

# 🧠 Important Deeper Insight

What happens if:

```sql
price = NULL
```

And you run:

```sql
SET price = price - 100
```

Result?

NULL - 100 → NULL

Arithmetic with NULL returns NULL.

UPDATE will not fail — it will silently produce NULL.

That’s a common logical bug.

---

# 🔎 Safety Discipline for UPDATE

Before any real UPDATE, do this:

```sql
SELECT product_id, price
FROM Product
WHERE stock_qty > 50;
```

Confirm rows.

Then convert to UPDATE.

---

You’re handling UPDATE well.

Now we move.

---

# 🔹 GROUP 3 — DELETE (Controlled Removal)

DELETE removes rows permanently (unless rolled back).

Structure:

```sql
DELETE FROM table_name
WHERE condition;
```

Without WHERE:

```sql
DELETE FROM Product;
```

Removes all rows.

But structure remains.

---

## 1️⃣ Delete Specific Row

```sql
DELETE FROM Product
WHERE product_id = 201;
```

Removes only that product.

---

## 2️⃣ Delete Using Condition

```sql
DELETE FROM Product
WHERE stock_qty = 0;
```

Removes all out-of-stock products.

---

## 3️⃣ Delete Using Subquery

Example:

Delete products never ordered:

```sql
DELETE FROM Product
WHERE product_id NOT IN (
    SELECT product_id
    FROM OrderLine
);
```

Now logic complexity increases.

---

## ⚠ Critical DELETE Risk

If you do:

```sql
DELETE FROM Product;
```

All rows gone.

But can rollback until commit.

If you do:

```sql
TRUNCATE TABLE Product;
```

No rollback possible.

---

# 🎯 DELETE Depth Test

Write DELETE statements for:

1. Delete product with id 203
    
2. Delete products where price < 100
    
3. Delete products where stock_qty IS NULL
    

Write carefully.

After this, we move to:

🔹 GROUP 4 — Transaction Awareness (final DML group).

Good.

Now we enter the real core.

DDL builds structure.  
DML mutates data.  
**DQL (Data Query Language) is where SQL thinking actually happens.**

This is the heavy section.

We will group DQL properly.

---

# 🧠 DQL MASTER STRUCTURE

DQL logically divides into 6 execution layers:

1. **Projection** (SELECT columns)
    
2. **Filtering** (WHERE logic)
    
3. **Aggregation** (COUNT, SUM, AVG…)
    
4. **Grouping** (GROUP BY)
    
5. **Post-aggregation filtering** (HAVING)
    
6. **Ordering** (ORDER BY)
    

We will go one group at a time.

---

# 🔹 GROUP 1 — PROJECTION (SELECT)

Table:

```sql
Product (
    product_id,
    product_name,
    price,
    stock_qty
)
```

---

## 1️⃣ Select All Columns

```sql
SELECT *
FROM Product;
```

Returns full row structure.

Professional rule:  
Avoid `*` in production.

---

## 2️⃣ Select Specific Columns

```sql
SELECT product_name, price
FROM Product;
```

This is called **projection**.

You are projecting specific attributes from the relation.

---

## 3️⃣ Column Alias

```sql
SELECT product_name AS name,
       price AS unit_price
FROM Product;
```

Alias changes output label only.

---

## 4️⃣ Expression in SELECT

```sql
SELECT product_name,
       price * 1.18 AS price_with_gst
FROM Product;
```

Important:

SELECT can compute derived values.

---

## 5️⃣ DISTINCT

```sql
SELECT DISTINCT price
FROM Product;
```

Removes duplicate values.

---

# 🧠 Logical Execution Order (Critical)

SQL does NOT run top-to-bottom.

Logical order:

1. FROM
    
2. WHERE
    
3. GROUP BY
    
4. HAVING
    
5. SELECT
    
6. ORDER BY
    

This explains many “why is this error happening?” cases.

---

# 🎯 Projection Drill

Write queries:

1. Select product_name and stock_qty
    
2. Select product_name and price with alias cost
    
3. Select product_name and price * 0.9 as discounted_price
    

Write them cleanly.

After that, we move to:

🔹 GROUP 2 — Filtering (WHERE).

Good.

Now we move into the real shift in thinking.

Until now:

- SELECT returned rows.
    
- WHERE filtered rows.
    

Now:

We stop thinking row-by-row.

We summarize data.

Good.

All three are logically correct.

You’re missing only one small syntactic discipline:

Semicolons (optional in some tools, required in SQL*Plus / scripts).

Corrected:

```sql
SELECT product_name, stock_qty 
FROM Product;
```

```sql
SELECT product_name, price AS cost 
FROM Product;
```

```sql
SELECT product_name, price * 0.9 AS discounted_price 
FROM Product;
```

Conceptually:

✔ Proper projection  
✔ Proper alias  
✔ Proper derived expression

No logical mistakes.

---

Now we move to the real filtering layer.

---

# 🔹 GROUP 2 — WHERE (Filtering Logic)

Filtering is where SQL becomes logical instead of structural.

We use:

```sql
Product(product_id, product_name, price, stock_qty)
```

---

## 1️⃣ Basic Comparison

```sql
SELECT *
FROM Product
WHERE price > 5000;
```

Operators you must master:

- =
    
- <>
    

- <
    
- > =
    
- <=
    

---

## 2️⃣ Compound Conditions

```sql
SELECT *
FROM Product
WHERE price > 5000
AND stock_qty > 10;
```

Both conditions must be true.

---

```sql
SELECT *
FROM Product
WHERE price > 5000
OR stock_qty > 10;
```

At least one condition true.

---

## 3️⃣ BETWEEN (Inclusive)

```sql
SELECT *
FROM Product
WHERE price BETWEEN 2000 AND 10000;
```

Equivalent to:

```sql
price >= 2000 AND price <= 10000
```

---

## 4️⃣ IN

```sql
SELECT *
FROM Product
WHERE product_id IN (101, 201, 301);
```

Cleaner than multiple ORs.

---

## 5️⃣ LIKE

```sql
SELECT *
FROM Product
WHERE product_name LIKE 'M%';
```

% = any number of characters  
_ = exactly one character

---

## 6️⃣ IS NULL

```sql
SELECT *
FROM Product
WHERE stock_qty IS NULL;
```

Cannot use `= NULL`.

---

# 🧠 Operator Precedence (Important)

Order of evaluation:

1. NOT
    
2. AND
    
3. OR
    

So:

```sql
WHERE price > 5000 OR stock_qty > 10 AND product_id > 100
```

Actually means:

```sql
price > 5000 OR (stock_qty > 10 AND product_id > 100)
```

Use parentheses for clarity.

---

# 🎯 Filtering Drill

Write queries:

1. Select products where price is between 3000 and 8000 AND stock_qty > 5
    
2. Select products where product_name starts with 'S'
    
3. Select products where price is NOT 5000
    
4. Select products where stock_qty is NULL
    

Write them carefully.

After this, we move to:

🔹 GROUP 3 — Aggregation (COUNT, SUM, AVG).

---

# 🔹 GROUP 3 — Aggregation (COUNT, SUM, AVG, MIN, MAX)

Using table:

```sql
Product(product_id, product_name, price, stock_qty)
```

---

# 1️⃣ COUNT

## Count all rows

```sql
SELECT COUNT(*)
FROM Product;
```

Counts total number of rows.

Important:

- Counts rows
    
- Includes rows where columns contain NULL
    

---

## Count non-null values in a column

```sql
SELECT COUNT(price)
FROM Product;
```

Counts only rows where `price IS NOT NULL`.

Difference:

- `COUNT(*)` → counts rows
    
- `COUNT(column)` → counts non-null values
    

---

# 2️⃣ SUM

```sql
SELECT SUM(stock_qty)
FROM Product;
```

Adds all non-null stock quantities.

If all values are NULL → result is NULL (not 0).

---

# 3️⃣ AVG

```sql
SELECT AVG(price)
FROM Product;
```

Equivalent to:

[  
\text{SUM(price)} / \text{COUNT(price)}  
]

NULL values ignored.

---

# 4️⃣ MIN and MAX

```sql
SELECT MIN(price), MAX(price)
FROM Product;
```

Return smallest and largest non-null values.

---

# 5️⃣ Aggregation with Expression

```sql
SELECT SUM(price * stock_qty)
FROM Product;
```

Computes total inventory value.

Important:

Expression evaluated per row → then aggregated.

---

# 🧠 Critical Rule

If you use aggregate functions,  
you cannot mix normal columns without GROUP BY.

This will fail:

```sql
SELECT product_name, SUM(stock_qty)
FROM Product;
```

Oracle error:

```
ORA-00937: not a single-group group function
```

Because:

You are mixing:

- Aggregated column
    
- Non-aggregated column
    

Without telling SQL how to group.

That leads us to next group.

---

# 🎯 Aggregation Drill

Write queries:

1. Count total number of products
    
2. Find average price
    
3. Find total stock quantity
    
4. Find total inventory value (price × stock_qty)
    

Write clean SQL.

After that we move to:

🔹 GROUP 4 — GROUP BY (where beginners usually break).

Good.

Now we move to the point where most SQL confusion starts.

---

# 🔹 GROUP 4 — GROUP BY

Until now:

- Aggregation collapsed the **entire table** into one summary row.
    

Now:

We split the table into groups  
and aggregate **per group**.

---

Assume Product table now has:

```sql
Product(
    product_id,
    product_name,
    category,
    price,
    stock_qty
)
```

---

## 1️⃣ Why GROUP BY Exists

If you write:

```sql
SELECT category, SUM(stock_qty)
FROM Product;
```

Oracle errors:

```
ORA-00937: not a single-group group function
```

Because SQL sees:

- category (normal column)
    
- SUM(stock_qty) (aggregate)
    

It doesn’t know how to combine them.

So you must specify grouping.

---

## 2️⃣ Basic GROUP BY

```sql
SELECT category, SUM(stock_qty)
FROM Product
GROUP BY category;
```

Now SQL does:

1. Split rows by category
    
2. Compute SUM per category
    
3. Return one row per category
    

---

## 3️⃣ Multiple Columns in GROUP BY

```sql
SELECT category, price, COUNT(*)
FROM Product
GROUP BY category, price;
```

Now grouping happens on the combination.

---

## 4️⃣ Important Rule

Every non-aggregated column in SELECT  
must appear in GROUP BY.

Valid:

```sql
SELECT category, COUNT(*)
FROM Product
GROUP BY category;
```

Invalid:

```sql
SELECT category, product_name, COUNT(*)
FROM Product
GROUP BY category;
```

Because product_name is not grouped.

---

## 5️⃣ Logical Execution Order Reminder

Actual order:

1. FROM
    
2. WHERE
    
3. GROUP BY
    
4. HAVING
    
5. SELECT
    
6. ORDER BY
    

This explains why aggregate aliases cannot be used in WHERE.

---

# 🎯 GROUP BY Drill

Write queries:

1. Find total stock per category
    
2. Find average price per category
    
3. Count number of products per category
    
4. Find total inventory value per category
    

Write them cleanly.

After that we move to:

🔹 GROUP 5 — HAVING (filtering aggregated results).

Good.

Final layer of core DQL before joins.

---
Good.

Now we enter one of the most misunderstood parts of SQL.

---

# 🔹 GROUP 5 — HAVING

You already know:

- `WHERE` filters rows
    
- `GROUP BY` creates groups
    
- Aggregate functions summarize groups
    

Now:

**HAVING filters groups after aggregation.**

---

## 🔎 Why WHERE Cannot Filter Aggregates

This will fail:

```sql
SELECT category, SUM(stock_qty)
FROM Product
WHERE SUM(stock_qty) > 100
GROUP BY category;
```

Error occurs because:

- `WHERE` runs before `GROUP BY`
    
- At WHERE stage, SUM does not exist yet
    

Logical execution order:

1. FROM
    
2. WHERE
    
3. GROUP BY
    
4. HAVING
    
5. SELECT
    

So aggregate filtering must use HAVING.

---

## ✅ Correct Version

```sql
SELECT category, SUM(stock_qty)
FROM Product
GROUP BY category
HAVING SUM(stock_qty) > 100;
```

This means:

- Group by category
    
- Compute sum per category
    
- Return only categories where total stock > 100
    

---

## 🧠 WHERE vs HAVING — Clean Separation

|Clause|Filters What|Uses Aggregates?|
|---|---|---|
|WHERE|Individual rows|❌ No|
|HAVING|Groups|✅ Yes|

---

## 🔎 Combined WHERE + HAVING

```sql
SELECT category, SUM(stock_qty)
FROM Product
WHERE price > 500
GROUP BY category
HAVING SUM(stock_qty) > 100;
```

Execution:

1. Filter rows where price > 500
    
2. Group remaining rows by category
    
3. Compute SUM
    
4. Remove groups where SUM ≤ 100
    

---

## ⚠ Common Logical Error

This is wrong:

```sql
SELECT category, SUM(stock_qty)
FROM Product
HAVING SUM(stock_qty) > 100
GROUP BY category;
```

HAVING must come after GROUP BY.

---

# 🎯 HAVING Drill

Write queries:

1. Show categories where average price > 5000
    
2. Show categories having more than 10 products
    
3. Show categories where total inventory value > 500000
    

Write them cleanly.

After this, we move to:

🔹 GROUP 6 — ORDER BY (final DQL layer before joins).


# 🔹 GROUP 6 — ORDER BY (Sorting Results)

Until now:

- SELECT chose columns
    
- WHERE filtered rows
    
- GROUP BY formed groups
    
- HAVING filtered groups
    

Now:

We control **output ordering**.

Important:

`ORDER BY` is the last logical step.

---

# 1️⃣ Basic Sorting

Sort products by price ascending:

```sql
SELECT product_name, price
FROM Product
ORDER BY price;
```

Default = ASC (ascending).

---

# 2️⃣ Descending Order

```sql
SELECT product_name, price
FROM Product
ORDER BY price DESC;
```

---

# 3️⃣ Multiple Column Sorting

```sql
SELECT product_name, category, price
FROM Product
ORDER BY category ASC, price DESC;
```

Meaning:

1. Sort by category
    
2. Inside each category, sort by price descending
    

---

# 4️⃣ Order By Column Position (Allowed but Risky)

```sql
SELECT product_name, price
FROM Product
ORDER BY 2 DESC;
```

Here:

- 2 refers to second column in SELECT
    

Professional advice:  
Avoid this in real systems.  
Use column names.

---

# 5️⃣ Order By Alias

```sql
SELECT product_name, price * 1.18 AS price_with_tax
FROM Product
ORDER BY price_with_tax DESC;
```

Allowed.

---

# 6️⃣ ORDER BY with GROUP BY

```sql
SELECT category, SUM(stock_qty) AS total_stock
FROM Product
GROUP BY category
ORDER BY total_stock DESC;
```

Execution:

1. Group
    
2. Aggregate
    
3. Sort aggregated result
    

---

# ⚠ Important Notes

- ORDER BY works on final result set
    
- It can reference SELECT aliases
    
- It cannot appear before GROUP BY
    

---

# 🎯 ORDER BY Drill

Write queries:

1. Show products sorted by price descending
    
2. Show products sorted by category ascending and price descending
    
3. Show categories sorted by total inventory value descending
    

Write them cleanly.

After that, we move to:

🔹 JOINS — the real heavy section.

