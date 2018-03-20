

```python
# Observation 1: Urban drivers as well as urban riders account for the majority of Pyber's employees and clientelle. 
# Observation 2: Pyber is used least in rural areas and most in urban areas, with suburban use in between. 
# Observation 3: Rural fares are higher, on average, than urban fares. 
```


```python
import pandas as pd
import numpy
import matplotlib.pyplot as plt
import seaborn as sb
%matplotlib inline
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
merge_data.head()
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
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>




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
total_rides.head()
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
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




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
urban = merge4[merge4["type"] == "Urban"]
#urban.head()
suburban = merge4[merge4["type"] == "Suburban"]
#suburban.head() 
rural = merge4[merge4["type"] == "Rural"]
#rural.head()
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
      <th>17</th>
      <td>East Leslie</td>
      <td>33.660909</td>
      <td>11</td>
      <td>Rural</td>
      <td>9</td>
    </tr>
    <tr>
      <th>18</th>
      <td>East Stephen</td>
      <td>39.053000</td>
      <td>10</td>
      <td>Rural</td>
      <td>6</td>
    </tr>
    <tr>
      <th>19</th>
      <td>East Troybury</td>
      <td>33.244286</td>
      <td>7</td>
      <td>Rural</td>
      <td>3</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Erikport</td>
      <td>30.043750</td>
      <td>8</td>
      <td>Rural</td>
      <td>3</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hernandezshire</td>
      <td>32.002222</td>
      <td>9</td>
      <td>Rural</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Attempt at using seaborn's lmplot to create bubble chart
palette = ["Gold", "#87cefa", "#f08080"]
s = merge4["driver_count"]
plot = sb.lmplot(x="ride_id", y="fare", aspect=3.5, data=merge4, hue="type", palette=palette, scatter_kws={"s": 300})
plot.set(ylim=(15, 45))
plot.set(xlim=(0, 40))
```




    <seaborn.axisgrid.FacetGrid at 0x268facf8>




![png](output_12_1.png)



```python
# Attempt at using Seaborn's pairplot to create bubble chart 
palette = ["Gold", "#87cefa", "#f08080"]

sb.pairplot(merge4, hue="type", palette=palette, x_vars="ride_id", y_vars="fare", kind='scatter', size=5, aspect=2)
#pairplot.set(ylim=(15,45))
plt.ylim(15, 45)
plt.xlim(0, 40)
plt.title("Pyber Ride Data")
plt.xlabel("Total Rides")
plt.ylabel("Average Fare($)")
```




    Text(29.4954,0.5,'Average Fare($)')




![png](output_13_1.png)



```python
# Attempt at using Matplotlib to create bubble chart 
#colors = ["Gold", "#87cefa", "#f08080"]
plt.ylim(15,50)
plt.xlim(0,40)
plt.scatter(x=urban['ride_id'], y=urban['fare'], s=urban['driver_count']*2, c="Gold", alpha=0.4, label="Urban")
plt.scatter(x=suburban['ride_id'], y=suburban['fare'], s=suburban['driver_count']*2, c="#87cefa", alpha=0.4, label="Suburban")
plt.scatter(x=rural['ride_id'], y=rural['fare'], s=rural['driver_count']*2, c="#f08080", alpha=0.4, label="Rural")
# Add titles (main and on axis)
plt.xlabel("Total Number of Rides per City")
plt.ylabel("Average Fare per City($)")
plt.title("Pyber Data")
plt.axis(aspect='equal')

plt.legend(scatterpoints=1, frameon=True, labelspacing=2, title='City Type', loc='best',
           fontsize=10)
#plt.figure(figsize=(50,25))
plt.show()
```


![png](output_14_0.png)



    <matplotlib.figure.Figure at 0x266ea860>



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
# Find the rides in each city type
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
# Pie chart 1
# Pyber fares by city type
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


![png](output_17_0.png)



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


![png](output_19_0.png)



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


![png](output_20_0.png)

