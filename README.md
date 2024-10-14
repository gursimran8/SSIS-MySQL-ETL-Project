# SSIS-MySQL-ETL-Project

## **Overview**
This project demonstrates the end-to-end ETL (Extract, Transform, Load) process using SQL Server Integration Services (SSIS) to load e-commerce data from flat files into a MySQL database for further analysis. 

## **Technologies Used**
- **ETL Tool**: SQL Server Integration Services (SSIS)
- **Database**: MySQL
- **Programming Language**: SQL
- **File Format**: Flat file (.txt)

## **ETL Process**

1. **Extraction**:  
   The e-commerce dataset is extracted from a flat file containing transaction details such as `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `CustomerID`, and `Country`.
![Customer Data from Flat File to MySQL](https://github.com/gursimran8/SSIS-MySQL-ETL-Project/blob/main/Images/Customer%20Data%20from%20Flat%20file%20to%20MySQL.png)
   
2. **Transformation**:  
   The following transformations were applied:
   - **Null treatment**: Conditional Split transformation to handle null values and split data based on specific conditions.
   - **Data type conversion**: Data Conversion transformation to ensure the compatibility of data types between flat file and MySQL.
   - **Derived Column**: The `Total_Amount` column was derived using a formula that multiplies `Quantity` and `UnitPrice`.
   
3. **Loading**:  
   The transformed data is loaded into the MySQL database table (`ecommerce_data`) using an ODBC connection.

## **Challenges and Resolutions**

### **Challenge 1: Flat File Format Issues**
**Problem**: The flat file format caused errors due to incompatible data types, especially when handling dates and large numbers.
**Resolution**: Implemented Data Conversion transformations to properly handle data types like `float`, `int`, and `datetime`. Specifically, the `InvoiceDate` field required special handling to prevent overflow errors in MySQL.

### **Challenge 2: Null Values in Data**
**Problem**: The dataset contained several null values in key columns like `CustomerID` and `Country`.
**Resolution**: Used Conditional Split transformation to filter out or handle rows with null values, ensuring data quality and consistency in the MySQL table.

### **Challenge 3: Multicast Transformation Confusion**
**Problem**: Understanding the purpose and impact of the Multicast transformation in the ETL flow.
**Resolution**: The Multicast transformation was used to create logical copies of the data stream, allowing the same dataset to be sent to multiple destinations for parallel processing.

### **Challenge 4: ODBC Destination Errors**
**Problem**: Errors during the ODBC Destination phase, especially related to `Date Overflow` when inserting into MySQL.
**Resolution**: Data types were thoroughly examined and adjusted to ensure MySQL compatibility, specifically ensuring that the `datetime` format was correct for the `InvoiceDate` field.

## **Project Files**
- **SSIS Package**: Includes all the ETL steps (available in this repository).
- **Flat File Dataset**: Contains sample e-commerce data used for this ETL process.
- **MySQL Table**: Schema for the `ecommerce_data` table that was created.

## **How to Run the Project**

1. **Prerequisites**:
   - SSIS installed on your machine.
   - MySQL database set up and ready to receive the transformed data.
   
2. **Execution Steps**:
   - Load the SSIS package in SQL Server Data Tools (SSDT).
   - Configure the flat file connection manager to point to your local e-commerce data file.
   - Configure the ODBC connection manager to connect to your MySQL database.
   - Execute the SSIS package.

3. **Expected Output**:  
   After running the package, the data from the flat file will be loaded into the `ecommerce_data` table in MySQL, with transformations applied.

## **Future Enhancements**
- Improve error handling in SSIS.
- Add more transformations based on business logic requirements.
- Schedule the ETL package using SQL Server Agent for automated data processing.
