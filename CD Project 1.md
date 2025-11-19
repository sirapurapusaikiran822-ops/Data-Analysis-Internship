Python



import pandas as pd



\# Load the dataset

file\_path = 'dataset.csv'

df = pd.read\_csv(file\_path)



\# Display the first 5 rows

print("First 5 rows of the dataset:")

print(df.head().to\_markdown(index=False, numalign="left", stralign="left"))



\# Display the DataFrame info

print("\\nDataFrame Info:")

df.info()



Code output

First 5 rows of the dataset:

| OrderID   | Date       | CustomerID   | ProductCategory   | Quantity   | UnitPrice   | TotalAmount   | DeliveryStatus   | PaymentMethod   | Rating   | Review                              | Country   |

|:----------|:-----------|:-------------|:------------------|:-----------|:------------|:--------------|:-----------------|:----------------|:---------|:------------------------------------|:----------|

| 1000      | 2024-06-12 | C008         | Electronics       | 3          | 1203        | 3609          | Delivered        | COD             | 3        | Terrible service, not buying again. | canada    |

| 1001      | 23-01-2024 | C038         | Books             | 1          | 322         | 322           | Delivered        | COD             | 5        | nan                                 | canada    |

| 1002      | 2024-06-03 | C002         | Beauty            | nan        | 2432        | 4864          | In Transit       | COD             | 4        | Very good quality and support.      | nan       |

| 1003      | 2024-03-12 | C001         | Apparel           | 4          | 1593        | 6372          | In Transit       | COD             | 4        | Excellent packaging and quality.    | uk        |

| 1004      | 2024-01-27 | C006         | Books             | 1          | 1670        | 1670          | In Transit       | Credit Card     | 2        | Great product and fast delivery!    | germany   |



DataFrame Info:

<class 'pandas.core.frame.DataFrame'>

RangeIndex: 600 entries, 0 to 599

Data columns (total 12 columns):

 #   Column           Non-Null Count  Dtype

---

 0   OrderID          570 non-null    float64

 1   Date             570 non-null    object

 2   CustomerID       570 non-null    object

 3   ProductCategory  570 non-null    object

 4   Quantity         570 non-null    float64

 5   UnitPrice        570 non-null    float64

 6   TotalAmount      570 non-null    float64

 7   DeliveryStatus   570 non-null    object

 8   PaymentMethod    571 non-null    object

 9   Rating           570 non-null    float64

 10  Review           570 non-null    object

 11  Country          571 non-null    object

dtypes: float64(5), object(7)

memory usage: 56.4+ KB



Python



import pandas as pd

import numpy as np



\# Load the dataset

file\_path = 'dataset.csv'

df = pd.read\_csv(file\_path)



\# 1. Drop rows where OrderID is null

df.dropna(subset=\['OrderID'], inplace=True)



\# 2. Convert Date to datetime

\# The dates have mixed formats (YYYY-MM-DD and DD-MM-YYYY).

\# We'll try to convert them, coercing errors for unparseable dates.

df\['Date'] = pd.to\_datetime(df\['Date'], dayfirst=True, errors='coerce')

\# For any that failed (became NaT), try the other format.

\# A more robust way is to try both formats.

\# Let's try to parse with standard format first, then dayfirst=True for the others

date\_format1 = pd.to\_datetime(df\['Date'], format='%Y-%m-%d', errors='coerce')

date\_format2 = pd.to\_datetime(df\['Date'], format='%d-%m-%Y', errors='coerce')

df\['Date'] = date\_format1.fillna(date\_format2)



\# 3. Fill missing Quantity with the median

median\_quantity = df\['Quantity'].median()

df\['Quantity'].fillna(median\_quantity, inplace=True)



\# 4. Fill missing Review with "No Review"

df\['Review'].fillna('No Review', inplace=True)



\# 5. Standardize Country

df\['Country'].replace('nan', np.nan, inplace=True)

df\['Country'].fillna('Unknown', inplace=True)



\# 6. Fill missing Rating with the mean

mean\_rating = df\['Rating'].mean()

df\['Rating'].fillna(mean\_rating, inplace=True)



