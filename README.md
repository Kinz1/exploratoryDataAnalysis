# exploratoryDataAnalysis
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.pyplot import ylabel

#Load the dataset
file_path = r"C:\Users\USER\Documents\SampleSuperstoreNew.csv"
data = pd.read_csv(file_path, encoding='latin1')

# Display the first few rows and last few rows
print("Dataset Preview: ")
print(data.head(3))
print(data.tail(3))

#Step 1: Cleaning
print("\nData Information:")
print(data.info())

# #Handle missing values
print("\nChecking missing values:")
print(data.isnull().sum())
data = data.dropna() #Drop rows with missing values

#Drop duplicates if any
data = data.drop_duplicates()

#Step 2: Exploratory Data Analysis(EDA)
print("\nBasic Statistics")
print(data.describe())

#Step 3: Sales by region
region_sales = data.groupby("Region")["Sales"].sum()
print("\nTotal sales by region:")
print(region_sales)

#Step 4: Profit by category
category_profit = data.groupby("Category")["Profit"].sum()#.sort_values(ascending=False)
print("\nProfit by category:")
print(category_profit)

#Step 5: Visualization
#Sales by region
plt.figure(figsize = (10, 6))
region_sales.plot(kind= "bar", color= "thistle")
plt.title("Sales By Region Graph")
plt.ylabel("Sales")
plt.xlabel("Region")
plt.show()

#Profit by category
plt.figure(figsize= (8, 8))
sns.barplot(x=category_profit.index, y=category_profit.values, palette='magma')
#category_profit.plot(kind= "bar", color= "blue")
plt.title("Profit by Category Graph")
plt.ylabel("Profit")
plt.xlabel("Category")
plt.show()

#Sales per month
data["Order Date"] = pd.to_datetime(data["Order Date"])
data["Month"] = data["Order Date"].dt.to_period("M")
monthly_sales = data.groupby("Month")["Sales"].sum()
print("\n")
print(monthly_sales)

#Graph of monthly sales
plt.figure(figsize= (8, 10))
monthly_sales.plot(kind = "line", color = "skyblue")
plt.title("Sales Per Month")
plt.ylabel("Sales")
plt.xlabel("Months")
plt.grid(True)
plt.show()

# Correlation Heatmap: a table showing the pairwise correlation coefficients between numerical columns in the dataset
plt.figure(figsize=(8, 6))
numeric_data = data.select_dtypes([int, float])
data_matrix = numeric_data.corr()
sns.heatmap(numeric_data.corr(), annot=True, cmap='coolwarm')
plt.title('Correlation Matrix')
plt.show()

#Total sales made altogether
print("\n")
print("The total sales is: ", region_sales.sum())
