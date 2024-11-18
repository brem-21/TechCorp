# TechCorp

![alt text](<assignment (3).jpg>)

# DATABASE NORMALIZATION IN 3NF
Based on the table given, we can see that the table is not in 3NF. We can normalize it by splitting it. Already, the case study mentioned the table consists of products, customers, orders and inventory table.





Certainly! Below is the **README** with necessary Markdown tags for a clean and structured format. This includes headings, code blocks, lists, and tables with proper syntax.

```markdown
# Database Normalization README

This document explains how to normalize the columns **OrderID**, **CustomerName**, **ProductID**, **ProductName**, **Category**, **CustomerAddress**, **Supplier**, **OrderDate**, **Quantity**, **Price**, and **Total** through the different normal forms: **1NF**, **2NF**, and **3NF**. The objective of this normalization process is to break down the attributes into tables that minimize redundancy, improve data integrity, and facilitate easier maintenance.

---

## Table of Contents

- [Step 1: Original Columns (Unnormalized Form)](#step-1-original-columns-unnormalized-form)
- [Step 2: 1st Normal Form (1NF)](#step-2-1st-normal-form-1nf)
- [Step 3: 2nd Normal Form (2NF)](#step-3-2nd-normal-form-2nf)
- [Step 4: 3rd Normal Form (3NF)](#step-4-3rd-normal-form-3nf)
- [Conclusion](#conclusion)

---

## Step 1: Original Columns (Unnormalized Form)

In the original unnormalized form, we have the following columns:

- **OrderID**
- **CustomerName**
- **ProductID**
- **ProductName**
- **Category**
- **CustomerAddress**
- **Supplier**
- **OrderDate**
- **Quantity**
- **Price**
- **Total**

### Issues:
1. **Data Redundancy**: Repeating customer information (like `CustomerName` and `CustomerAddress`) for each order.
2. **Partial Dependency**: Certain attributes depend only on parts of the primary key (e.g., `CustomerName` depends on `OrderID`, not `ProductID`).
3. **Transitive Dependency**: Attributes like `Total` are derived from `Quantity` and `Price`, causing redundancy.

---

## Step 2: 1st Normal Form (1NF)

### **1NF Requirements:**
- All attributes must contain **atomic values** (no multiple values or arrays).
- Each record should be **unique**, meaning no repeating rows.

In this case, the table already contains atomic values. To ensure uniqueness, we need a **primary key**. In this case, **OrderID** is a unique identifier for each order, but if we have multiple products per order, we could use a composite key of **(OrderID, ProductID)**.

#### **1NF Table Structure (with primary key)**:
| **Column Name**   | **Description**                                      |
|-------------------|------------------------------------------------------|
| `OrderID`         | Unique identifier for each order.                   |
| `CustomerName`    | Name of the customer who placed the order.          |
| `ProductID`       | Unique identifier for the product.                  |
| `ProductName`     | Name of the product being ordered.                  |
| `Category`        | Category of the product (e.g., Electronics, Furniture). |
| `CustomerAddress` | Address of the customer.                            |
| `Supplier`        | Supplier of the product.                            |
| `OrderDate`       | The date the order was placed.                      |
| `Quantity`        | Number of units ordered.                            |
| `Price`           | Price of one unit of the product.                   |
| `Total`           | Total price (`Quantity * Price`), but can be calculated. |

- **Primary Key**: `OrderID` (or composite key `OrderID, ProductID` if multiple products per order).

---

## Step 3: 2nd Normal Form (2NF)

### **2NF Requirements:**
- The table must be in **1NF**.
- All **non-key attributes** must be **fully functionally dependent** on the **primary key**.
- No **partial dependencies** (where non-key attributes depend on part of the composite primary key).

### **Analysis for 2NF**:
- **CustomerName**, **CustomerAddress**, and **OrderDate** depend only on `OrderID`, not on `ProductID`.
- **ProductName**, **Category**, and **Price** depend only on `ProductID`, not on `OrderID`.

This implies a **partial dependency**, so we need to split the columns into different tables.

#### **Tables in 2NF**:

1. **Orders Table** (Order-related information):
   | **Column Name**   | **Description**                                      |
   |-------------------|------------------------------------------------------|
   | `OrderID`         | Unique identifier for each order.                   |
   | `CustomerName`    | Name of the customer who placed the order.          |
   | `CustomerAddress` | Address of the customer.                            |
   | `Supplier`        | Supplier of the product.                            |
   | `OrderDate`       | The date the order was placed.                      |

   - **Primary Key**: `OrderID`

2. **Products Table** (Product-related information):
   | **Column Name**   | **Description**                                      |
   |-------------------|------------------------------------------------------|
   | `ProductID`       | Unique identifier for the product.                  |
   | `ProductName`     | Name of the product being ordered.                  |
   | `Category`        | Category of the product (e.g., Electronics, Furniture). |
   | `Price`           | Price of one unit of the product.                   |

   - **Primary Key**: `ProductID`

3. **OrderDetails Table** (Order and product relationship, stores `Quantity` and `Total`):
   | **Column Name**   | **Description**                                      |
   |-------------------|------------------------------------------------------|
   | `OrderID`         | Foreign Key, references `OrderID` in **Orders**.     |
   | `ProductID`       | Foreign Key, references `ProductID` in **Products**. |
   | `Quantity`        | Number of units ordered.                            |
   | `Total`           | Calculated total (`Quantity * Price`).               |

   - **Primary Key**: (`OrderID`, `ProductID`)
   - **Foreign Keys**:
     - `OrderID` references `OrderID` in **Orders**.
     - `ProductID` references `ProductID` in **Products**.

---

## Step 4: 3rd Normal Form (3NF)

### **3NF Requirements:**
- The table must be in **2NF**.
- There should be **no transitive dependencies**, meaning non-key attributes must not depend on other non-key attributes.

### **Analysis for 3NF**:
- In the **OrderDetails** table, the **Total** column is derived from `Quantity` and `Price`. This is a **transitive dependency** because `Total` can be calculated based on `Quantity` and `Price`. Storing `Total` is redundant.

### **Normalization to 3NF**:
- We **remove the `Total` column** from the **OrderDetails** table since it can be calculated dynamically.

#### **3NF Tables**:

1. **Orders Table** (unchanged from 2NF):
   | **Column Name**   | **Description**                                      |
   |-------------------|------------------------------------------------------|
   | `OrderID`         | Unique identifier for each order.                   |
   | `CustomerName`    | Name of the customer who placed the order.          |
   | `CustomerAddress` | Address of the customer.                            |
   | `Supplier`        | Supplier of the product.                            |
   | `OrderDate`       | The date the order was placed.                      |

   - **Primary Key**: `OrderID`

2. **Products Table** (unchanged from 2NF):
   | **Column Name**   | **Description**                                      |
   |-------------------|------------------------------------------------------|
   | `ProductID`       | Unique identifier for the product.                  |
   | `ProductName`     | Name of the product being ordered.                  |
   | `Category`        | Category of the product (e.g., Electronics, Furniture). |
   | `Price`           | Price of one unit of the product.                   |

   - **Primary Key**: `ProductID`

3. **OrderDetails Table** (removes `Total` column):
   | **Column Name**   | **Description**                                      |
   |-------------------|------------------------------------------------------|
   | `OrderID`         | Foreign Key, references `OrderID` in **Orders**.     |
   | `ProductID`       | Foreign Key, references `ProductID` in **Products**. |
   | `Quantity`        | Number of units ordered.                            |

   - **Primary Key**: (`OrderID`, `ProductID`)
   - **Foreign Keys**:
     - `OrderID` references `OrderID` in **Orders**.
     - `ProductID` references `ProductID` in **Products**.

---

## Conclusion

By following the steps of normalization, we have:
- **Eliminated redundancy** by breaking down the original columns into separate tables.
- **Eliminated partial dependencies** by ensuring that non-key attributes depend on the full primary key.
- **Eliminated transitive dependencies** by removing the `Total` column, which was a derived value.

### **Primary and Foreign Keys**:
- **Primary Keys**: Ensure that each record in a table is unique.
- **Foreign Keys**: Ensure referential integrity between related tables, linking `OrderDetails` to both `Orders` and `Products`.

This normalized structure ensures better data integrity, reduces redundancy, and facilitates easier maintenance in the long





















# PART 4
Creating a database named ProductCatalog,
![alt text](image.png)

A collection named Product
![alt text](image-1.png)

Mongo queries to insert data into the collection.
To insert a single document to the collection, use the insert_one() method.
    FOR ELECTRONICS;
    db.products.insertOne({
  "ProductID": "E123456",
  "Name": "Smartphone X10",
  "Brand": "TechNova",
  "Price": 599.99,
  "Specifications": {
    "Display": "6.5 inches AMOLED",
    "Processor": "Snapdragon 888",
    "Battery": "4000mAh",
    "Camera": {
      "Rear": "64MP",
      "Front": "20MP"
    },
    "Storage": "128GB",
    "RAM": "8GB"
  },
  "Warranty": "2 years",
  "ReleaseDate": ISODate("2023-05-01T00:00:00Z"),
  "Category": "Electronics"
});

FOR CLOTHING;
    db.products.insertOne({
  "ProductID": "C987654",
  "Name": "Men's T-Shirt",
  "Size": "L",
  "Color": "Blue",
  "Material": "Cotton",
  "Price": 19.99,
  "Brand": "FashionCo",
  "StockQuantity": 150,
  "Category": "Clothing",
  "Gender": "Male"
});
 FOR BOOK;
    db.products.insertOne({
  "ProductID": "B543210",
  "Title": "The Great Adventure",
  "Author": "Jane Doe",
  "Genre": "Adventure Fiction",
  "ISBN": "978-1-23456-789-0",
  "Price": 15.99,
  "Publisher": "Adventure Books Inc.",
  "PublicationDate": ISODate("2021-03-15T00:00:00Z"),
  "Pages": 350,
  "Language": "English",
  "Category": "Books"
});


To insert multiple documents to the collection, use the insert_many() method.db.products.insertMany([
  // Electronics products
  {
    "ProductID": "E123456",
    "Name": "Smartphone X10",
    "Brand": "TechNova",
    "Price": 599.99,
    "Specifications": {
      "Display": "6.5 inches AMOLED",
      "Processor": "Snapdragon 888",
      "Battery": "4000mAh",
      "Camera": {
        "Rear": "64MP",
        "Front": "20MP"
      },
      "Storage": "128GB",
      "RAM": "8GB"
    },
    "Warranty": "2 years",
    "ReleaseDate": ISODate("2023-05-01T00:00:00Z"),
    "Category": "Electronics"
  },
  {
    "ProductID": "E234567",
    "Name": "Laptop Pro 15",
    "Brand": "CompTech",
    "Price": 1299.99,
    "Specifications": {
      "Display": "15.6 inches Retina",
      "Processor": "Intel i7-10750H",
      "Battery": "5500mAh",
      "Camera": {
        "Rear": "No camera",
        "Front": "720p"
      },
      "Storage": "512GB SSD",
      "RAM": "16GB"
    },
    "Warranty": "1 year",
    "ReleaseDate": ISODate("2022-08-10T00:00:00Z"),
    "Category": "Electronics"
  },

  // Clothing products
  {
    "ProductID": "C987654",
    "Name": "Men's T-Shirt",
    "Size": "L",
    "Color": "Blue",
    "Material": "Cotton",
    "Price": 19.99,
    "Brand": "FashionCo",
    "StockQuantity": 150,
    "Category": "Clothing",
    "Gender": "Male"
  },
  {
    "ProductID": "C765432",
    "Name": "Women's Jeans",
    "Size": "M",
    "Color": "Black",
    "Material": "Denim",
    "Price": 49.99,
    "Brand": "StyleWear",
    "StockQuantity": 200,
    "Category": "Clothing",
    "Gender": "Female"
  },

  // Book products
  {
    "ProductID": "B543210",
    "Title": "The Great Adventure",
    "Author": "Jane Doe",
    "Genre": "Adventure Fiction",
    "ISBN": "978-1-23456-789-0",
    "Price": 15.99,
    "Publisher": "Adventure Books Inc.",
    "PublicationDate": ISODate("2021-03-15T00:00:00Z"),
    "Pages": 350,
    "Language": "English",
    "Category": "Books"
  },
  {
    "ProductID": "B123456",
    "Title": "Science 101",
    "Author": "John Smith",
    "Genre": "Educational",
    "ISBN": "978-0-12345-678-9",
    "Price": 24.99,
    "Publisher": "EduPress",
    "PublicationDate": ISODate("2020-07-20T00:00:00Z"),
    "Pages": 200,
    "Language": "English",
    "Category": "Books"
  }
]);

![alt text](image-2.png)



# PART 5
![alt text](datalake.jpg)

For TechCorp, the data lake serves as a central location where all kinds of data (e.g., products, customers, orders, inventory) from various sources can be ingested, stored, processed, and analyzed. The data lake allows the company to efficiently move away from the current monolithic table, which causes redundancy and performance issues, and transition to a more modern, scalable system.

## Data Ingestion (Batch/Scheduled & Real-Time Streaming):

Batch/Scheduled Ingestion: TechCorp can periodically (e.g., daily, weekly) ingest large batches of data from their legacy systems (such as the old database) into the data lake. This could include structured data like sales orders, customer profiles, or inventory updates. The data is typically stored in raw format, which means it hasn't been cleaned, transformed, or processed yet.

Real-Time Streaming: For real-time operations like customer orders, product inventory updates, or website interactions, TechCorp can stream data into the data lake in near real-time. This allows TechCorp to continuously capture and store live data as it flows from different sources.

## Raw Data Store to EDA (Exploratory Data Analysis):

The raw data stored in the data lake can then be accessed for Exploratory Data Analysis (EDA). EDA involves analyzing the raw, unprocessed data to discover trends, patterns, relationships, and potential issues.
TechCorp can use data wrangling and ETL (Extract, Transform, Load) processes to clean and preprocess the raw data from the data lake before performing EDA.
The data might be analyzed using visualization tools (Power BI) or statistical techniques. This stage helps identify any data quality issues, redundancies, and also allows TechCorp to gain insights into the structure and meaning of the data.


## Real-Time Processing to Processed Data:

As part of the real-time data processing, raw data from the data lake can be continuously processed, cleaned, and transformed into a more structured and usable format. This is crucial for TechCorp, especially in scenarios where the company requires up-to-the-minute insights, such as inventory updates or real-time product availability.
Tools like Apache Spark, Flink, or cloud-based solutions (e.g., AWS Glue, Azure Data Factory) can be used to process this data efficiently, transforming it into a more usable form for analysis and downstream applications (like predictive modeling or BI analytics).
The processed data is then stored in a more structured format, such as in a data warehouse or structured storage layer (still within the broader data lake ecosystem), making it easier to work with for more advanced analytics.

## From Processed Data to Predictive Modeling:

Once the data is processed, TechCorp can leverage predictive modeling techniques to analyze trends, forecast demand, or predict customer behavior. This could involve machine learning algorithms (like regression models, classification models, or time series forecasting) to generate insights that are valuable for decision-making.
The processed data in the data lake can be fed into machine learning pipelines, enabling predictions about future product sales, inventory needs, customer churn, and more. Tools like TensorFlow, Scikit-learn, or cloud-based services can help build these predictive models.
The results of these models can be stored back in the data lake or a separate database for further analysis or integration into other business processes.
From Processed Data to BI Analytics:

**Finally**, TechCorp can use the processed data from the data lake to fuel Business Intelligence (BI) Analytics. BI tools (e.g., Power BI, Tableau, Looker) can connect directly to the data lake or the data warehouse layer, enabling decision-makers to access real-time dashboards, reports, and visualizations that help track business KPIs, product performance, sales trends, and more.
BI analytics will provide insights that support decision-making at various levels of the organization, from operational managers to executives, by offering a clear, visual understanding of key metrics and business trends.