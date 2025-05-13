# Install required libraries
pip install pandas
pip install matplotlib seaborn
pip install numpy

# Import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()

# Load and clean the dataset
births = pd.read_csv("births.csv")
print(births.head())

births['day'].fillna(0, inplace=True)
births['day'] = births['day'].astype(int)

# Create 'decade' column
births['decade'] = 10 * (births['year'] // 10)
print(births.head())

# Pivot table by decade and gender
birth_decade = births.pivot_table('births', index='decade', columns='gender', aggfunc='sum')

# Plot births per decade
birth_decade.plot()
plt.ylabel("Total births per year")
plt.title("Birth Trends by Gender and Decade")
plt.grid(True)
plt.tight_layout()
plt.show()

# Remove outliers using IQR method
quartiles = np.percentile(births['births'], [25, 50, 75])
mean = quartiles[1]
sigma = 0.74 * (quartiles[2] - quartiles[0])

births = births.query('(births > @mean - 5 * @sigma) & (births < @mean + 5 * @sigma)')

# Convert to datetime index
births.index = pd.to_datetime(10000 * births.year + 100 * births.month + births.day, format='%Y%m%d')
births['day of week'] = births.index.dayofweek

# Average births by day of week and gender
births_pivot = births.pivot_table('births', index='day of week', columns='gender', aggfunc='mean')

# Plot weekly birth trends
births_pivot.plot()
plt.title("Average Births by Day of Week")
plt.ylabel("Mean Births")
plt.grid(True)
plt.tight_layout()
plt.show()






   







