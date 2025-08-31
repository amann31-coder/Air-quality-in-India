# Air-quality-in-India
Air quality report using python codes 

# Let's start by importing the libraries we’ll need
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
cityday = pd.read_csv('/kaggle/input/air-quality-data-in-india/city_day.csv')

# Take a quick look at the first few rows
cityday.head()
# Basic info about columns, data types, and null values
cityday.info()

# How many missing values are there in each column?
cityday.isnull().sum()

# Quick statistical summary of the numerical columns
cityday.describe()
cityday_filled = cityday.fillna(cityday.mean(numeric_only=True))
cityday['Date'] = pd.to_datetime(cityday['Date'])
city = "Delhi"   # Change this to any city you like
city_data = cityday[cityday['City'] == city]

plt.figure(figsize=(12,5))
plt.plot(city_data['Date'], city_data['AQI'], color='blue')
plt.title(f"AQI Trend in {city}", fontsize=14)
plt.xlabel("Date")
plt.ylabel("Air Quality Index (AQI)")
plt.grid(True)
plt.show()
cityday_group = cityday.groupby("City")['AQI'].mean().sort_values(ascending=False)

plt.figure(figsize=(10,6))
cityday_group.plot(kind='bar', color='teal')
plt.title("Average AQI by City (2015–2020)", fontsize=14)
plt.ylabel("Average AQI")
plt.show()
plt.figure(figsize=(10,6))
sns.heatmap(cityday_filled[['PM2.5','PM10','NO','NO2','NOx','NH3','CO','SO2','O3','AQI']].corr(), 
            annot=True, cmap="coolwarm")
plt.title("Correlation of Pollutants with AQI")
plt.show()
