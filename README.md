# SSIS-MySQL-ETL-Project

## Project Overview

This project demonstrates how to load e-commerce data from a flat file into a MySQL database using SQL Server Integration Services (SSIS). The objective is to apply transformation logic, handle data challenges, and load processed data into a MySQL table. 

---

### Key Features

- Load data from a flat file.
- Apply conditional split transformations to categorize data (e.g., high-value and low-value orders).
- Use the Multicast component for parallel processing.
- Handle data type conversions.
- Load the transformed data into a MySQL table for further analysis.

---

## ETL Process Steps

1. **Customer Data from Flat File to MySQL**:
   - Load raw customer data from a flat file into MySQL for analysis and processing.
   
   ![Customer Data from Flat File to MySQL](https://github.com/gursimran8/SSIS-MySQL-ETL-Project/blob/main/Images/Customer%20Data%20from%20Flat%20file%20to%20MySQL.png)

2. **Extract of High Value and Low Value Orders**:
   - Use conditional splits to filter the dataset based on order values into high-value and low-value orders.
   
   ![Extract of High Value Orders](https://github.com/gursimran8/SSIS-MySQL-ETL-Project/blob/main/Images/Extract%20of%20high%20value%20orders.png)

3. **MySQL E-commerce Table After Extraction**:
   - Verify the data loaded into MySQL using SQL queries.
   
   ![MySQL E-commerce Table After Extraction](https://github.com/gursimran8/SSIS-MySQL-ETL-Project/blob/main/Images/MySQL%20E_commerce%20table%20after%20extraction.png)

4. **MySQL Query to Create, Alter, and View Table**:
   - SQL queries were written to create, alter, and view the MySQL table. 

   ```sql
   CREATE DATABASE e_commerce;
   USE e_commerce;
   CREATE TABLE customer_data (
       InvoiceNo INT,
       StockCode VARCHAR(6),
       Description VARCHAR(35),
       Quantity TINYINT,
       InvoiceDate DATETIME,
       UnitPrice FLOAT,
       CustomerID FLOAT,
       Country VARCHAR(14)
   );
   ALTER TABLE customer_data ADD COLUMN Total_Amount FLOAT;
   SELECT * FROM customer_data;


![MySQL query to create alter and view table](https://github.com/gursimran8/SSIS-MySQL-ETL-Project/blob/main/MySQL%20query%20to%20create%20alter%20and%20view%20table%2C.png)



## Challenges and Resolutions

### 1. Challenge: Data Type Mismatch
**Issue**:  
There was a mismatch between the data types in the flat file and the MySQL table schema (e.g., integers vs floats). This caused errors during data loading.

**Resolution**:  
We implemented a **Data Conversion** transformation in SSIS to convert the data types from the flat file to match the MySQL table schema. For example, we converted the `Quantity` and `Total_Amount` fields from their original formats to `float` to ensure smooth data insertion.

---

### 2. Challenge: Date Overflow Errors
**Issue**:  
The SSIS package failed to load certain rows into MySQL due to date overflow errors in the `InvoiceDate` field. The flat file contained date values that did not match the required format in MySQL.

**Resolution**:  
We resolved this by adjusting the date formats in the flat file and using SSIS to handle date conversions. The `InvoiceDate` field was configured with the correct date format in SSIS and MySQL, ensuring all date values were processed without errors.

---

### 3. Challenge: Handling Null Values
**Issue**:  
The source flat file contained null or missing values in critical fields such as `CustomerID` and `Quantity`, which caused the SSIS package to fail during execution.

**Resolution**:  
We applied a **Conditional Split** transformation in SSIS to filter out rows with null values. This allowed valid rows to continue processing while null values were either ignored or directed to error handling components.

---

### 4. Challenge: Multicasting Data
**Issue**:  
While using the **Multicast** transformation to split data for parallel processing, we encountered issues with data flow configuration. The multicast operation needed to create logical copies of the dataset, but errors occurred due to incorrect setup.

**Resolution**:  
The **Multicast** component was properly configured to create parallel streams of the same dataset, which allowed us to process different parts of the data independently (e.g., high-value and low-value orders) without data loss or transformation issues.

---

### 5. Challenge: SQL Errors (Overflow, etc.)
**Issue**:  
SQL overflow errors occurred when inserting data into MySQL, primarily due to incorrect data formats in columns like `Quantity` and `UnitPrice`.

**Resolution**:  
We addressed these errors by ensuring proper data type conversion in SSIS. For example, converting `Quantity` to a `TINYINT` and `UnitPrice` to a `FLOAT` helped resolve the overflow errors, and data was successfully inserted into MySQL.