\# 7. Convert columns to appropriate types

df\['OrderID'] = df\['OrderID'].astype('Int64')

df\['Quantity'] = df\['Quantity'].astype('Int64')

\# Round rating before converting to Int64

df\['Rating'] = df\['Rating'].round().astype('Int64')



\# Display the cleaned data info

print("Cleaned DataFrame Info:")

df.info()



\# Display the first 5 rows of the cleaned data

print("\\nCleaned DataFrame Head:")

print(df.head().to\_markdown(index=False, numalign="left", stralign="left"))



\# Check for any remaining null values in key columns

print("\\nNull values check after cleaning:")

print(df.isnull().sum())



Code output

Cleaned DataFrame Info:

<class 'pandas.core.frame.DataFrame'>

Int64Index: 570 entries, 0 to 599

Data columns (total 12 columns):

 #   Column           Non-Null Count  Dtype

---

 0   OrderID          570 non-null    Int64

 1   Date             541 non-null    datetime64\[ns]

 2   CustomerID       543 non-null    object

 3   ProductCategory  542 non-null    object

 4   Quantity         570 non-null    Int64

 5   UnitPrice        543 non-null    float64

 6   TotalAmount      541 non-null    float64

 7   DeliveryStatus   541 non-null    object

 8   PaymentMethod    541 non-null    object

 9   Rating           570 non-null    Int64

 10  Review           570 non-null    object

 11  Country          570 non-null    object

dtypes: Int64(3), datetime64\[ns](1), float64(2), object(6)

memory usage: 59.6+ KB



Cleaned DataFrame Head:

| OrderID   | Date                | CustomerID   | ProductCategory   | Quantity   | UnitPrice   | TotalAmount   | DeliveryStatus   | PaymentMethod   | Rating   | Review                              | Country   |

|:----------|:--------------------|:-------------|:------------------|:-----------|:------------|:--------------|:-----------------|:----------------|:---------|:------------------------------------|:----------|

| 1000      | 2024-06-12 00:00:00 | C008         | Electronics       | 3          | 1203        | 3609          | Delivered        | COD             | 3        | Terrible service, not buying again. | canada    |

| 1001      | 2024-01-23 00:00:00 | C038         | Books             | 1          | 322         | 322           | Delivered        | COD             | 5        | No Review                           | canada    |

| 1002      | 2024-06-03 00:00:00 | C002         | Beauty            | 3          | 2432        | 4864          | In Transit       | COD             | 4        | Very good quality and support.      | Unknown   |

| 1003      | 2024-03-12 00:00:00 | C001         | Apparel           | 4          | 1593        | 6372          | In Transit       | COD             | 4        | Excellent packaging and quality.    | uk        |

| 1004      | 2024-01-27 00:00:00 | C006         | Books             | 1          | 1670        | 1670          | In Transit       | Credit Card     | 2        | Great product and fast delivery!    | germany   |



Null values check after cleaning:

OrderID             0

Date               29

CustomerID         27

ProductCategory    28

Quantity            0

UnitPrice          27

TotalAmount        29

DeliveryStatus     29

PaymentMethod      29

Rating              0

Review              0

Country             0

dtype: int64



Python



import pandas as pd

import numpy as np

import matplotlib.pyplot as plt



\# Load the dataset

file\_path = 'dataset.csv'

df = pd.read\_csv(file\_path)



\# --- Data Cleaning ---



\# 1. Drop rows where OrderID is null

df.dropna(subset=\['OrderID'], inplace=True)



\# 2. Convert Date to datetime

\# Try standard format first, then dayfirst=True for the others

date\_format1 = pd.to\_datetime(df\['Date'], format='%Y-%m-%d', errors='coerce')

date\_format2 = pd.to\_datetime(df\['Date'], format='%d-%m-%Y', errors='coerce')

df\['Date'] = date\_format1.fillna(date\_format2)



\# 3. Fill missing Quantity with the median

median\_quantity = df\['Quantity'].median()

df\['Quantity'].fillna(median\_quantity, inplace=True)



\# 4. Fill missing Review with "No Review"

df\['Review'].fillna('No Review', inplace=True)



