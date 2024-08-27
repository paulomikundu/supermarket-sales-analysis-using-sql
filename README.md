# supermarket-sales-analysis-using-sql
-- Create database
CREATE DATABASE IF NOT EXISTS supermarket_db;
USE supermarket_db;

-- Employees table
CREATE TABLE Employees3 (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    hire_date DATE,
    salary DECIMAL(10, 2),
    address VARCHAR(255),
    email VARCHAR(100),
    phone_number VARCHAR(20),
    birth_date DATE,
    gender VARCHAR(10)
);

-- Vegetables table
CREATE TABLE Vegetables3 (
    vegetable_id INT PRIMARY KEY,
    name VARCHAR(100),
    variety VARCHAR(50),
    price_per_kg DECIMAL(10, 2),
    stock_quantity INT,
    supplier VARCHAR(100),
    origin VARCHAR(100),
    harvest_date DATE,
    expiration_date DATE,
    weight_per_unit DECIMAL(10, 2)
);

-- Fruits table
CREATE TABLE Fruits3 (
    fruit_id INT PRIMARY KEY,
    name VARCHAR(100),
    variety VARCHAR(50),
    price_per_kg DECIMAL(10, 2),
    stock_quantity INT,
    supplier VARCHAR(100),
    origin VARCHAR(100),
    harvest_date DATE,
    expiration_date DATE,
    weight_per_unit DECIMAL(10, 2)
);

-- Groceries table
CREATE TABLE Groceries3 (
    grocery_id INT PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2),
    stock_quantity INT,
    supplier VARCHAR(100),
    origin VARCHAR(100),
    purchase_date DATE,
    expiration_date DATE,
    weight_per_unit DECIMAL(10, 2)
);

-- Sales table
CREATE TABLE Sales3 (
    sale_id INT PRIMARY KEY,
    product_id INT,
    employee_id INT,
    quantity DECIMAL(10, 2),
    sale_date DATE,
    FOREIGN KEY (product_id) REFERENCES Groceries(grocery_id),
    FOREIGN KEY (employee_id) REFERENCES Employees(employee_id)
);

-- Inserting sample data into Employees table
INSERT INTO Employees3 VALUES
(1, 'John Doe', 'Sales', '2020-01-01', 50000.00, '123 Main St, City', 'john.doe@example.com', '123-456-7890', '1985-05-10', 'Male'),
(2, 'Jane Smith', 'Marketing', '2019-05-15', 60000.00, '456 Elm St, Town', 'jane.smith@example.com', '987-654-3210', '1987-08-25', 'Female'),
(3, 'Alice Johnson', 'Finance', '2021-03-10', 55000.00, '789 Oak St, Village', 'alice.johnson@example.com', '456-789-0123', '1990-12-15', 'Female');

-- Inserting sample data into Vegetables table
INSERT INTO Vegetables3 VALUES
(1, 'Carrots', 'Baby Carrots', 1.50, 200, 'Farm Fresh Produce', 'India', '2024-04-01', '2024-04-15', 0.2),
(2, 'Tomatoes', 'Red Tomatoes', 2.00, 150, 'Fresh Farms', 'India', '2024-04-02', '2024-04-16', 0.3),
(3, 'Spinach', 'Green Spinach', 1.75, 180, 'Organic Harvest', 'India', '2024-04-03', '2024-04-17', 0.25);

-- Inserting sample data into Fruits table
INSERT INTO Fruits3 VALUES
(1, 'Apples', 'Red Apples', 2.00, 190, 'Apple Orchard', 'India', '2024-04-04', '2024-04-18', 0.4),
(2, 'Bananas', 'Yellow Bananas', 1.50, 220, 'Tropical Farms', 'India', '2024-04-05', '2024-04-19', 0.3),
(3, 'Oranges', 'Navel Oranges', 1.75, 200, 'Citrus Farms', 'India', '2024-04-06', '2024-04-20', 0.35);

-- Inserting sample data into Groceries table
INSERT INTO Groceries3 VALUES
(1, 'Milk', 'Dairy', 2.00, 100, 'Local Dairy Farm', 'India', '2024-04-07', '2024-04-21', 1.0),
(2, 'Bread', 'Bakery', 1.50, 120, 'Local Bakery', 'India', '2024-04-08', '2024-04-22', 0.5),
(3, 'Rice', 'Grains', 3.00, 80, 'Local Rice Mill', 'India', '2024-04-09', '2024-04-23', 1.0);

