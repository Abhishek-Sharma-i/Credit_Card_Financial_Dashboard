### Credit_Card_Financial_Dashboard
##Power BI Dashboard

#📁 Project Overview
This project builds a weekly credit card financial dashboard using Power BI, leveraging SQL data as the source.
It delivers clear insights into key performance metrics, customer behavior, revenue drivers, and weekly trends, enabling business stakeholders to monitor operations and make strategic decisions effectively.

#🎯 Project Objective
To develop a comprehensive credit card weekly dashboard that provides real-time analytics on customer segments, spending behavior, revenue patterns, risk indicators, and performance health.

##Dashboard----
1. Credit Card Customer Report
<img width="1575" height="904" alt="image" src="https://github.com/user-attachments/assets/b6fba942-95af-4e78-89e9-4f3280e3fd4a" />

2. Credit card Transacation Report
<img width="1602" height="920" alt="image" src="https://github.com/user-attachments/assets/96052c03-7837-428a-b1f9-68d65d015ffc" />


#📂 Data Source & Background
Data was extracted from a SQL database, containing:

✔️ cc_detail (Credit Card Ops & Transactions)

✔️ cust_detail (Customer Demographics & Behaviour Profiles)

The complete SQL script used for database setup and data import is given below ⬇️

#🧾 SQL Queries Used

-- Create a database 
CREATE DATABASE ccdb;

-- Create cc_detail table
CREATE TABLE cc_detail (
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);

-- Create cust_detail table
CREATE TABLE cust_detail (
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
);

-- Copy csv data into SQL

COPY cc_detail
FROM 'D:\Credit Card Data\credit_card.csv'
DELIMITER ','
CSV HEADER;

COPY cust_detail
FROM 'D:\Credit Card Data\customer.csv'
DELIMITER ','
CSV HEADER;

-- Insert additional data into SQL
COPY cc_detail
FROM 'D:\Credit Card Data\cc_add.csv'
DELIMITER ','
CSV HEADER;

COPY cust_detail
FROM 'D:\Credit Card Data\cust_add.csv'
DELIMITER ','
CSV HEADER;

#🔧 Data Processing & Modelling Workflow
✔️ Data Loading & Cleaning

Imported raw transactional and customer level data

Handled missing, duplicate, and invalid values

Standardized date, text and numeric formats

✔️ Modeling in Power BI

Created relationships between credit and customer tables

Established Star Schema model for reporting

Created calculated columns for segmentation

#🧠 DAX Measures & Calculated Fields
🔹 Age Group Segmentation
AgeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)

🔹 Income Group Segmentation
IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Med",
    'public cust_detail'[income] >= 70000, "High",
    "unknown"
)

🔹 Week Number Calculation
week_num2 = WEEKNUM('public cc_detail'[week_start_date])

🔹 Revenue Calculation
Revenue =
    'public cc_detail'[annual_fees] +
    'public cc_detail'[total_trans_amt] +
    'public cc_detail'[interest_earned]

🔹 Current Week Revenue Measure
Current_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])
    )
)

🔹 Previous Week Revenue Measure
Previous_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])-1
    )
)

# Insights 
<img width="1895" height="1022" alt="image" src="https://github.com/user-attachments/assets/6384e4e3-20e4-433a-b688-61f8d602d46d" />

#🏗️ Tech Stack Used
| Layer            | Tools              |
| ---------------- | ------------------ |
| Database         | SQL Server         |
| ETL / Processing | Power Query        |
| Reporting        | Power BI           |
| Logic            | DAX + SQL          |
| Visuals          | Power BI Dashboard |