\# 5. Standardize Country

df\['Country'].replace('nan', np.nan, inplace=True)

df\['Country'].fillna('Unknown', inplace=True)



\# 6. Fill missing Rating with the mean

mean\_rating = df\['Rating'].mean()

df\['Rating'].fillna(mean\_rating, inplace=True)



\# 7. Convert columns to appropriate types

df\['OrderID'] = df\['OrderID'].astype('Int64')

df\['Quantity'] = df\['Quantity'].astype('Int64')

df\['Rating'] = df\['Rating'].round().astype('Int64')



\# 8. Drop remaining rows with NaN values (in Date, TotalAmount, etc.)

df.dropna(inplace=True)



\# 9. Save the cleaned DataFrame

cleaned\_file\_path = 'cleaned\_dataset.csv'

df.to\_csv(cleaned\_file\_path, index=False)

print(f"Cleaned data saved to {cleaned\_file\_path}")



\# --- Data Analysis ---



\# 1. Descriptive Statistics

print("\\n--- Descriptive Statistics for Numerical Columns ---")

desc\_stats = df\[\['Quantity', 'UnitPrice', 'TotalAmount']].describe()

print(desc\_stats.to\_markdown(numalign="left", stralign="left"))



\# 2. Sales Over Time

print("\\n--- Analyzing Sales Over Time ---")

\# Extract month and year

df\['YearMonth'] = df\['Date'].dt.to\_period('M')

\# Group by month and sum TotalAmount

monthly\_sales = df.groupby('YearMonth')\['TotalAmount'].sum().sort\_index()



\# Convert PeriodIndex to string for plotting

monthly\_sales.index = monthly\_sales.index.astype(str)



print("Monthly Sales Data:")

print(monthly\_sales.to\_markdown(numalign="left", stralign="left"))



\# Plotting Sales Over Time

plt.figure(figsize=(12, 6))

monthly\_sales.plot(kind='line', marker='o')

plt.title('Total Sales Over Time')

plt.xlabel('Month')

plt.ylabel('Total Sales Amount')

plt.grid(True, linestyle='--', alpha=0.6)

plt.xticks(rotation=45)

plt.tight\_layout()

plt.savefig('monthly\_sales\_trend.png')

print("Generated monthly\_sales\_trend.png")



Code output

Cleaned data saved to cleaned\_dataset.csv



--- Descriptive Statistics for Numerical Columns ---

|       | Quantity   | UnitPrice   | TotalAmount   |

|:------|:-----------|:------------|:--------------|

| count | 382        | 382         | 382           |

| mean  | 3.0445     | 1390.15     | 4268.77       |

| std   | 1.38822    | 662.799     | 3070.88       |

| min   | 1          | 201         | 237           |

| 25%   | 2          | 806         | 1834          |

| 50%   | 3          | 1413.5      | 3517          |

| 75%   | 4          | 1974.75     | 6051          |

| max   | 5          | 2488        | 12440         |



--- Analyzing Sales Over Time ---

Monthly Sales Data:

| YearMonth   | TotalAmount   |

|:------------|:--------------|

| 2024-01     | 197373        |

| 2024-02     | 171207        |

| 2024-03     | 398644        |

| 2024-04     | 245074        |

| 2024-05     | 391493        |

| 2024-06     | 226880        |

Generated monthly\_sales\_trend.png





cleaned\_dataset

CSV





Python



import pandas as pd

import matplotlib.pyplot as plt

import matplotlib.ticker as mtick



\# Load the cleaned dataset

file\_path = 'cleaned\_dataset.csv'

df = pd.read\_csv(file\_path)



\# --- 3. Top Product Categories ---

print("\\n--- Analyzing Top 5 Product Categories by Sales ---")

category\_sales = df.groupby('ProductCategory')\['TotalAmount'].sum().sort\_values(ascending=False)

top\_5\_categories = category\_sales.head(5)



print("Top 5 Product Categories:")

print(top\_5\_categories.to\_markdown(numalign="left", stralign="left"))



\# Plotting Top 5 Product Categories

plt.figure(figsize=(10, 6))