-- Inserting sample data into Sales table
INSERT INTO Sales3 VALUES
(1, 1, 1, 2.5, '2024-04-10'),
(2, 2, 2, 1.8, '2024-04-11'),
(3, 3, 3, 3.2, '2024-04-12');
-- Retrieve all employees
SELECT * FROM Employees3;

-- Retrieve all vegetables
SELECT * FROM Vegetables3;

-- Retrieve all fruits
SELECT * FROM Fruits3;

-- Retrieve all groceries
SELECT * FROM Groceries3;

-- Retrieve all sales
SELECT * FROM Sales3;

-- Retrieve employees working in the Sales department
SELECT * FROM Employees3 WHERE department = 'Sales';

-- Retrieve vegetables with a stock quantity greater than 100
SELECT * FROM Vegetables1 WHERE stock_quantity > 100;

-- Retrieve fruits with a price per kg less than 2.00
SELECT * FROM Fruits3 WHERE price_per_kg < 2.00;

-- Retrieve groceries purchased after '2024-04-08'
SELECT * FROM Groceries3 WHERE purchase_date > '2024-04-08';

-- Retrieve sales made by employee with ID 2
SELECT * FROM Sales3 WHERE employee_id = 2;
SELECT 
    s.sale_id,
    e.name AS employee_name,
    g.name AS product_name,
    s.quantity,
    s.sale_date
FROM 
    Sales3 s
JOIN 
    Employees3 e ON s.employee_id = e.employee_id
JOIN 
    Groceries3 g ON s.product_id = g.grocery_id;
-- INNER JOIN
SELECT e.*, s.*
FROM Employees3 e
INNER JOIN Sales3 s ON e.employee_id = s.employee_id;

-- LEFT JOIN
SELECT e.*, s.*
FROM Employees3 e
LEFT JOIN Sales3 s ON e.employee_id = s.employee_id;

-- RIGHT JOIN
SELECT e.*, s.*
FROM Employees3 e
RIGHT JOIN Sales3 s ON e.employee_id = s.employee_id;

-- FULL OUTER JOIN (MySQL does not directly support FULL OUTER JOIN, so we can simulate it using LEFT JOIN and UNION ALL)
SELECT e.*, s.*
FROM Employees3 e
LEFT JOIN Sales3 s ON e.employee_id = s.employee_id
UNION ALL
SELECT e.*, s.*
FROM Employees3 e
RIGHT JOIN Sales3 s ON e.employee_id = s.employee_id
WHERE e.employee_id IS NULL;

-- CROSS JOIN
SELECT e.*, s.*
FROM Employees3 e
CROSS JOIN Sales3 s;
-- Join Sales with Fruits
SELECT s.*, f.*
FROM Sales3 s
INNER JOIN Fruits3 f ON s.product_id = f.fruit_id;

-- Join Sales with Vegetables
SELECT s.*, v.*
FROM Sales3 s
INNER JOIN Vegetables1 v ON s.product_id = v.vegetable_id;

-- Join Sales with Groceries
SELECT s.*, g.*
FROM Sales3 s
INNER JOIN Groceries3 g ON s.product_id = g.grocery_id;
-- Join Sales with Fruits and display a string message
SELECT 'Sales of Fruits:', s.sale_id, s.quantity, f.name AS product_name, f.origin
FROM Sales3 s
INNER JOIN Fruits3 f ON s.product_id = f.fruit_id;

-- Join Sales with Vegetables and display a string message
SELECT 'Sales of Vegetables:', s.sale_id, s.quantity, v.name AS product_name, v.origin
FROM Sales3 s
INNER JOIN Vegetables1 v ON s.product_id = v.vegetable_id;

-- Join Sales with Groceries and display a string message
SELECT 'Sales of Groceries:', s.sale_id, s.quantity, g.name AS product_name, g.origin
FROM Sales3 s
INNER JOIN Groceries3 g ON s.product_id = g.grocery_id;
-- Join Sales with Fruits and display a string message along with employee ID and name
SELECT 'Sales of Fruits:', s.sale_id, s.quantity, f.name AS product_name, f.origin, e.employee_id, e.name AS employee_name
FROM Sales3 s
INNER JOIN Fruits3 f ON s.product_id = f.fruit_id
INNER JOIN Employees3 e ON s.employee_id = e.employee_id;

