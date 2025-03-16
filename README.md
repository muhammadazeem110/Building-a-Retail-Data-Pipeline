![walmartecomm.png](attachment:image.png)

# Walmart Sales Analysis Data Pipeline

Walmart is the biggest retail store in the United States. Like others, they have been expanding their e-commerce business. By the end of 2022, e-commerce represented a roaring $80 billion in sales, which accounts for 13% of Walmart's total sales. One of the main factors that affects their sales is public holidays, such as the Super Bowl, Labor Day, Thanksgiving, and Christmas.

## Project Overview

In this project, you have been tasked with creating a data pipeline for analyzing supply and demand around holidays, along with conducting a preliminary analysis of the data. You will be working with two data sources: grocery sales and complementary data.

## Data Sources

### 1. `grocery_sales` Table (PostgreSQL Database)

This table contains the following features:

- `index` - Unique ID of the row
- `Store_ID` - Store number
- `Date` - The week of sales
- `Weekly_Sales` - Sales for the given store

### 2. `extra_data.parquet` File

This file contains complementary data:

- `IsHoliday` - Whether the week contains a public holiday (1 if yes, 0 if no)
- `Temperature` - Temperature on the day of sale
- `Fuel_Price` - Cost of fuel in the region
- `CPI` – Prevailing consumer price index
- `Unemployment` - The prevailing unemployment rate
- `MarkDown1`, `MarkDown2`, `MarkDown3`, `MarkDown4` - Number of promotional markdowns
- `Dept` - Department number in each store
- `Size` - Size of the store
- `Type` - Type of the store (depends on Size column)

## Data Pipeline Steps

Build a data pipeline using custom functions to extract, transform, aggregate, and load e-commerce data. The SQL query for `grocery_sales` and the `extract()` function have already been implemented for you.

To start the project, run the first two cells, then proceed with the following steps:

1. **Implement a function named `transform()`**

   - Takes `merged_df` as input.
   - Fills missing numerical values (using any method of your choice).
   - Adds a column "Month".
   - Keeps only the rows where the weekly sales are over $10,000.
   - Drops unnecessary columns.
   - Returns a DataFrame stored as the `clean_data` variable.

2. **Implement the function `avg_weekly_sales_per_month()`**

   - Takes `clean_data` as input.
   - Selects the "Month" and "Weekly_Sales" columns.
   - Uses a chain operation with `groupby()`, `agg()`, `reset_index()`, and `round()`.
   - Groups by the "Month" column and calculates the average monthly sales.
   - Calls `reset_index()` to start a new index order.
   - Rounds the results to two decimal places.

3. **Create a function called `load()`**

   - Takes the cleaned and aggregated DataFrames and their paths.
   - Saves them as `clean_data.csv` and `agg_data.csv`, respectively, without an index.

4. **Define a function `validation()`**
   - Checks whether the two CSV files from `load()` exist in the current working directory.

### Data Processing

You will need to merge these datasets and perform necessary data manipulations. The transformed DataFrame should be stored as the `clean_data` variable containing the following columns:

- `Store_ID`
- `Month`
- `Dept`
- `IsHoliday`
- `Weekly_Sales`
- `CPI`
- `Unemployment`

### Data Analysis

After merging and cleaning the data, you will analyze Walmart's monthly sales and store the results in the `agg_data` variable, which should look like:

| Month | Weekly_Sales |
| ----- | ------------ |
| 1.0   | 33174.178494 |
| 2.0   | 34333.326579 |
| ...   | ...          |

### Output

Finally, you should save `clean_data` and `agg_data` as CSV files.

### Tools

It is recommended to use `pandas` for this project.

```sql
-- Write your SQL query here
SELECT * FROM grocery_sales;
```

```python
import pandas as pd
import os

# Extract function is already implemented for you
def extract(store_data, extra_data):
    extra_df = pd.read_parquet(extra_data)
    merged_df = store_data.merge(extra_df, on="index")
    return merged_df

# Call the extract() function and store it as the "merged_df" variable
merged_df = extract(grocery_sales, "extra_data.parquet")
```

```python
# Create the transform() function with one parameter: "raw_data"
def transform(raw_data):
    # Write your code here
    pass

# Call the transform() function and pass the merged DataFrame
clean_data = transform(merged_df)

# Create the avg_weekly_sales_per_month function that takes in the cleaned data from the last step
def avg_weekly_sales_per_month(clean_data):
    # Write your code here
    pass

# Call the avg_weekly_sales_per_month() function and pass the cleaned DataFrame

# Create the load() function that takes in the cleaned DataFrame and the aggregated one with the paths where they are going to be stored
def load(full_data, full_data_file_path, agg_data, agg_data_file_path):
    # Write your code here
    pass

# Call the load() function and pass the cleaned and aggregated DataFrames with their paths

# Create the validation() function with one parameter: file_path - to check whether the previous function was correctly executed
def validation(file_path):
    # Write your code here
    pass

# Call the validation() function and pass first, the cleaned DataFrame path, and then the aggregated DataFrame path
```