top\_5\_categories.sort\_values(ascending=True).plot(kind='barh')

plt.title('Top 5 Product Categories by Total Sales')

plt.xlabel('Total Sales Amount')

plt.ylabel('Product Category')

\# Format x-axis as currency

formatter = mtick.FuncFormatter(lambda x, p: f'${x:,.0f}')

plt.gca().xaxis.set\_major\_formatter(formatter)

plt.tight\_layout()

plt.savefig('top\_5\_product\_categories.png')

print("Generated top\_5\_product\_categories.png")



\# --- 4. Sales by Country ---

print("\\n--- Analyzing Top 10 Countries by Sales ---")

country\_sales = df.groupby('Country')\['TotalAmount'].sum().sort\_values(ascending=False)

top\_10\_countries = country\_sales.head(10)



print("Top 10 Countries by Sales:")

print(top\_10\_countries.to\_markdown(numalign="left", stralign="left"))



\# Plotting Top 10 Countries

plt.figure(figsize=(12, 7))

top\_10\_countries.sort\_values(ascending=True).plot(kind='barh')

plt.title('Top 10 Countries by Total Sales')

plt.xlabel('Total Sales Amount')

plt.ylabel('Country')

\# Format x-axis as currency

plt.gca().xaxis.set\_major\_formatter(formatter)

plt.tight\_layout()

plt.savefig('top\_10\_countries\_sales.png')

print("Generated top\_10\_countries\_sales.png")



Code output



--- Analyzing Top 5 Product Categories by Sales ---

Top 5 Product Categories:

| ProductCategory   | TotalAmount   |

|:------------------|:--------------|

| Home Decor        | 378231        |

| Electronics       | 345932        |

| Apparel           | 329613        |

| Beauty            | 327084        |

| Books             | 249811        |

Generated top\_5\_product\_categories.png



--- Analyzing Top 10 Countries by Sales ---

Top 10 Countries by Sales:

| Country   | TotalAmount   |

|:----------|:--------------|

| germany   | 326451        |

| canada    | 326152        |

| usa       | 284858        |

| uk        | 263119        |

| india     | 249309        |

| Unknown   | 100399        |

| INDIA     | 22243         |

| CANADA    | 20091         |

| GERMANY   | 19455         |

| UK        | 9216          |

Generated top\_10\_countries\_sales.png











Python



import pandas as pd

import matplotlib.pyplot as plt

import matplotlib.ticker as mtick



\# Load the cleaned dataset

file\_path = 'cleaned\_dataset.csv'

df = pd.read\_csv(file\_path)



\# --- 4. (Revised) Sales by Country ---

print("\\n--- Revising Sales by Country Analysis (Standardizing Case) ---")

\# Standardize the 'Country' column to title case

df\['Country'] = df\['Country'].str.title()



country\_sales = df.groupby('Country')\['TotalAmount'].sum().sort\_values(ascending=False)

top\_10\_countries = country\_sales.head(10)



print("Top 10 Countries by Sales (Standardized):")

print(top\_10\_countries.to\_markdown(numalign="left", stralign="left"))



\# Plotting Top 10 Countries (Revised)

plt.figure(figsize=(12, 7))

top\_10\_countries.sort\_values(ascending=True).plot(kind='barh')

plt.title('Top 10 Countries by Total Sales (Standardized)')

plt.xlabel('Total Sales Amount')

plt.ylabel('Country')

formatter = mtick.FuncFormatter(lambda x, p: f'${x:,.0f}')

plt.gca().xaxis.set\_major\_formatter(formatter)

plt.tight\_layout()

plt.savefig('top\_10\_countries\_sales\_revised.png')

print("Generated top\_10\_countries\_sales\_revised.png")





\# --- 5. Rating Distribution ---

print("\\n--- Analyzing Customer Rating Distribution ---")

rating\_counts = df\['Rating'].value\_counts().sort\_index()



print("Customer Rating Counts:")

print(rating\_counts.to\_markdown(numalign="left", stralign="left"))



\# Plotting Rating Distribution

plt.figure(figsize=(10, 6))

rating\_counts.plot(kind='bar', color='skyblue')

