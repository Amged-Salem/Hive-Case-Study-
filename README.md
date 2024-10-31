## Hive Commands for Retail and Customer Data Analysis
This project demonstrates a basic Hive setup to create, load, and query tables for retail transaction data and customer information. The dataset includes two tables:

Retail: Contains transaction details
Customers: Contains customer information
# Prerequisites
Apache Hive installed and configured
CSV files for retail and customer data uploaded locally at:
/home/bigdata/Desktop/retail.csv
/home/bigdata/Desktop/customer.csv
# Steps
1. Create Database and Use It
Create a database named transactions and switch to it.

CREATE DATABASE transactions;
USE transactions;

2. Create and Load Data into retail Table
The retail table contains transaction data with fields for transaction ID, timestamp, product ID, quantity, price, and customer ID.


CREATE TABLE retail (
    transactions_id INT,
    `timestamp` STRING,  -- Enclosed in backticks to avoid conflict with reserved keyword
    product_id INT,
    quantity INT,
    price FLOAT,
    customer_id INT
)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
TBLPROPERTIES ("skip.header.line.count"="1");
# Load Data

LOAD DATA LOCAL INPATH '/home/bigdata/Desktop/retail.csv' INTO TABLE retail;


DESCRIBE retail;


SELECT * FROM retail;
Count Total Records:


SELECT COUNT(*) FROM retail;
Filter Records with Price > 5000:

SELECT * FROM retail WHERE price > 5000;
Rename Table
Rename retail table to retails:


ALTER TABLE transactions.retail RENAME TO transactions.retails;
3. Create and Load Data into customers Table
The customers table stores customer details with fields for customer ID, name, email, and location.

Create Table

CREATE TABLE customers (
    customer_id INT,
    customer_name STRING,
    email STRING,
    location STRING
)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
TBLPROPERTIES ("skip.header.line.count"="1");
Load Data

LOAD DATA LOCAL INPATH '/home/bigdata/Desktop/customer.csv' INTO TABLE customers;
# 4. Join Tables
Retrieve information by joining retail and customers tables on the common customer_id field.


SELECT r.transactions_id, r.product_id, r.quantity, r.price, c.customer_name, c.location
FROM retail r
JOIN customers c ON r.customer_id = c.customer_id;
# Notes
Make sure to modify the file paths (/home/bigdata/Desktop/retail.csv and /home/bigdata/Desktop/customer.csv) based on the actual data location.
Replace timestamp with backticks (`) due to Hive's reserved keywords.
