# SQL

## Database

## Schema

A schema is like a namespace in a database.<br>
Create a schema:
```SQL
CREATE SCHEMA sales;
```

Delete a schema
```SQL
DROP SCHEMA sales;
```

## Tables

Create a table in the database
```SQL
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INT,
    total NUMERIC
);
```

Create a table in the schema `sales`
```SQL
CREATE TABLE sales.orders (
    id SERIAL PRIMARY KEY,
    customer_id INT,
    total NUMERIC
);
```

Select the data from the table sales.orders

```SQL
SELECT * FROM sales.orders;
```


Describe a table
```SQL
SELECT column_name, data_type, is_nullable, column_default
FROM information_schema.columns
WHERE table_name = 'sales.orders';
```

update a table
```SQL
ALTER TABLE sales.orders ADD COLUMN Description TEXT;
ALTER TABLE sales.orders DROP COLUMN Description;
```

## ROWS

Insert value
```SQL
INSERT INTO sales.orders (id, customer_id, total)
VALUES (1, 124585, 35589);
```

Update a value
```SQL
UPDATE sales.orders
SET total = 0
WHERE id = 1;
```

Delete a value
```SQL
DELETE FROM sales.orders WHERE id = 1;
```