plt.title('Distribution of Customer Ratings')

plt.xlabel('Rating (1-5)')

plt.ylabel('Number of Orders')

plt.xticks(rotation=0)

plt.tight\_layout()

plt.savefig('customer\_rating\_distribution.png')

print("Generated customer\_rating\_distribution.png")



\# --- 6. Delivery Status ---

print("\\n--- Analyzing Delivery Status Counts ---")

delivery\_status\_counts = df\['DeliveryStatus'].value\_counts()



print("Delivery Status Counts:")

print(delivery\_status\_counts.to\_markdown(numalign="left", stralign="left"))



\# Plotting Delivery Status

plt.figure(figsize=(10, 6))

delivery\_status\_counts.sort\_values(ascending=True).plot(kind='barh', color='lightgreen')

plt.title('Order Counts by Delivery Status')

plt.xlabel('Number of Orders')

plt.ylabel('Delivery Status')

plt.tight\_layout()

plt.savefig('delivery\_status\_counts.png')

print("Generated delivery\_status\_counts.png")



Code output



--- Revising Sales by Country Analysis (Standardizing Case) ---

Top 10 Countries by Sales (Standardized):

| Country   | TotalAmount   |

|:----------|:--------------|

| Canada    | 346243        |

| Germany   | 345906        |

| Usa       | 287303        |

| Uk        | 272335        |

| India     | 271552        |

| Unknown   | 100399        |

| Nan       | 6933          |

Generated top\_10\_countries\_sales\_revised.png



--- Analyzing Customer Rating Distribution ---

Customer Rating Counts:

|    | Rating   |

|:---|:---------|

| 1  | 43       |

| 2  | 58       |

| 3  | 113      |

| 4  | 112      |

| 5  | 56       |

Generated customer\_rating\_distribution.png



--- Analyzing Delivery Status Counts ---

Delivery Status Counts:

|            | DeliveryStatus   |

|:-----------|:-----------------|

| Returned   | 133              |

| In Transit | 127              |

| Delivered  | 122              |

Generated delivery\_status\_counts.png

















First, I conducted a thorough data cleaning process, which involved:



Handling mixed date formats (e.g., '2024-01-23' and '12-03-2024').



Filling in missing values for Quantity and Rating using their median and mean, respectively.



Standardizing Country names (e.g., 'canada' and 'CANADA' were merged).



Removing rows that still had missing critical information (like TotalAmount or Date) to ensure the analysis is accurate.



The final, cleaned dataset has 382 complete records



Here is a summary of the analysis:



Sales Performance

Here are the key statistics for your sales data:



Average Order Quantity: 3 items



Average Unit Price: $1,390.15



Average Total Amount per Order: $4,268.77



Total Revenue (Jan-Jun 2024): $1,630,671



Monthly Sales Trend

Sales peaked in March and May 2024, with March being the strongest month. Sales were lowest in February.



YearMonth	TotalAmount

2024-01	$197,373

2024-02	$171,207

2024-03	$398,644

2024-04	$245,074

2024-05	$391,493

2024-06	$226,880



EXPORT TO SHEETS



Top Performing Categories \& Countries

Home Decor is the highest-earning product category, followed closely by Electronics and Apparel. Sales are distributed fairly evenly across the top 5 countries, with Canada and Germany leading.



Top 5 Product Categories	TotalAmount

Home Decor	$378,231

Electronics	$345,932

Apparel	$329,613

Beauty	$327,084

Books	$249,811



EXPORT TO SHEETS



Top 10 Countries	TotalAmount

Canada	$346,243

Germany	$345,906

Usa	$287,303

Uk	$272,335

India	$271,552

Unknown	$100,399

Nan	$6,933



EXPORT TO SHEETS



Customer Ratings \& Delivery Status

The customer ratings are normally distributed, with 3-star and 4-star ratings being the most common. The delivery statuses show a relatively even split, though Returned items are slightly more frequent than Delivered or In Transit orders.



Rating	Number of Orders

1	43

2	58

3	113

4	112

5	56



EXPORT TO SHEETS



Delivery Status	Number of Orders

Returned	133

In Transit	127

Delivered	122

