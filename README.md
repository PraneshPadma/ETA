# Delivery Five Cities Dataset Processing Project

## Project Overview

This project focuses on **loading, merging, cleaning, and preprocessing delivery datasets** collected from five different cities. The main objective is to create a unified and clean dataset that can later be used for:

* Data Analysis
* Machine Learning
* Delivery Time Prediction
* Logistics Optimization
* Geographic Analysis
* Courier Performance Analysis

The project uses **Python** along with the **Pandas** library for data manipulation and preprocessing.

---

# Dataset Information

The project works with delivery datasets from five cities:

| File Name         | Description                  |
| ----------------- | ---------------------------- |
| `delivery_cq.csv` | Delivery dataset for city CQ |
| `delivery_jl.csv` | Delivery dataset for city JL |
| `delivery_hz.csv` | Delivery dataset for city HZ |
| `delivery_sh.csv` | Delivery dataset for city SH |
| `delivery_yt.csv` | Delivery dataset for city YT |

---

# Technologies Used

* Python 3.x
* Pandas
* OS Module
* Collections Module

---

# Project Workflow

The project follows a complete ETL-style preprocessing pipeline:

## 1. Import Required Libraries

```python
import pandas as pd
import os
from collections import Counter
```

Libraries are imported for:

* Data manipulation (`pandas`)
* File handling (`os`)
* Counting records (`Counter`)

---

# 2. Define Dataset Path

```python
data_path = r"C:\Users\prane\Downloads\DFCD\Delivery_Five_Cities_Datasets"
```

The dataset directory path is defined to access all CSV files.

---

# 3. Read Multiple CSV Files

```python
dfs = {
    f"df_city{i+1}": pd.read_csv(os.path.join(data_path, file))
    for i, file in enumerate(filenames)
}
```

All datasets are loaded dynamically into separate DataFrames using dictionary comprehension.

---

# 4. Merge All Datasets

```python
merged_df = pd.concat(
    [df_city1, df_city2, df_city3, df_city4, df_city5],
    ignore_index=True
)
```

The datasets are merged into one unified DataFrame.

---

# 5. Explore Dataset

The project prints:

* First 5 rows
* Dataset structure
* Column data types
* Record counts

Example:

```python
print(merged_df.head())
print(merged_df.info())
```

---

# 6. Count Records by City

```python
city_counts = Counter(merged_df['city'])
```

This step helps analyze the number of delivery records available for each city.

---

# 7. Remove Duplicate Rows

```python
merged_df = merged_df.drop_duplicates()
```

Duplicate rows are removed to improve data quality.

---

# 8. Save Merged Dataset

```python
merged_df.to_csv("merged_city_data.csv", index=False)
```

The cleaned merged dataset is saved locally.

---

# 9. Handle Missing Values

The project identifies missing values in important columns:

```python
missing_values = merged_df.loc[:, [
    'accept_time',
    'delivery_time',
    'lng',
    'lat',
    'courier_id'
]].isnull().sum()
```

---

# 10. Remove Invalid Time Records

Rows with missing timestamps are removed:

```python
merged_df = merged_df.dropna(
    subset=['accept_time', 'delivery_time']
)
```

This ensures accurate time-based analysis.

---

# 11. Fill Missing Latitude & Longitude

```python
mean_lat_lng = merged_df.groupby('city')[['lat', 'lng']].transform('mean')

merged_df['lat'] = merged_df['lat'].fillna(mean_lat_lng['lat'])
merged_df['lng'] = merged_df['lng'].fillna(mean_lat_lng['lng'])
```

Missing geographic coordinates are replaced using city-wise averages.

---

# 12. Detect Duplicate Order IDs

```python
duplicates = merged_df[
    merged_df['order_id'].isin(
        merged_df['order_id'][
            merged_df['order_id'].duplicated()
        ]
    )
]
```

This helps identify repeated orders in the dataset.

---

# 13. Data Type Conversion

Several columns are converted into appropriate formats.

## Datetime Conversion

```python
accept_time = pd.to_datetime(...)
delivery_time = pd.to_datetime(...)
```

## Numeric Conversion

```python
lat = pd.to_numeric(...)
lng = pd.to_numeric(...)
```

## Categorical Conversion

```python
aoi_type = merged_df['aoi_type'].astype('category')
```

---

# Final Dataset Features

After preprocessing, the final dataset contains:

* Cleaned records
* Standardized datetime columns
* Numeric geographic coordinates
* Removed duplicates
* Handled missing values
* Optimized categorical columns

---

# Expected Output

The project generates:

## Cleaned Merged Dataset

```bash
merged_city_data.csv
```

---

# Example Columns in Dataset

Possible columns include:

| Column Name     | Description                   |
| --------------- | ----------------------------- |
| `order_id`      | Unique order identifier       |
| `city`          | City name/code                |
| `accept_time`   | Order acceptance timestamp    |
| `delivery_time` | Delivery completion timestamp |
| `lat`           | Latitude                      |
| `lng`           | Longitude                     |
| `courier_id`    | Courier identifier            |
| `aoi_type`      | Area of interest category     |

---

# How to Run the Project

## Step 1: Install Required Libraries

```bash
pip install pandas
```

---

## Step 2: Place Dataset Files

Store all CSV files inside:

```bash
Delivery_Five_Cities_Datasets/
```

---

## Step 3: Run the Script

```bash
python project.py
```

---

# Sample Output

```bash
Loaded DataFrames:
dict_keys(['df_city1', 'df_city2', 'df_city3', 'df_city4', 'df_city5'])

Final Dataset Shape:
(XXXXX, XX)

Merged dataset saved successfully.
```

---

# Future Improvements

Possible future enhancements:

* Delivery time prediction using Machine Learning
* Route optimization
* Interactive dashboards
* Geospatial visualization
* Courier efficiency analysis
* Real-time delivery tracking

---

# Machine Learning Possibilities

This cleaned dataset can be used for:

* Regression Models
* Clustering
* Time Series Analysis
* ETA Prediction
* Demand Forecasting

---

# Project Structure

```bash
├── Delivery_Five_Cities_Datasets/
│   ├── delivery_cq.csv
│   ├── delivery_jl.csv
│   ├── delivery_hz.csv
│   ├── delivery_sh.csv
│   └── delivery_yt.csv
│
├── merged_city_data.csv
├── project.py
└── README.md
```

---

# Author

Praneeth

---

# License

This project is intended for educational and research purposes.

---
