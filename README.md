# **E-Commerce-Customer-Churn-Analysis-using-SQL**
This project focuses on analyzing e-commerce customer data using SQL to understand customer churn behavior. It examines customer demographics, purchasing patterns, and key factors influencing customer retention. The analysis helps identify churn trends and generate insights to improve customer retention.


## 📌 **Project Overview**

This project focuses on analyzing customer churn data for an e-commerce platform using **MySQL**. By analyzing customer behavior, transactional histories, logistics data, and support interactions, the project uncovers critical friction points influencing customer retention. The analysis follows a structured data framework—spanning descriptive, diagnostic, predictive, and prescriptive phases—to provide actionable, data-driven recommendations aimed at reducing churn and improving overall Customer Lifetime Value (LTV).

## ❓ 2. Problem Statement

•  Analyze customer data to identify patterns and factors contributing to customer churn in the e-commerce industry.
•  Identify the number of active and churned customers to understand customer retention levels.
•  Examine customer behavior based on tenure, preferred payment methods, order categories, and purchase frequency.
•  Evaluate the relationship between customer complaints, satisfaction scores, and churn status.
•  Determine the impact of warehouse-to-home distance on customer churn.
•  Compare customer preferences based on city tier, gender, marital status, and coupon usage.
•  Analyze customer return records and identify customers who have churned and raised complaints.
•  Generate actionable insights to support better customer retention strategies and business decisions.


## 🎯 **Project Objective**

The primary objective of this project is to analyze, clean, and evaluate e-commerce customer data to identify the root operational and behavioral drivers behind customer churn. By building a structured analytical data pipeline in MySQL, the project aims to transform raw, inconsistent transactional data into business intelligence.
Ultimately, the goal is to provide prescriptive, data-backed strategies—such as optimizing delivery logistics, restructuring promotional cashback structures, and engineering a prioritized customer support workflow—to minimize customer attrition, protect profit margins, and maximize long-term retention.

---

## 🛠️ Tools & Technologies

● MySQL:
- DDL & DML Commands
- MySQL Constraints
- Joins
- Aggregate Functions
- Subqueries
- CASE Expression
- NULL Handling
- SELECT & WHERE
- GROUP BY & HAVING
- ORDER BY & LIMIT


## 💾 Data Architecture & Schema
The project utilizes a relational database named `ecomm` consisting of two primary tables: `customer_churn` and `customer_returns`.

### 1. `customer_churn` Table
Contains the core demographic, behavioral, and operational metrics for each customer account.

*   `CustomerID` (INT, Primary Key): Unique identifier for each customer.
*   `Tenure` (INT): Total months the customer has been with the platform.
*   `PreferredLoginDevice` (VARCHAR): The primary device used to log in.
*   `CityTier` (INT): The tier classification of the customer's city (1, 2, or 3).
*   `WarehouseToHome` (INT): Distance in kilometers from the warehouse to the customer's home.
*   `PreferredPaymentMode` (VARCHAR): Primary payment method selected by the user.
*   `Gender` (ENUM): Customer gender ('Male', 'Female').
*   `HoursSpentOnApp` (INT): Total hours spent on the application.
*   `NumberOfDeviceRegistered` (INT): Total number of devices linked to the account.
*   `PreferredOrderCat` (VARCHAR): Main product category ordered by the customer.
*   `SatisfactionScore` (INT): Customer satisfaction rating (1 to 5).
*   `MaritalStatus` (VARCHAR): Marital status ('Single', 'Married', 'Divorced').
*   `NumberOfAddress` (INT): Total addresses saved by the user.
*   `OrderAmountHikeFromlastYear` (INT): Percentage increase in order value compared to last year.
*   `CouponUsed` (INT): Number of promotional coupons redeemed.
*   `OrderCount` (INT): Total number of orders placed.
*   `DaySinceLastOrder` (INT): Days elapsed since the customer's last transaction.
*   `CashbackAmount` (INT): Total cashback amount credited to the user.
*   `ComplaintReceived` (VARCHAR): Standardized indicator ('Yes', 'No') showing if a complaint was filed.
*   `ChurnStatus` (VARCHAR): The target metric mapping if an account is 'Active' or 'Churned'.

### 2. `customer_returns` Table
Tracks post-purchase friction metrics by logging returned products and financial refunds.

*   `ReturnID` (INT, Primary Key): Unique identifier for the return transaction.
*   `CustomerID` (INT, Foreign Key): Links directly back to the `customer_churn` table.
*   `ReturnDate` (DATE): The exact calendar date the return was processed.
*   `RefundAmount` (INT): Total currency refunded to the customer.

---

## 🛠️ Data Cleaning & Transformation Pipeline
To ensure data integrity, structural consistency, and statistical accuracy, a rigorous cleaning process was executed using dynamic SQL mutations:

