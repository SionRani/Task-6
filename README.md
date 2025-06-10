# Task-6 Sales Trend Analysis Using Aggregations
# Title : 
Sales Trend Analysis Using Aggregations

# Objective
To analyze monthly revenue and order volume from sales data to understand time trends.

# Dataset
Online Sales Data.csv: This CSV file contains transactional sales data.

# Key Columns Used:
Date (renamed to order_date in the database)
Total Revenue (renamed to amount in the database)
Transaction ID (renamed to order_id in the database)
# Tools Used
Python: For data loading and preprocessing (using pandas and sqlite3 libraries).
SQLite: For database management and executing SQL queries.
How to Run This Analysis
Follow these two main steps to reproduce the analysis:

# Step 1: Load Data into SQLite Database
This Python script reads the Online Sales Data.csv file and loads its content into an SQLite database table named online_sales. It also renames the relevant columns to order_date, amount, and order_id for consistency with the SQL query.

Save the script: Save the following code as a Python file (e.g., load_sales_data.py) in the same directory as your Online Sales Data.csv file.

# Python

import pandas as pd
import sqlite3

csv_file_path = 'Online Sales Data.csv'
db_file_path = 'online_sales_data.db'
table_name = 'online_sales'

try:
    df = pd.read_csv(csv_file_path)
    df['Date'] = pd.to_datetime(df['Date']) # Convert 'Date' column to datetime

    conn = sqlite3.connect(db_file_path)

    df.rename(columns={
        'Date': 'order_date',
        'Total Revenue': 'amount',
        'Transaction ID': 'order_id'
    }, inplace=True)

    df.to_sql(table_name, conn, if_exists='replace', index=False)

    print(f"Successfully loaded '{csv_file_path}' into SQLite database '{db_file_path}' as table '{table_name}'.")
    print("Columns 'Date', 'Total Revenue', 'Transaction ID' were renamed to 'order_date', 'amount', 'order_id' respectively in the database.")

except FileNotFoundError:
    print(f"Error: The file '{csv_file_path}' was not found. Ensure it's in the correct directory or provide the full path.")
except Exception as e:
    print(f"An error occurred: {e}")
finally:
    if 'conn' in locals() and conn:
        conn.close()
Run the script: Open a terminal or command prompt, navigate to the directory where you saved the files, and execute:

Bash

python load_sales_data.py
This will create an SQLite database file named online_sales_data.db.

# Step 2: Execute the SQL Query
Once the data is loaded into online_sales_data.db, you can run the following SQL query using any SQLite client connected to this database.

# SQL

SELECT
    STRFTIME('%Y-%m', order_date) AS sales_year_month,
    SUM(amount) AS total_revenue,
    COUNT(DISTINCT order_id) AS total_volume
FROM
    online_sales
GROUP BY
    sales_year_month
ORDER BY
    sales_year_month;
