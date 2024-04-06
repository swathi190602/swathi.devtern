# swathi.devtern
my intenship task which was given by devtern company


import pandas as pd
import matplotlib.pyplot as plt

# Load data (replace 'data.csv' with your filename)
try:
  data = pd.read_csv('data.csv')
except FileNotFoundError:
  print("Error: File 'data.csv' not found. Please check the file path.")
  exit()

# Handle missing values (if any)
data.dropna(inplace=True)

# Convert datetime column (if needed)
if 'datetime' in data.columns:
  try:
    data['datetime'] = pd.to_datetime(data['datetime'])
  except:
    print("Warning: Error converting 'datetime' column. Check data format.")

# Create weekday and hour features from datetime (if applicable)
if 'datetime' in data.columns:
  data['Weekday'] = data['datetime'].dt.weekdayname
  data['Hour'] = data['datetime'].dt.hour

# Feature engineering (add more based on your data)
data['Distance_Category'] = pd.cut(data['distance'], bins=[0, 2, 5, 10, float('inf')], labels=['<2km', '2-5km', '5-10km', '>10km'])

# Weekday analysis
weekday_counts = data['Weekday'].value_counts()
plt.bar(weekday_counts.index, weekday_counts.values)
plt.xlabel('Weekday')
plt.ylabel('Trip Count')
plt.title('Uber Trips by Weekday')
plt.show()

# Hourly analysis
hour_counts = data['Hour'].value_counts()
plt.plot(hour_counts.index, hour_counts.values)
plt.xlabel('Hour')
plt.ylabel('Trip Count')
plt.title('Uber Trips by Hour')
plt.show()

# Distance analysis by category
distance_category_counts = data['Distance_Category'].value_counts()
plt.bar(distance_category_counts.index, distance_category_counts.values)
plt.xlabel('Distance Category')
plt.ylabel('Trip Count')
plt.title('Trip Distribution by Distance')
plt.show()


print("Data exploration complete. Visualizations generated.")