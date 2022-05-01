# 12 months sales data analysis

## Import necessary libraries


```python
import pandas as pd
import os
import matplotlib.pyplot as plt
from itertools import combinations
from collections import Counter
```

### Task #1: Check the file in working directory


```python
os.path.exists("Sales_Data")
```




    True



### Task #2: Merging 12 months data into a single csv file


```python
files = [file for file in os.listdir('D:/DataScienceProjects/Sales_Analysis/Sales_Data')]
all_months_data = pd.DataFrame()
for file in files:
    df = pd.read_csv('D:/DataScienceProjects/Sales_Analysis/Sales_Data/'+file)
    all_months_data = pd.concat([all_months_data, df])
    all_months_data.to_csv('Sales_Data.csv', index=False)
Sales_Data = pd.read_csv('Sales_Data.csv')
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Purchase Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>917 1st St, Dallas, TX 75001</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>682 Chestnut St, Boston, MA 02215</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
    </tr>
  </tbody>
</table>
</div>



## Question1: What was the best month for sales? how much was earned that month?

### Task #3: Clean up the data

#### Drop all NaN and 'Or' rows 


```python
Sales_Data = Sales_Data.dropna(how='all')
Sales_Data = Sales_Data[Sales_Data['Order Date'].str[0:2] != 'Or']
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Purchase Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>917 1st St, Dallas, TX 75001</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>682 Chestnut St, Boston, MA 02215</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/30/19 09:27</td>
      <td>333 8th St, Los Angeles, CA 90001</td>
    </tr>
  </tbody>
</table>
</div>



### Task #4: Add month Column 


```python
Sales_Data['Month'] = Sales_Data['Order Date'].str[0:2]
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Purchase Address</th>
      <th>Month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>917 1st St, Dallas, TX 75001</td>
      <td>04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>682 Chestnut St, Boston, MA 02215</td>
      <td>04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>04</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/30/19 09:27</td>
      <td>333 8th St, Los Angeles, CA 90001</td>
      <td>04</td>
    </tr>
  </tbody>
</table>
</div>




```python
Sales_Data['Month'] = Sales_Data['Month'].astype('int32')
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Purchase Address</th>
      <th>Month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>917 1st St, Dallas, TX 75001</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>682 Chestnut St, Boston, MA 02215</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/30/19 09:27</td>
      <td>333 8th St, Los Angeles, CA 90001</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



### Task #5: Convert 'Quantity Ordered' & 'Price Each' columns into integers & float respectively


```python
Sales_Data['Quantity Ordered'] = pd.to_numeric(Sales_Data['Quantity Ordered'])
Sales_Data['Price Each'] = pd.to_numeric(Sales_Data['Price Each'])
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Purchase Address</th>
      <th>Month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>917 1st St, Dallas, TX 75001</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>682 Chestnut St, Boston, MA 02215</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600.00</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/30/19 09:27</td>
      <td>333 8th St, Los Angeles, CA 90001</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



### Task #6: Add on a Sales column


```python
Sales_Data['Sales'] = Sales_Data['Quantity Ordered'] * Sales_Data['Price Each']
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Purchase Address</th>
      <th>Month</th>
      <th>Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>917 1st St, Dallas, TX 75001</td>
      <td>4</td>
      <td>23.90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>682 Chestnut St, Boston, MA 02215</td>
      <td>4</td>
      <td>99.99</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600.00</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>4</td>
      <td>600.00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>669 Spruce St, Los Angeles, CA 90001</td>
      <td>4</td>
      <td>11.99</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/30/19 09:27</td>
      <td>333 8th St, Los Angeles, CA 90001</td>
      <td>4</td>
      <td>11.99</td>
    </tr>
  </tbody>
</table>
</div>



### Task #7: Compute the best month for sales and the amount earned


```python
result_month = Sales_Data.groupby('Month').sum()
result_month
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Sales</th>
    </tr>
    <tr>
      <th>Month</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>10903</td>
      <td>1811768.38</td>
      <td>1822256.73</td>
    </tr>
    <tr>
      <th>2</th>
      <td>13449</td>
      <td>2188884.72</td>
      <td>2202022.42</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17005</td>
      <td>2791207.83</td>
      <td>2807100.38</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20558</td>
      <td>3367671.02</td>
      <td>3390670.24</td>
    </tr>
    <tr>
      <th>5</th>
      <td>18667</td>
      <td>3135125.13</td>
      <td>3152606.75</td>
    </tr>
    <tr>
      <th>6</th>
      <td>15253</td>
      <td>2562025.61</td>
      <td>2577802.26</td>
    </tr>
    <tr>
      <th>7</th>
      <td>16072</td>
      <td>2632539.56</td>
      <td>2647775.76</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13448</td>
      <td>2230345.42</td>
      <td>2244467.88</td>
    </tr>
    <tr>
      <th>9</th>
      <td>13109</td>
      <td>2084992.09</td>
      <td>2097560.13</td>
    </tr>
    <tr>
      <th>10</th>
      <td>22703</td>
      <td>3715554.83</td>
      <td>3736726.88</td>
    </tr>
    <tr>
      <th>11</th>
      <td>19798</td>
      <td>3180600.68</td>
      <td>3199603.20</td>
    </tr>
    <tr>
      <th>12</th>
      <td>28114</td>
      <td>4588415.41</td>
      <td>4613443.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
months = range(1,13)
plt.bar(months,result_month['Sales'])
plt.xticks(months)
plt.ylabel('Sales in USD ($)')
plt.xlabel("month number")
```




    Text(0.5, 0, 'month number')




    
