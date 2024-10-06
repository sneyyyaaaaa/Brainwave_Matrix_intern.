# Brainwave_Matrix_intern.

Supermart Grocery Sales data Analysis using Python

import pandas as pd

# Load the dataset
data = pd.read_csv('/content/Supermart Grocery Sales - Retail Analytics Dataset.csv')  # Replace with your CSV file path

# Display the first few rows
print(data.head())

# Check for missing values
print(data.isnull().sum())

# Drop any rows with missing values
data.dropna(inplace=True)

# Remove duplicates
data.drop_duplicates(inplace=True)


# Calculate total sales and total profit
total_sales = data['Sales'].sum()
total_profit = data['Profit'].sum()

print(f'Total Sales: ${total_sales:.2f}')
print(f'Total Profit: ${total_profit:.2f}')



# ABC Analysis based on Total Sales
sales_summary = data.groupby('Sub Category')['Sales'].sum().reset_index()
sales_summary = sales_summary.sort_values(by='Sales', ascending=False)

# Calculate cumulative sales percentage
sales_summary['Cumulative_Sales'] = sales_summary['Sales'].cumsum()
total_sales_value = sales_summary['Sales'].sum()
sales_summary['Cumulative_Percentage'] = (sales_summary['Cumulative_Sales'] / total_sales_value) * 100

# Classify into A, B, C categories
def categorize(row):
    if row['Cumulative_Percentage'] <= 80:
        return 'A'
    elif row['Cumulative_Percentage'] <= 95:
        return 'B'
    else:
        return 'C'

sales_summary['Category'] = sales_summary.apply(categorize, axis=1)

print(sales_summary[['Sub Category', 'Sales', 'Category']])

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
data = pd.read_csv('/content/Supermart Grocery Sales - Retail Analytics Dataset.csv')  # Replace with your CSV file path

# Display the first few rows and data types
print(data.head())
print(data.dtypes)

# Convert "Order Date" to datetime format with error handling
data['Order Date'] = pd.to_datetime(data['Order Date'], errors='coerce')

# Drop rows where "Order Date" could not be converted
data.dropna(subset=['Order Date'], inplace=True)

# Set "Order Date" as index for resampling
data.set_index('Order Date', inplace=True)

# Group by month and year to calculate monthly sales
monthly_sales = data.resample('M').sum()

# Calculate monthly growth
monthly_sales['Monthly_Growth'] = monthly_sales['Sales'].pct_change() * 100

# Print monthly sales data for verification
print(monthly_sales[['Sales', 'Monthly_Growth']])

# Plotting Monthly Sales Growth
plt.figure(figsize=(12, 6))
sns.lineplot(data=monthly_sales, x=monthly_sales.index, y='Monthly_Growth')
plt.title('Monthly Sales Growth')
plt.xlabel('Month')
plt.ylabel('Growth (%)')
plt.xticks(rotation=45)
plt.grid()
plt.show()

# Top Sub Categories by Sales (assuming you have already calculated sales_summary)
sales_summary = data.groupby('Sub Category')['Sales'].sum().reset_index()
top_sub_categories = sales_summary.sort_values(by='Sales', ascending=False).head(10)

# Plotting Top Sub Categories by Sales
plt.figure(figsize=(12, 6))
sns.barplot(x='Sales', y='Sub Category', data=top_sub_categories)
plt.title('Top 10 Sub Categories by Sales')
plt.xlabel('Sales ($)')
plt.ylabel('Sub Category')
plt.show()
