

```python
# Observable Trends

# Trend One - The first most obvious trend is that male consumers donimate this data set and make up the majority of players at 82%. 
# Trend Two - The second trend is that most game consumers fall between the agaes of 15 and 24, the largest group at 41.1% is 20 to 24 year olds and the second largest group is 15 to 19 year olds making up 23.65% of consumers. 
# Trend Three - My last observation is that only two games appear in both lists for the top five most popular and the top five most profitable games, Final Critic and Stormcaller. 
```


```python
# import dependecies
import csv
import os
import pandas as pd
import numpy as np
```


```python
# create file paths
items_df = pd.read_csv("HeroesOfPymoli/generated_data/items_complete.csv")
players_df = pd.read_csv("HeroesOfPymoli/generated_data/players_complete.csv")
purchase_df = pd.read_json("HeroesOfPymoli/generated_data/purchase_data.json")
```


```python
# Total Player Count

# find total players using count
total_players = players_df['SN'].count()

# create new df for total players
d = {'Total Players': [total_players]}
total_players_df = pd.DataFrame(data=d)
total_players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1163</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Total)

# Number of Unique Items
unique_items = purchase_df['Item Name'].nunique()

# Average Purchase Price
mean_purchase = purchase_df['Price'].mean()

# Total Number of Purchases
total_purchases = purchase_df['SN'].count()

# Total Revenue
total_revenue = purchase_df['Price'].sum()

# Create dataframe for purchasing analysis

d = {'Number of Unique Items': [unique_items], 'Average Price':[mean_purchase], 
     'Number of Purchases': [total_purchases], 'Total Revenue':[total_revenue]}
purchasing_analysis_df = pd.DataFrame(data=d)

# map format for currency values
purchasing_analysis_df['Average Price'] = purchasing_analysis_df['Average Price'].map('${:,.2f}'.format)
purchasing_analysis_df['Total Revenue'] = purchasing_analysis_df['Total Revenue'].map('${:,.2f}'.format)

purchasing_analysis_df
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics (of Total Players)

# get gender totals
gender_demo_df = players_df['Gender'].value_counts()

# input totals to df
gender_demo_df = pd.DataFrame(data=gender_demo_df)

# change column title
gender_demo_df.rename(columns={'Gender': 'Total Count'}, inplace=True)

# calculate percentage for each gender
percent_gender = gender_demo_df['Total Count']/total_players * 100

# input to df and format
gender_demo_df["Percentage of Players"] = percent_gender
gender_demo_df['Percentage of Players'] = gender_demo_df['Percentage of Players'].map('{:,.2f}%'.format)

gender_demo_df
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>954</td>
      <td>82.03%</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>187</td>
      <td>16.08%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>22</td>
      <td>1.89%</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)

# number of purchases
purchase_count = purchase_df.groupby('Gender')['Item ID'].count()

# average price
ave_price = purchase_df.groupby('Gender')['Price'].mean()

# total revenue
total_value = purchase_df.groupby('Gender')['Price'].sum()

# normalize totals
norm_totals = total_value / gender_demo_df['Total Count']



# input to df
purchase_gender = pd.DataFrame({"Purchase Count": purchase_count,
                                "Average Purchase Price": ave_price,
                                "Total Purchase Value": total_value,
                               "Normalized Totals": norm_totals
                               })

# format numbers
purchase_gender['Average Purchase Price'] = purchase_gender['Average Purchase Price'].map('${:,.2f}'.format)
purchase_gender['Total Purchase Value'] = purchase_gender['Total Purchase Value'].map('${:,.2f}'.format)
purchase_gender['Normalized Totals'] = purchase_gender['Normalized Totals'].map('${:,.2f}'.format)

purchase_gender

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$2.05</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$1.96</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$1.62</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics

# create bins for age groups
bins = [0, 10, 15, 20, 25, 30, 35, 40, 45]
group_labels = [ '<10', '10-14', '15-19', '20-24', '25-29', '30-34','35-39','>40']

# add age group column to table
pd.cut(players_df["Age"], bins, labels=group_labels)

players_df["Age Group"] = pd.cut(players_df["Age"], bins, labels=group_labels)


# create table for age groups 
# group by age
age_group = players_df.groupby(["Age Group"])

# total per group
total_count = age_group['SN'].count()

# percent
percent_age = age_group['SN'].count()/total_players * 100

# input to table 
age_demo = pd.DataFrame({"Percentage of Players": percent_age,
                                "Total Count": total_count})
# format mapping
age_demo['Percentage of Players'] = age_demo['Percentage of Players'].map('{:,.2f}%'.format)

age_demo
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5.33%</td>
      <td>62</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>7.48%</td>
      <td>87</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>23.65%</td>
      <td>275</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>41.10%</td>
      <td>478</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.97%</td>
      <td>116</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.57%</td>
      <td>88</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.13%</td>
      <td>48</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>0.77%</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Five Spenders

# gather data for each column 

# total number of purchases
top_purchase_count = purchase_df.groupby('SN')['Item ID'].count()

# total of purchases per SN
top_total = purchase_df.groupby('SN')['Price'].sum()

# average purchase per SN
top_ave_price = top_total/top_purchase_count
```