![output_20_1](https://user-images.githubusercontent.com/75635908/166145822-b53bd823-6e5d-4beb-b052-78849e495d91.png)

    


Answer1: The best month of sales is December with 4,613,443.34 USD

## Question 2: What city had the highest number of sales?

### Task #8: Add a City column 


```python
def get_city(address):
    return address.split(',')[1]

def get_state(address):
    return address.split(',')[2].split(' ')[1]

Sales_Data['City'] = Sales_Data['Purchase Address'].apply(lambda x: get_city(x)+ ' '+get_state(x))
Sales_Data.drop(columns='Purchase Address', inplace=True)
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Month</th>
      <th>Sales</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>04/19/19 08:46</td>
      <td>4</td>
      <td>23.90</td>
      <td>Dallas TX</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>04/07/19 22:30</td>
      <td>4</td>
      <td>99.99</td>
      <td>Boston MA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600.00</td>
      <td>04/12/19 14:38</td>
      <td>4</td>
      <td>600.00</td>
      <td>Los Angeles CA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/12/19 14:38</td>
      <td>4</td>
      <td>11.99</td>
      <td>Los Angeles CA</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>04/30/19 09:27</td>
      <td>4</td>
      <td>11.99</td>
      <td>Los Angeles CA</td>
    </tr>
  </tbody>
</table>
</div>



### Task #9: Compute the city with the highest sales


```python
result_city = Sales_Data.groupby('City').sum()
cities = [city for city, df in Sales_Data.groupby('City')]
plt.bar(cities,result_city['Sales'])
plt.xticks(cities, rotation='vertical', size=12)
plt.ylabel('Sales in USD ($)')
plt.xlabel("City name")
```




    Text(0.5, 0, 'City name')




    
![output_26_1](https://user-images.githubusercontent.com/75635908/166145792-a5acf39a-60e2-4b61-8c22-461c03f9f54b.png)

    


Answer2: The city with the highest number of sales is San Francisco with 8,262,203.91 USD of sales.

## Question 3: What time should we display the advertisement to increase the likelihood that the customer will buy the product

### Task #10: Convert 'Order Date' column into date time format


```python
Sales_Data['Order Date'] = pd.to_datetime(Sales_Data['Order Date'])
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Month</th>
      <th>Sales</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>2019-04-19 08:46:00</td>
      <td>4</td>
      <td>23.90</td>
      <td>Dallas TX</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>2019-04-07 22:30:00</td>
      <td>4</td>
      <td>99.99</td>
      <td>Boston MA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600.00</td>
      <td>2019-04-12 14:38:00</td>
      <td>4</td>
      <td>600.00</td>
      <td>Los Angeles CA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>2019-04-12 14:38:00</td>
      <td>4</td>
      <td>11.99</td>
      <td>Los Angeles CA</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>2019-04-30 09:27:00</td>
      <td>4</td>
      <td>11.99</td>
      <td>Los Angeles CA</td>
    </tr>
  </tbody>
</table>
</div>



### Task #11: Add on 'Hour' and 'minute' columns


```python
Sales_Data['Hour'] = Sales_Data['Order Date'].dt.hour
Sales_Data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Order ID</th>
      <th>Product</th>
      <th>Quantity Ordered</th>
      <th>Price Each</th>
      <th>Order Date</th>
      <th>Month</th>
      <th>Sales</th>
      <th>City</th>
      <th>Hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>176558</td>
      <td>USB-C Charging Cable</td>
      <td>2</td>
      <td>11.95</td>
      <td>2019-04-19 08:46:00</td>
      <td>4</td>
      <td>23.90</td>
      <td>Dallas TX</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>176559</td>
      <td>Bose SoundSport Headphones</td>
      <td>1</td>
      <td>99.99</td>
      <td>2019-04-07 22:30:00</td>
      <td>4</td>
      <td>99.99</td>
      <td>Boston MA</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176560</td>
      <td>Google Phone</td>
      <td>1</td>
      <td>600.00</td>
      <td>2019-04-12 14:38:00</td>
      <td>4</td>
      <td>600.00</td>
      <td>Los Angeles CA</td>
      <td>14</td>
    </tr>
    <tr>
      <th>4</th>
      <td>176560</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>2019-04-12 14:38:00</td>
      <td>4</td>
      <td>11.99</td>
      <td>Los Angeles CA</td>
      <td>14</td>
    </tr>
    <tr>
      <th>5</th>
      <td>176561</td>
      <td>Wired Headphones</td>
      <td>1</td>
      <td>11.99</td>
      <td>2019-04-30 09:27:00</td>
      <td>4</td>
      <td>11.99</td>
      <td>Los Angeles CA</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>



### Task #12: Compute the best time to display advertisement


```python
hours = [hour for hour, df in Sales_Data.groupby('Hour')]
result_hour = Sales_Data.groupby(['Hour']).count()['Order ID']
plt.plot(hours, result_hour)
plt.xticks(hours)
plt.ylabel('Number of Orders')
plt.xlabel("Hours")
plt.grid()
plt.show()
```


    
![output_34_0](https://user-images.githubusercontent.com/75635908/166145765-d9335284-145e-48ba-9e84-28afb6b8a6ae.png)

    


Answer3: The best hours to display advertisement is around 11am (11hour) and at 7pm (19hour).

## Question 4: What products are most often best sold together?

### Task #13: Compute duplicate for Order ID to see which products were sold together and adding 'Grouped Products' column


```python
df = Sales_Data[Sales_Data['Order ID'].duplicated(keep=False)]
df['Grouped Products'] = df.groupby('Order ID')['Product'].transform(lambda x: ','.join(x))
df = df[['Order ID', 'Grouped Products']].drop_duplicates()

count = Counter()
for row in df['Grouped Products']:
    row_list = row.split(',')
    count.update(Counter(combinations(row_list, 2)))
    
for key, value in count.most_common(10):
    print(key, value)
```

    ('iPhone', 'Lightning Charging Cable') 1005
    ('Google Phone', 'USB-C Charging Cable') 987
    ('iPhone', 'Wired Headphones') 447
    ('Google Phone', 'Wired Headphones') 414
    ('Vareebadd Phone', 'USB-C Charging Cable') 361
    ('iPhone', 'Apple Airpods Headphones') 360
    ('Google Phone', 'Bose SoundSport Headphones') 220
    ('USB-C Charging Cable', 'Wired Headphones') 160
    ('Vareebadd Phone', 'Wired Headphones') 143
    ('Lightning Charging Cable', 'Wired Headphones') 92
    

    C:\Users\crispin\AppData\Local\Temp\ipykernel_9036\89776269.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['Grouped Products'] = df.groupby('Order ID')['Product'].transform(lambda x: ','.join(x))
    

## Question 5: What product sold the most? and why do you think it sold the most?

### Task #14: compute the the product sold the most 


```python
product_group = Sales_Data.groupby('Product')
quantity_ordered = product_group.sum()['Quantity Ordered']
products = [product for product, df in product_group]

plt.bar(products, quantity_ordered)
plt.ylabel('Quantity Ordered')
plt.xlabel('Products')
plt.xticks(products, rotation='vertical', size=12)
plt.show()
```


    
![output_41_0](https://user-images.githubusercontent.com/75635908/166145731-c90a9690-bb71-4a64-97ed-6ed370cc2034.png)



Answer 5a: the most sold product is AAA Batteries (4-pack)

### Task #15: Analyse the reason why it is the most sold product 


```python
product_group = Sales_Data.groupby('Product')
quantity_ordered = product_group.sum()['Quantity Ordered']
products = [product for product, df in product_group]
prices = Sales_Data.groupby('Product').mean()['Price Each']

fig, ax1 = plt.subplots()

ax2 = ax1.twinx()
ax1.bar(products, quantity_ordered, color='g')
ax2.plot(products, prices, 'b-')

ax1.set_xlabel('Products')
ax1.set_ylabel('Quantity Ordered', color='g')
ax2.set_ylabel('Prices ($)', color='b')
ax1.set_xticklabels(products, rotation='vertical', size= 12)

plt.show()
```

    C:\Users\crispin\AppData\Local\Temp\ipykernel_9036\1185802277.py:15: UserWarning: FixedFormatter should only be used together with FixedLocator
      ax1.set_xticklabels(products, rotation='vertical', size= 12)
    


    
![output_44_1](https://user-images.githubusercontent.com/75635908/166145677-70ce9708-abe2-490c-b29f-e25e8ff89bbf.png)

    


Answer 5b: The reason why the AAA Batteries are the most sold is beacause of the low price
