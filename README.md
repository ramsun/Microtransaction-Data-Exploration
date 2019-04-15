

```python
'''
Based on the data bellow, we can draw 3 conclusions
1.  There was an overwhemlming majority of men who were involved in microtransactions in the game 
    (84.03% of players), while the number of women who were involed in microtransactions was only 
    14.06%.
2.  The age demographics that are involved in the most amount of microtransactions are between the 
    ages of 19 and 26, where 57.12% of users fall.  There is also a sizeable demographic in the 
    15-18 range at 15.62%.
3.  Both the most popular and most profitable items were the same in this data set.  The most 
    valuable item was Oathbreaker, Last Hope of the Breaking Storm, which brought in 50.76.
'''
```


```python
# import dependencies
import pandas as pd
import numpy as np
```


```python
# create a raw data parent data frame from a given csv
path = "./purchase_data.csv"
raw_df = pd.read_csv(path)

# display the first 5 rows of the raw data
raw_df.head()
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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Player Count Analysis
# Identify the total number of unique SN tags from the raw data frame
SN_series = raw_df["SN"].value_counts()
player_count = len(SN_series)

# print the total to the notebook
print("Number of players in Heroes of Pyoli: " + str(player_count))
```

    Number of players in Heroes of Pyoli: 576
    


```python
# Purchasing Analysis (Total)
# Perform a summary analysis of the raw data and determine the 4 key aspects of the data,
# which includes the number of unique items, the average purchase price, the total number of
# Purchases, and the total amount of revenue that was made from the data set

# perform the summary analysis
item_series = raw_df["Item Name"].value_counts() 
num_unique_items = len(item_series)
revenue = raw_df["Price"].sum()
purchase_count = len(raw_df)
avg_purchase_price = round(revenue / purchase_count, 2)

# create a dictionary of the summary data and put it into a data frame for output
purchase_data = {"Number of Unique Items" : num_unique_items , 
                    "Average Purchase Price" : avg_purchase_price ,
                    "Total Number of Purchases" : purchase_count,
                    "Total Revenue" : revenue}
purchasing_analysis = pd.DataFrame(data=purchase_data, index = ["Purchasing Analysis"])

# output to notebook
purchasing_analysis
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
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Purchasing Analysis</th>
      <td>179</td>
      <td>3.05</td>
      <td>780</td>
      <td>2379.77</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender demographics (continued)
# shows the number of genders and their respective counts

#create a series based on each gender in the raw data
male_series = raw_df[raw_df.Gender == "Male"]["SN"].value_counts()
female_series = raw_df[raw_df.Gender == "Female"]["SN"].value_counts()
other_series = raw_df[raw_df.Gender == "Other / Non-Disclosed"]["SN"].value_counts()

# determine the count of each gender
male_count = len(male_series)
female_count = len(female_series)
other_count = len(other_series)

# calculate percentages
male_percentage = round(male_count/player_count * 100,2)
female_percentage = round(female_count/player_count * 100,2)
other_percentage = round(other_count/player_count * 100,2)

# initialize the output table with an index based on gender
gender_index_arr = ["Male", "Female", "Other / Non-Disclosed"]
gender_demographics = pd.DataFrame(index = gender_index_arr)

#assign each gender's data column by column
gender_demographics['Total Count'] = [male_count,female_count,other_count]
gender_demographics['Percentage of Players'] = [male_percentage,female_percentage,other_percentage]

# output to notebook
gender_demographics
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
      <td>484</td>
      <td>84.03</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
# This will be a purchasing analysis of the raw data based on gender

# initialize a new df that will contain summary purchase info based on gender
purchasing_analysis_df = raw_df.groupby("Gender").Price.agg(["count", "mean","sum"])

# total money spent based on gender of the user
female_revenue = raw_df[raw_df.Gender == "Female"].Price.sum()
male_revenue = raw_df[raw_df.Gender == "Male"].Price.sum()
other_revenue = raw_df[raw_df.Gender == "Other / Non-Disclosed"].Price.sum()

# average purchase count for each gender
female_average_purchase_total_per_person = round(female_revenue/female_count, 2)
male_average_purchase_total_per_person = round(male_revenue/male_count, 2)
other_average_purchase_total_per_person = round(other_revenue/other_count,2)

# assigns values to the output df and clean it up
purchasing_analysis_df["Average Purchase Total Per Person"] = [female_average_purchase_total_per_person,male_average_purchase_total_per_person,other_average_purchase_total_per_person]
purchasing_analysis_df["mean"] = round(purchasing_analysis_df["mean"],2)
purchasing_analysis_df = purchasing_analysis_df.rename(columns = {"count": "Purchase Count", "mean":"Average Purchase Price", "sum":"Total Purchase Value"})

# ouput to notebook
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Total Per Person</th>
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
      <td>113</td>
      <td>3.20</td>
      <td>361.94</td>
      <td>4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>3.02</td>
      <td>1967.64</td>
      <td>4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>3.35</td>
      <td>50.19</td>
      <td>4.56</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age demographics
# Come up with demographics data based on the number of unique users

# create the bins and labels
bins = [0,10,14,18,22,26,30,34,38,42,100]
age_ranges = ["<=10", "11-14", "15-18", "19-22", "23-26","27-30", "31-34", "35-38", "39-42", "43+"]

# create a new data frame that only keeps the purchases from unique users
unique_SN_df = raw_df
unique_SN_df = unique_SN_df.drop_duplicates(subset = 'SN', keep = 'first')

# create an age demographics report based off of the unique SN data frame
age_demographics_df = unique_SN_df.groupby(pd.cut(unique_SN_df["Age"], bins,labels = age_ranges))
age_demographics_df = age_demographics_df.count()
age_demographics_df = age_demographics_df.drop(columns = ['SN', 'Age','Gender','Item ID','Item Name','Price']) 
age_demographics_df = age_demographics_df.rename(columns = {"Purchase ID":"Total Count"}) 
age_demographics_df["Percentage of Players"] = round(age_demographics_df["Total Count"] / player_count * 100,2)

# output to notebook
age_demographics_df
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
    <tr>
      <th>Age</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;=10</th>
      <td>24</td>
      <td>4.17</td>
    </tr>
    <tr>
      <th>11-14</th>
      <td>15</td>
      <td>2.60</td>
    </tr>
    <tr>
      <th>15-18</th>
      <td>90</td>
      <td>15.62</td>
    </tr>
    <tr>
      <th>19-22</th>
      <td>178</td>
      <td>30.90</td>
    </tr>
    <tr>
      <th>23-26</th>
      <td>151</td>
      <td>26.22</td>
    </tr>
    <tr>
      <th>27-30</th>
      <td>48</td>
      <td>8.33</td>
    </tr>
    <tr>
      <th>31-34</th>
      <td>27</td>
      <td>4.69</td>
    </tr>
    <tr>
      <th>35-38</th>
      <td>25</td>
      <td>4.34</td>
    </tr>
    <tr>
      <th>39-42</th>
      <td>14</td>
      <td>2.43</td>
    </tr>
    <tr>
      <th>43+</th>
      <td>4</td>
      <td>0.69</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Age)

# create an age purchase report, which will now include every purchase in the analysis and not just unique purchases
age_purchase_analysis_df = raw_df.groupby(pd.cut(raw_df["Age"], bins,labels = age_ranges))
age_purchase_analysis_df = age_purchase_analysis_df.count()
age_purchase_analysis_df = age_purchase_analysis_df.drop(columns = ['SN', 'Age','Gender','Item ID','Item Name','Price']) 
age_purchase_analysis_df = age_purchase_analysis_df.rename(columns = {"Purchase ID":"Total Count"}) 
age_purchase_analysis_df["Percentage of Players"] = round(age_purchase_analysis_df["Total Count"] / player_count * 100,2)

# create a binned raw data data frame for analysis
raw_data_binned_df = raw_df
raw_data_binned_df["Binning"] = pd.cut(raw_data_binned_df["Age"], bins,labels = age_ranges)

# initialize the lists that will be used as column data for the output data frame
tot_pur_val_list = []
avg_pur_price_list = []
avg_pur_price_per_person_list = []

# create a list for the total purchase value and average purchase price based on the label
for label in age_ranges:
    tot_pur_val_list.append(raw_data_binned_df[raw_data_binned_df.Binning == label].Price.sum())
    avg_pur_price_list.append(round(raw_data_binned_df[raw_data_binned_df.Binning == label].Price.mean(),2))

# create a list that will make use of data from the a data frame in the previous cell
avg_pur_price_per_person_list = round(tot_pur_val_list / age_demographics_df["Total Count"],2)

# append the lists from the previous loop to the output data frame
age_purchase_analysis_df["Average Purchase Price"] = avg_pur_price_list
age_purchase_analysis_df["Total Purchase Value"] = tot_pur_val_list
age_purchase_analysis_df["Average Total Purchase Per Person"] = avg_pur_price_per_person_list

#clean up and print to notebook
age_purchase_analysis_df
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Average Total Purchase Per Person</th>
    </tr>
    <tr>
      <th>Age</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;=10</th>
      <td>32</td>
      <td>5.56</td>
      <td>3.40</td>
      <td>108.96</td>
      <td>4.54</td>
    </tr>
    <tr>
      <th>11-14</th>
      <td>19</td>
      <td>3.30</td>
      <td>2.68</td>
      <td>50.95</td>
      <td>3.40</td>
    </tr>
    <tr>
      <th>15-18</th>
      <td>113</td>
      <td>19.62</td>
      <td>3.03</td>
      <td>342.91</td>
      <td>3.81</td>
    </tr>
    <tr>
      <th>19-22</th>
      <td>254</td>
      <td>44.10</td>
      <td>3.04</td>
      <td>771.89</td>
      <td>4.34</td>
    </tr>
    <tr>
      <th>23-26</th>
      <td>207</td>
      <td>35.94</td>
      <td>3.06</td>
      <td>634.24</td>
      <td>4.20</td>
    </tr>
    <tr>
      <th>27-30</th>
      <td>63</td>
      <td>10.94</td>
      <td>2.88</td>
      <td>181.23</td>
      <td>3.78</td>
    </tr>
    <tr>
      <th>31-34</th>
      <td>38</td>
      <td>6.60</td>
      <td>2.73</td>
      <td>103.68</td>
      <td>3.84</td>
    </tr>
    <tr>
      <th>35-38</th>
      <td>35</td>
      <td>6.08</td>
      <td>3.55</td>
      <td>124.35</td>
      <td>4.97</td>
    </tr>
    <tr>
      <th>39-42</th>
      <td>15</td>
      <td>2.60</td>
      <td>3.37</td>
      <td>50.50</td>
      <td>3.61</td>
    </tr>
    <tr>
      <th>43+</th>
      <td>4</td>
      <td>0.69</td>
      <td>2.77</td>
      <td>11.06</td>
      <td>2.76</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
# Determines the top 5 users who spent the most on the microtransactions

# create a new data frame grouped by the SN tag
gb_SN_df = raw_df.groupby("SN").Price.agg(["count","mean","sum"])

# create new df that will hold the rows information of the top 5 spenders
top_spenders_df = gb_SN_df.nlargest(5, 'sum')
top_spenders_df["mean"] = round(top_spenders_df["mean"],2)
top_spenders_df = top_spenders_df.rename(columns = {"count":"Purchase Count","mean":"Average Purchase Price", "sum": "Total Purchase Value"})

# output to notebook
top_spenders_df
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>3.79</td>
      <td>18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>3.86</td>
      <td>15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>4.61</td>
      <td>13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>3.40</td>
      <td>13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>4.37</td>
      <td>13.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
# Determines which items are the most popular based on number of sales

# create a new data frame grouped by each unique item id, name, and price
gb_items_df = raw_df.groupby(["Item ID", "Item Name", "Price"]).Price.agg(["count", "sum"])

# return the top 5 items based on purchase count
popular_items_df = gb_items_df.nlargest(5, 'count')
popular_items_df = popular_items_df.rename(columns = {"count":"Purchase Count", "sum":"Total Purchase Value"}) # clean headers

# output to notebook
popular_items_df
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
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <th>4.23</th>
      <td>12</td>
      <td>50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <th>4.90</th>
      <td>9</td>
      <td>44.10</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <th>3.53</th>
      <td>9</td>
      <td>31.77</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <th>4.58</th>
      <td>9</td>
      <td>41.22</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <th>1.02</th>
      <td>8</td>
      <td>8.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
# Determinesthe top 5 which items provided the most profit in terms of total purchase value

# create a new data frame that organizes the raw data by the item id, item name, and price, all 
gb_items_df = raw_df.groupby(["Item ID", "Item Name", "Price"]).Price.agg(["count", "sum"])

# return the top 5 items based on purchase count
# create a new data frame that contains the rows of the items with the higest total purchase value (aka sum) 
profitable_items_df = gb_items_df.nlargest(5, 'sum')
profitable_items_df = popular_items_df.rename(columns = {"count":"Purchase Count", "sum":"Total Purchase Value"}) # clean headers

# output to notebook
profitable_items_df
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
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <th>4.23</th>
      <td>12</td>
      <td>50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <th>4.90</th>
      <td>9</td>
      <td>44.10</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <th>3.53</th>
      <td>9</td>
      <td>31.77</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <th>4.58</th>
      <td>9</td>
      <td>41.22</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <th>1.02</th>
      <td>8</td>
      <td>8.16</td>
    </tr>
  </tbody>
</table>
</div>