```python
# input to DF
top_five = pd.DataFrame({"Purchase Count": top_purchase_count, "Average Purchase Price": top_ave_price, "Total Purchase Value": top_total})

# sort and limit to top five
top_five_spenders = top_five.sort_values("Total Purchase Value", ascending=False)
top_five_spenders = top_five_spenders.head()

# map formatting
top_five_spenders['Average Purchase Price'] = top_five_spenders['Average Purchase Price'].map('${:,.2f}'.format)
top_five_spenders['Total Purchase Value'] = top_five_spenders['Total Purchase Value'].map('${:,.2f}'.format)

top_five_spenders
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items

# gather data for each column 

# total number of purchases
most_pop_count = purchase_df.groupby('Item Name')['Item ID'].count()

# total of purchases per game
most_pop_rev = purchase_df.groupby('Item Name')['Price'].sum()

# price per game
most_pop_price = most_pop_rev/most_pop_count

# input to DF
most_pop = pd.DataFrame({"Purchase Count": most_pop_count, "Item Price": most_pop_price, "Total Purchase Value": most_pop_rev})

# sort by purchase count and limit to top five
most_pop = most_pop.sort_values("Purchase Count", ascending=False)
most_popular_five = most_pop.head()

# map formatting
most_popular_five['Item Price'] = most_popular_five['Item Price'].map('${:,.2f}'.format)
most_popular_five['Total Purchase Value'] = most_popular_five['Total Purchase Value'].map('${:,.2f}'.format)

most_popular_five
```

    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:22: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:23: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy





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
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>$2.76</td>
      <td>$38.60</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>$3.46</td>
      <td>$34.65</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items

# gather data for each column 

# total number of purchases
most_prof_count = purchase_df.groupby('Item Name')['Item ID'].count()

# total of purchases per game
most_prof_rev = purchase_df.groupby('Item Name')['Price'].sum()

# price per game
most_prof_price = most_prof_rev/most_prof_count

# input to DF
most_prof = pd.DataFrame({"Purchase Count": most_prof_count, "Item Price": most_prof_price, "Total Purchase Value": most_prof_rev})

# sort by total purchase value and limit to top five
most_prof = most_prof.sort_values("Total Purchase Value", ascending=False)
most_profitable_five = most_prof.head()

# map formatting
most_profitable_five['Item Price'] = most_profitable_five['Item Price'].map('${:,.2f}'.format)
most_profitable_five['Total Purchase Value'] = most_profitable_five['Total Purchase Value'].map('${:,.2f}'.format)

most_profitable_five
```

    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:22: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:23: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy





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
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>$2.76</td>
      <td>$38.60</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>$3.46</td>
      <td>$34.65</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
  </tbody>
</table>
</div>


# Pandas-HW