*   **Missing Value Imputation (Continuous Variables):** Null values in continuous fields (`WarehouseToHome`, `HoursSpentOnApp`, `OrderAmountHikeFromlastYear`, `DaySinceLastOrder`) were dynamically imputed using their respective rounded column averages.
*   **Missing Value Imputation (Categorical/Discrete Variables):** Mode imputation was leveraged to clean missing values in `Tenure` (Mode: 1 month), `CouponUsed` (Mode: 1 coupon), and `OrderCount` (Mode: 2 orders) by using frequency distribution subqueries.
*   **Outlier Treatment:** Extreme operational outliers in logistics (`WarehouseToHome` values > 100 km) were completely removed from the dataset to protect statistical models from distortion.
*   **Value Standardization & Uniformity:** Categorical inconsistencies were resolved across key behavioral features:
    *   Unified `"Phone"` values into `"Mobile Phone"` within the login device category.
    *   Unified `"Mobile"` into `"Mobile Phone"` within preferred order categories.
    *   Standardized abbreviations into clean readable values (`"COD"` → `"Cash on Delivery"`; `"CC"` → `"Credit Card"`).
*   **Schema Transformation:** 
    *   Renamed misaligned operational columns (`PreferedOrderCat` → `PreferredOrderCat`; `HourSpendOnApp` → `HoursSpentOnApp`).
    *   Created explicit business status strings (`ComplaintReceived` derived from `Complain`; `ChurnStatus` derived from `Churn`).
    *   Dropped redundant binary columns (`Churn`, `Complain`) to minimize structural overhead.

---

## 📈 Executive Summary Analytics

### Descriptive Analytics (What Happened)
The finalized dataset monitors an active customer base of **4,680 customers against 948 churned users**. Accounts that officially churned present an incredibly short life cycle, maintaining an **average platform tenure of only 3 months**. From a corporate cost perspective, this user drop-off resulted in an immediate financial loss of **₹1,52,030 in sunk cashback rewards** paid out to users who subsequently exited. Additionally, high-risk pockets have emerged in specific product segments; for instance, inside City Tier-3, a subset of **150 churned users** explicitly listed "Laptop & Accessory" as their primary shopping category.

### Diagnostic Analytics (Why It Happened)
Customer service friction and fulfillment delays stand out as the primary catalysts for user abandonment. Diagnostic subqueries confirm that **53.59% of all churned customers had filed a complaint** before leaving. The average satisfaction score among these complaining accounts stands at a low **3 out of 5**, signaling an ineffective resolution workflow. Furthermore, supply chain proximity directly dictates user loyalty: customers residing far from distribution centers (**"Far Distance" category, >15 km**) suffered from delayed shipping, generating **498 churned customers**, whereas accounts in the immediate vicinity (**"Very Close Distance", ≤ 5 km**) registered only **1 single churned user**.

### Predictive Analytics (What Is Likely to Happen)
If operational and service processes remain unchanged, the platform can expect the following trajectories:
*   **Complaint-Driven Attrition:** Any active account that logs an unmitigated support complaint possesses a **>50% statistical probability of churning** within the following 30–60 days.
*   **Logistics Churn Accumulation:** Accounts situated beyond the 15 km fulfillment perimeter will continue to cycle out of the platform, driving up customer acquisition costs (CAC).
*   **Margin Erosion:** Issuing upfront, unearned cashback incentives to accounts in their first 90 days will continue to burn capital, averaging a **₹160 sunk cost per churned user**.

### Prescriptive Analytics (How to Act)
To optimize customer retention, protect marketing margins, and secure long-term loyalty, the following strategic changes should be deployed:
*   **Deploy a Priority Support Flag:** Build an automated trigger that funnels support tickets from active users with a satisfaction score ≤ 3 into a **Priority Resolution Queue** to resolve friction before they churn.
*   **Transition to a Tiered Loyalty Program:** Eliminate immediate, high-value cashback rewards for new accounts. Restructure the reward framework into a milestone-based system where maximum cashback scales with account age, effectively preserving margin during high-risk deployment windows.
*   **Implement Distant Fulfillment Optimization:** Address the fulfillment gap for users in the "Far Distance" segment (>15 km) by integrating regional 3PL partners or local hyper-local dark stores to slash transit times down to parity with close-range zones.

---

## 🚀 How to Run the Project
1. Open your SQL client (e.g., MySQL Workbench, phpMyAdmin, or Command Line Interface).
2. Execute the schema script `DROP DATABASE IF EXISTS ecomm; CREATE DATABASE ecomm; USE ecomm;` along with the table definitions.
3. Import the provided record dump data rows into the database.
4. Execute the cleaning scripts sequentially to clean the tables and set up the analytics columns.
5. Run the diagnostic queries to generate real-time metrics reporting on operational churn indicators.

## ✅ **Conclusion**

This project helped analyze e-commerce customer behavior and understand customer churn patterns using MySQL. Data cleaning, transformation, and SQL queries were used to organize customer information and perform meaningful analysis.
The analysis covered customer retention, purchase behavior, payment preferences, complaints, satisfaction scores, cashback, and product returns.
The findings can help e-commerce businesses identify potential customer retention issues, understand customer preferences, improve customer satisfaction, and develop effective strategies to reduce customer churn.

---

## 👩‍💻 Author

**Shanthini**

**Aspiring Data Analyst**

**Skills:** MySQL | SQL | Database Design | Data Management

## 📄 License

This project is created for educational and assignment purposes only.

## 🙏 Acknowledgments

I would like to thank:

- **Entri Elevate Course Support** for providing guidance and learning resources throughout this project.
- **MySQL Documentation** for valuable references and support in understanding SQL commands, database design, and implementation.

   