-- Join Sales with Vegetables and display a string message along with employee ID and name
SELECT 'Sales of Vegetables:', s.sale_id, s.quantity, v.name AS product_name, v.origin, e.employee_id, e.name AS employee_name
FROM Sales3 s
INNER JOIN Vegetables1 v ON s.product_id = v.vegetable_id
INNER JOIN Employees3 e ON s.employee_id = e.employee_id;

-- Join Sales with Groceries and display a string message along with employee ID and name
SELECT 'Sales of Groceries:', s.sale_id, s.quantity, g.name AS product_name, g.origin, e.employee_id, e.name AS employee_name
FROM Sales3 s
INNER JOIN Groceries3 g ON s.product_id = g.grocery_id
INNER JOIN Employees3 e ON s.employee_id = e.employee_id;
-- Order employees by name in ascending order
SELECT * FROM Employees3 ORDER BY name ASC;

-- Order employees by salary in descending order
SELECT * FROM Employees3 ORDER BY salary DESC;

-- Order employees by hire date in ascending order
SELECT * FROM Employees3 ORDER BY hire_date ASC;

-- Order vegetables by name in ascending order
SELECT * FROM Vegetables1 ORDER BY name ASC;

-- Order vegetables by stock quantity in descending order
SELECT * FROM Vegetables1 ORDER BY stock_quantity DESC;

-- Order vegetables by price per kg in ascending order
SELECT * FROM Vegetables1 ORDER BY price_per_kg ASC;

-- Order fruits by name in ascending order
SELECT * FROM Fruits3 ORDER BY name ASC;

-- Order fruits by stock quantity in descending order
SELECT * FROM Fruits3 ORDER BY stock_quantity DESC;

-- Order fruits by price per kg in ascending order
SELECT * FROM Fruits3 ORDER BY price_per_kg ASC;

-- Order groceries by name in ascending order
SELECT * FROM Groceries3 ORDER BY name ASC;

-- Order groceries by stock quantity in descending order
SELECT * FROM Groceries3 ORDER BY stock_quantity DESC;

-- Order groceries by price in ascending order
SELECT * FROM Groceries3 ORDER BY price ASC;
-- Let's start a transaction
START TRANSACTION;

-- Suppose we want to increase the salary of employee with ID 1
UPDATE Employees3 SET salary = salary + 5000 WHERE employee_id = 1;

-- Check the updated salary
SELECT * FROM Employees3 WHERE employee_id = 1;

-- If the update is correct, we COMMIT the transaction
COMMIT;

-- If there was an error or we want to undo the changes, we ROLLBACK the transaction
ROLLBACK;

-- Suppose we want to add a new column named 'age' to the Employees3 table
ALTER TABLE Employees3 ADD COLUMN age INT;

-- Check the structure of the table
DESC Employees3;

-- If we made a mistake in altering the table, we can rollback the ALTER statement
ROLLBACK;

-- Suppose now we want to add the 'age' column again
ALTER TABLE Employees3 ADD COLUMN age INT;

-- Suppose we want to update the age of employees based on their birth date
-- For simplicity, let's assume age is calculated in years from birth date to the current date
UPDATE Employees3 SET age = YEAR(CURDATE()) - YEAR(birth_date);

-- Check if the update was successful
SELECT * FROM Employees3;

-- If everything is correct, we COMMIT the transaction
COMMIT;
-- Calculate the total sale cost for each product
SELECT 
    g.name AS product_name,
    MAX(s.quantity * g.price) AS max_sale_cost,
    MIN(s.quantity * g.price) AS min_sale_cost,
    AVG(s.quantity * g.price) AS avg_sale_cost
FROM 
    Sales3 s
JOIN 
    Groceries3 g ON s.product_id = g.grocery_id
GROUP BY 
    g.name;


-- Order sales by sale date in ascending order
SELECT * FROM Sales3 ORDER BY sale_date ASC;

-- Order sales by quantity in descending order
SELECT * FROM Sales3 ORDER BY quantity DESC;

-- Order sales by product_id in ascending order
SELECT * FROM Sales3 ORDER BY product_id ASC;


