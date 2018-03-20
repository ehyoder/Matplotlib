

```python
import pandas as pd
import numpy
import matplotlib.pyplot as plt
import seaborn as sb
%matplotlib inline
#pip install seaborn
```


```python
# Read first csv as a dataframe
city_data = pd.read_csv("city_data.csv")
#city_data.head()
```


```python
# Read second csv as a dataframe
ride_data = pd.read_csv("ride_data.csv")
#ride_data.head()
```


```python
# Merge csvs into new dataframe 
merge_data = (pd.merge(city_data, ride_data, on="city"))
#merge_data.head()
```


```python
# Classify each city as urban, rural, or suburban
urban_rural_suburban = merge_data[["city","type"]]
urban_rural_suburban = urban_rural_suburban.groupby("city").min()
urban_rural_suburban = urban_rural_suburban.reset_index(level=None, drop=False, inplace=False)
#urban_rural_suburban.head()
```


```python
# Calculate average fare per city
average_fare = merge_data.groupby("city").mean()["fare"]
average_fare = average_fare.to_frame()
average_fare = average_fare.reset_index(level=None, drop=False, inplace=False)
#average_fare.head()
```


```python
# Calculate total number of rides per city
total_rides = merge_data.groupby("city").count()["ride_id"]
total_rides = total_rides.to_frame()
total_rides = total_rides.reset_index(level=None, drop=False, inplace=False)
#total_rides.head()
```


```python
# Calculate number of drivers per city
city_drivers = city_data.groupby("city").sum()["driver_count"]
city_drivers = city_drivers.to_frame()
city_drivers = city_drivers.reset_index(level=None, drop=False, inplace=False)
city_drivers.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>67</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Merge dataframes
merge2 = (pd.merge(average_fare, total_rides, on="city"))
merge3 = (pd.merge(merge2, urban_rural_suburban, on="city"))
#merge3.head()
```


```python
merge4 = (pd.merge(merge3, city_drivers, on="city"))
merge4.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>type</th>
      <th>driver_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>23.928710</td>
      <td>31</td>
      <td>Urban</td>
      <td>21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>20.609615</td>
      <td>26</td>
      <td>Urban</td>
      <td>67</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>37.315556</td>
      <td>9</td>
      <td>Suburban</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>23.625000</td>
      <td>22</td>
      <td>Urban</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>21.981579</td>
      <td>19</td>
      <td>Urban</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Attempt at using seaborn's lmplot to create bubble chart
palette = ["Gold", "#87cefa", "#f08080"]
#plt.xlim(0, 40)
#plt.ylim(0, 50)
s = merge4["driver_count"]
plot = sb.lmplot(x="ride_id", y="fare", aspect=4, data=merge4, hue="type", palette=palette, scatter_kws={"s": 300})
plot.set(ylim=(15, 45))
plot.set(xlim=(0, 40))
```




    <seaborn.axisgrid.FacetGrid at 0xc736ba8>




![png](output_10_1.png)



```python
# Attempt at using Seaborn's pairplot to create bubble chart 
palette = ["Gold", "#87cefa", "#f08080"]
sb.pairplot(merge4, hue="type", palette=palette, x_vars="ride_id", y_vars="fare", kind='scatter', size=5, aspect=2)
#pairplot.set(ylim=(15,45))
#sb.plt.ylim(15, 45)
#sb.plt.xlim(0, 40)
```




    <seaborn.axisgrid.PairGrid at 0x1c743c18>




![png](output_11_1.png)



```python
# Attempt at using Matplotlib to create bubble chart 
colors = ["Gold", "#87cefa", "#f08080"]
plt.ylim(10,50)
plt.xlim(0,40)
plt.scatter(x=merge4['ride_id'], y=merge4['fare'], s=merge4['driver_count']*4, c=colors, alpha=0.4)
# Add titles (main and on axis)
plt.xlabel("Total Number of Rides per City")
plt.ylabel("Average Fare per City($)")
plt.title("Pyber Data")
plt.axis(aspect='equal')

plt.legend(scatterpoints=3, frameon=True, labelspacing=2, title='City Type', loc='best',
           ncol=5,
           fontsize=10)
plt.show()
```


![png](output_12_0.png)



```python
# Count the drivers in each city type
drivers = city_data.groupby("type").sum()["driver_count"]
drivers = drivers.to_frame()
drivers = drivers.reset_index(level=None, drop=False, inplace=False)
drivers
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>driver_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>104</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>638</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>2607</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find the 
city_type = merge_data.groupby("type").sum()
city_type = city_type.reset_index(level=None, drop=False, inplace=False)
city_type.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>driver_count</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>727</td>
      <td>4255.09</td>
      <td>658729360193746</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>9730</td>
      <td>20335.69</td>
      <td>3139583688401015</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>64501</td>
      <td>40078.34</td>
      <td>7890194186030600</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pie chart #1
plt.pie(
    # using data from groupby
    city_type['fare'],
    # with the labels being types of city
    labels=city_type['type'],
    # with shadows
    shadow=True,
    # with colors
    colors=["Gold", "#87cefa", "#f08080"],
    # with one slide exploded out
    #explode=(0, 0, 0, 0, 0.15),
    # with the start angle at 135%
    startangle=135,
    # with the percent listed as a fraction
    autopct='%1.1f%%',
    )

# View the plot drop above
plt.axis('equal')

# Add a title to the chart
plt.title("Pyber Fares by City Type")

# View the plot
plt.tight_layout()
plt.show()
```


![png](output_15_0.png)



```python
total_rides_city_type = merge_data.groupby("type").count()["ride_id"]
total_rides_city_type = total_rides_city_type.to_frame()

total_rides_city_type = total_rides_city_type.reset_index(level=None, drop=False, inplace=False)
total_rides_city_type
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>125</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>657</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>1625</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pie chart 2
# % of Total Rides by City Type

plt.pie(
    # using data from groupby
    total_rides_city_type['ride_id'],
    # with the labels being types of city
    labels=total_rides_city_type['type'],
    # with shadows
    shadow=True,
    # with colors
    colors=["Gold", "#87cefa", "#f08080"],
    # with one slide exploded out
    #explode=(0, 0, 0, 0, 0.15),
    # with the start angle at 135%
    startangle=135,
    # with the percent listed as a fraction
    autopct='%1.1f%%',
    )

# View the plot drop above
plt.axis('equal')

# Add a title to the chart
plt.title("Pyber Rides by City Type")

# View the plot
plt.tight_layout()
plt.show()
```


![png](output_17_0.png)



```python
# Pie chart 3 
# % of Total Drivers by City Type

# Pie chart #1
plt.pie(
    # using data from groupby
    drivers['driver_count'],
    # with the labels being types of city
    labels=city_type['type'],
    # with shadows
    shadow=True,
    # with colors
    colors=["Gold", "#87cefa", "#f08080"],
    # with one slide exploded out
    #explode=(0, 0, 0, 0, 0.15),
    # with the start angle at 135%
    startangle=135,
    # with the percent listed as a fraction
    autopct='%1.1f%%',
    )

# View the plot drop above
plt.axis('equal')

# Add a title to the chart
plt.title("Pyber Drivers by City Type")

# View the plot
plt.tight_layout()
plt.show()
```


![png](output_18_0.png)

