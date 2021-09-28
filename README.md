# Sales Data Analysis
In this data analysis we use Python Pandas & Python Matplotlib to analyze and answer business questions about 12 months worth of sales data. The data contains hundreds of thousands of electronics store purchases broken down by month, product type, cost, purchase address, etc.

We start by cleaning our data. Once we have cleaned up our data, we move to the data exploration and visualisation part.

## Data Source: 
**Kaggle**

## Project Outcomes:
By the end of this project we would have :

### *The Imports*
```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import glob
import datetime as dt
```

### *Concatenating all files together*
```
path = 'C:/Users/unend/JupyterNoteBook/Sales Dataset/Sales_Data' # use your path
all_files = glob.glob(path + "/*.csv")

li = []

for filename in all_files:
    df = pd.read_csv(filename, index_col=None, header=0)
    li.append(df)

sales_df = pd.concat(li, axis=0, ignore_index=True)
sales_df.head()
```

### *Basic Information about our dataset*

### Shape of our dataset
```
sales_df.shape
```

### Columns in our dataset
```
sales_df.info()
```

### Some Basic Information about our data
```
sales_df.describe()
```

### *Clean up the data!*

### Finding out the null values in our dataset
```
sales_df.isna().sum()
```

### Dropping all null values from our dataset
```
sales_df.dropna(inplace=True)
```

### Checking our revised dataset
```
sales_df.info()
```

### Find all rows with repeating and invalid data
```
sales_df[sales_df['Order ID']== 'Order ID']
```

### Remove all duplicate rows
```
sales_df.drop(sales_df[sales_df['Order ID']== 'Order ID'].index,inplace=True)
```

### Checking Our Data after cleaning
```
sales_df[sales_df['Order ID']== 'Order ID'] #no more rows satisfying the condition
```

### Finding out the data types of all Columns in our dataset
```
sales_df.dtypes
```

### Update the columns to their appropriate data types
```
sales_df['Order ID'] = pd.to_numeric(sales_df['Order ID'])
sales_df['Quantity Ordered'] = pd.to_numeric(sales_df['Quantity Ordered'])
sales_df['Price Each'] = pd.to_numeric(sales_df['Price Each'])
sales_df['Order Date'] = pd.to_datetime(sales_df['Order Date'])
```
### Looking at the updated columns
```
sales_df.dtypes
```

### Resetting the Index
```
sales_df.head()#index before resetting
sales_df.reset_index()
sales_df.head()
```

### *Augment data with additional columns*

### Add a separate month column
```
sales_df['Month'] = pd.to_datetime(sales_df['Order Date']).dt.month
sales_df.head(2)
```

### Add a separate column for city
```
sales_df['City'] = sales_df['Purchase Address'].str.split(',').str[1] 
sales_df.head()
```

### *Data Exploration*

### What was the best month for sales? How much was earned that month?
```
sales_df.groupby(sales_df['Month']).sum()['Quantity Ordered']

sales_df['Sales'] = sales_df['Quantity Ordered'] * sales_df['Price Each']
sales_df.groupby('Month').sum()['Sales']
```

### Plot Sales per Month data
```
sales_df.groupby('Month').sum()['Sales'].plot(kind='bar',lw=3,ec='white',ylabel='Sales in USD $ (1 unit = 1000000)',hatch='/',figsize=(10,5),legend=True)
```

### What city sold the most product?
```
sales_df.groupby(sales_df['City']).sum()['Quantity Ordered']
```

### Plot the above data statistics for products sold in each City
```
sales_df.groupby(sales_df['City']).sum()['Quantity Ordered'].plot(kind='bar',ylabel='Quantity Sold')
```

### What time should we display advertisements to maximize likelihood of customer's buying product?
```
sales_df['Time']= sales_df['Order Date'].dt.hour
sales_df.groupby(sales_df['Time']).sum()['Quantity Ordered'].plot(figsize=(10,5))
plt.xticks(sales_df['Time'].unique())
plt.grid()
```

### Number of different types of products sold
```
sales_df['Product'].value_counts()
```

### Plot the Product by Quantity Ordered data
```
sales_df.groupby('Product').sum()['Quantity Ordered'].plot(kind='bar')
```

### Plot Separate Grids to show the sales of each product in different cities
```
sns.set(font_scale=1)
g=sns.FacetGrid(sales_df,row='City',aspect=3)
g.map(sns.histplot, 'Product')
plt.xticks(rotation=90)
```

## Project Setup:
To clone this repository you need to have Python compiler installed on your system alongside pandas and seaborn libraries. I would rather suggest that you download jupyter notebook if you've not already.

To access all of the files I recommend you fork this repo and then clone it locally. Instructions on how to do this can be found here: https://help.github.com/en/github/getting-started-with-github/fork-a-repo

The other option is to click the green "clone or download" button and then click "Download ZIP". You then should extract all of the files to the location you want to edit your code.

Installing Jupyter Notebook: https://jupyter.readthedocs.io/en/latest/install.html<br>
Installing Pandas library: https://pandas.pydata.org/pandas-docs/stable/install.html
































