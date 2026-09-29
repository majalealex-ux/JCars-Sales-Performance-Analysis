# JCars Sales Performance Analysis

## 1. Project Background

The JCars Sales Performance Analysis project was developed to transform a raw vehicle sales dataset into a structured analytical solution using Microsoft Power BI.

The dataset contained **276 transaction records and 46 columns**, covering information about customers, vehicles, locations, sales representatives, payments, deliveries, costs, discounts, and revenue.

The objective was not simply to create a dashboard, but to follow an end-to-end data analytics process:

**Investigate → Clean → Validate → Model → Analyse → Visualize → Communicate**

---

## 2. Dataset Investigation

The initial investigation was performed using Microsoft Excel before transforming the data in Power Query.

The investigation focused on understanding:

* The grain of the dataset
* Column structure and data types
* Missing values
* Duplicate identifiers
* Category inconsistencies
* Date anomalies
* Relationships between attributes
* Potential business-rule issues

### Dataset Grain

The analysis established that:

> **One row represents one transaction/order record.**

This was important because it determined how transaction counts, units sold, revenue, costs, and other measures should be calculated.

---

## 3. Identifying the Order ID Issue

One of the important findings during the investigation was that `Order ID` was not unique.

Several Order IDs appeared more than once, including:

* `ord1020`
* `CAR1086`
* `ord1174`

The duplicate records contained different values in other columns.

Because the records represented different transaction-level information, the duplicates were **not automatically removed**.

Instead, a separate `Transaction Key` was created to provide a unique identifier for each fact-table row.

This allowed:

* Every transaction record to remain in the analysis
* `Order ID` to remain as a source/business reference
* The fact table to have a reliable row-level key

This was an important modelling decision because removing the duplicate Order IDs would have resulted in loss of source records.

---

## 4. Data Cleaning & Transformation

After the investigation, Power Query was used to clean and transform the dataset.

The cleaning process included:

* Standardizing inconsistent categorical values
* Correcting data types
* Converting valid date values
* Handling invalid and missing dates
* Standardizing customer and vehicle attributes
* Creating validation fields
* Creating `Transaction Key`
* Creating `Delivery Days`
* Separating descriptive attributes from measurable transaction fields

The final transformed dataset retained all **276 transaction records**.

Power Query validation resulted in:

> **0 technical errors**

Data-quality issues that could not be reliably resolved from the source data were retained and documented rather than changed based on assumptions.

---

## 5. Date Validation

Date fields required particular attention because the source dataset contained:

* Missing dates
* Invalid date values
* Excel serial dates
* Text representations
* Other inconsistent date entries

Order and delivery dates were cleaned into appropriate date fields where reliable values were available.

A `Delivery Days` field was created by calculating the difference between delivery date and order date.

Four records produced negative delivery durations:

* `LCL-1080`
* `CAR1219`
* `ord1229`
* `LCL1236`

These records were not silently removed. They were retained as data-quality anomalies because the source data did not provide enough evidence to determine the correct dates.

---

## 6. Data Modelling

The cleaned dataset was transformed into a **star schema** in Power BI.

The model contains one fact table:

### Fact Sales

The fact table represents the transaction grain and contains the measurable transaction information.

Key fields include:

* Transaction Key
* Order ID
* Customer Key
* Vehicle Key
* Location Key
* Sales Key
* Payment Key
* Delivery Key
* Order Date Key
* Delivery Date Key
* Units Sold
* Unit Selling Price
* Unit Cost
* Discount
* Delivery Fee
* Logistics Cost
* Revenue Recorded
* Delivery Days

### Dimension Tables

The supporting dimensions are:

* `Dim Customer`
* `Dim Vehicle`
* `Dim Location`
* `Dim Sales`
* `Dim Payment`
* `Dim Delivery`
* `Dim Date`

The dimensions provide descriptive context while the fact table stores transaction-level measures.

---

## 7. Relationship Design

The model uses **one-to-many relationships** from the dimension tables to the fact table with **single-direction filtering**.

The Date dimension has two relationships with the fact table:

* `Order Date` — active
* `Delivery Date` — inactive

The active Order Date relationship is used for the primary date analysis.

The inactive Delivery Date relationship provides the ability to perform delivery-date analysis when required using DAX techniques such as `USERELATIONSHIP`.

---

## 8. DAX & Analytical Measures

DAX measures were created to answer the business questions dynamically.

Examples include:

* Total Revenue
* Total Units Sold
* Total Transactions
* Total Cost
* Total Profit
* Profit Margin
* Average Delivery Days
* Average Unit Selling Price
* Average Discount
* Total Delivery Fees
* Total Logistics Cost
* Revenue Per Unit
* Average Units per Transaction
* Profit per Transaction
* Delivery Fee % of Revenue

The measures were designed to respond to filters and selections made within the Power BI report.

---

## 9. Dashboard Structure

The final report contains three analytical pages.

### Executive Overview

Provides a high-level view of sales performance, including:

* Total Revenue
* Total Profit
* Profit Margin
* Units Sold
* Transactions
* Average Delivery Days
* Revenue trends
* Revenue by vehicle make
* Revenue by region
* Revenue by customer type

### Sales & Profitability

Focuses on:

* Profit by vehicle make
* Profit margin by vehicle make
* Units sold by vehicle make
* Revenue by lead source
* Transactions by lead source
* Revenue per unit by lead source

### Delivery & Operations

Focuses on:

* Transactions by delivery status
* Average delivery days
* Delivery fees
* Returned versus non-returned transactions

---

## 10. Validation

The model and report were tested after development.

Validation included:

* Checking that all 276 transaction records were retained
* Confirming that the Transaction Key was unique
* Testing relationships between dimensions and the fact table
* Testing date filtering
* Testing slicer interactions
* Checking that visuals responded correctly to selections
* Checking that KPI values changed appropriately under filters
* Reviewing delivery and payment inconsistencies
* Confirming that Power Query contained no technical errors

The final report successfully responded to interactive filters across the report pages.

---

## 11. Key Analytical Considerations

Several business and modelling considerations remain important when interpreting the results.

### No Stable Customer ID

The source dataset does not contain a stable Customer ID.

Customer profiles were therefore created using the available customer attributes, but these should not automatically be interpreted as unique real-world customers.

### No Vehicle ID

The source data does not contain a separate Vehicle ID.

The vehicle dimension therefore represents a vehicle profile/configuration based on the selected combination of vehicle attributes.

### Sales Representative Names

The dataset contains apparent naming variants such as full names and shortened names.

For example, records exist for both names such as `Faith Achieng` and `Faith`.

The available data does not provide enough evidence to determine whether these represent the same individual, so they were not automatically merged.

### Profitability Definition

The `Total Cost` measure currently represents vehicle acquisition cost:

**Units Sold × Unit Cost**

Delivery fees and logistics costs are analysed separately.

Therefore, `Total Profit` should be interpreted as:

**Recorded Revenue − Vehicle Acquisition Cost**

rather than a complete accounting profit measure.

---

## 12. Outcome

The project transformed a raw and inconsistent dataset into a structured Power BI analytical solution.

The final workflow demonstrates practical experience in:

* Data investigation
* Data cleaning
* Data validation
* Power Query
* Star-schema modelling
* Relationship design
* DAX
* Interactive dashboard development
* Business analysis
* Data-quality assessment
* Documentation

The project emphasizes not only the final dashboard, but also the reasoning and validation steps used to make the underlying analysis more reliable.
