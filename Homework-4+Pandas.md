

```python
#Import Dependencies
import pandas as pd

```


```python
#Import the raw data 
json_path = "HeroesOfPymoli/Resource/purchase_data.json"
purchase_df= pd.read_json(json_path)
purchase_df.head()

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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



### Player Count


```python
# Count total number of players
count_player= purchase_df["SN"].nunique()
count_player_df = pd.DataFrame([{"Total Players":count_player}])
count_player_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Total)


```python
# Purchasing Analysis
unique_item = purchase_df["Item ID"].value_counts().count()
average_price= purchase_df["Price"].mean()
total_number_purchase = len(purchase_df.index)
total_revenue = purchase_df["Price"].sum()
purchasing_analysis = pd.DataFrame({"Number of Unique Item":[unique_item],
                                    "Avergae Price":["$"+ str(round(average_price,2))],
                                    "Number of purchase":[total_number_purchase],
                                    "Total Revenue":["$" + str(round(total_revenue,2))]})  


purchasing_analysis

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
      <th>Avergae Price</th>
      <th>Number of Unique Item</th>
      <th>Number of purchase</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>183</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>



### Gender Demographics



```python
#Unique Players
unique_players_df= purchase_df.drop_duplicates(subset="SN")

#Unique Players Gender
unique_players_gender=unique_players_df.groupby("Gender")
unique_players_gender.head()
unique_players_gender_df = pd.DataFrame({
    "Percentage of Players": unique_players_gender["Age"].count()/count_player*100,
    "Total Count":unique_players_gender["Age"].count()
})

unique_players_gender_df 
#Format
unique_players_gender_df["Percentage of Players"]=unique_players_gender_df["Percentage of Players"].map("{:.2f}".format)
unique_players_gender_df
unique_players_gender_df.sort_values(by="Total Count",ascending=False)
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Gender)


```python
#count the times of purchase male and female bought(could repeat)
purchase_gender_count=purchase_df.groupby("Gender")
purchase_gender_count.count()["Age"]
#Create new df
purchase_gender_count_df=pd.DataFrame({"Purchase Count":purchase_gender_count.count()["Age"],
                                      "Average Purchase Price":purchase_gender_count["Price"].mean(),
                                      "Total Purchase Value":purchase_gender_count["Price"].sum(),
                                      "Normalized Totals":purchase_gender_count.sum()["Price"]/unique_players_gender_df["Total Count"]})



purchase_gender_count_df.sort_values(by="Total Purchase Value",ascending=False)
#Format
purchase_gender_count_df["Average Purchase Price"]=purchase_gender_count_df["Average Purchase Price"].map("{:.2f}".format)

purchase_gender_count_df["Normalized Totals"]=purchase_gender_count_df["Normalized Totals"].map("{:.2f}".format)

purchase_gender_count_df["Total Purchase Value"]=purchase_gender_count_df["Total Purchase Value"].map("{:.2f}".format)

purchase_gender_count_df

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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
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
      <td>2.82</td>
      <td>3.83</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>2.95</td>
      <td>4.02</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>3.25</td>
      <td>4.47</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>



### Age Demographics


```python
#Check the max and min age of the data
print(purchase_df["Age"].min())
print(purchase_df["Age"].max())
```

    7
    45



```python
# Set bins and labels
bins = [0,10,15,20,25,30,35,40,45]
group_labels = ["< 10","10 - 14","15 - 19","20 - 24","25 - 29","30 - 34","35 - 39","40+"]
pd.cut(unique_players_df["Age"], bins, labels=group_labels)

unique_players_df["Age Range"]=pd.cut(unique_players_df["Age"], bins, labels=group_labels)
unique_players_df.head()
# Creating a group based off of the bins
unique_players_age_group=unique_players_df.groupby("Age Range")
unique_players_age_group.count()["Age"]

unique_players_age_group_df= pd.DataFrame({
    "Percentage of Players":unique_players_age_group.count()["Age"]/count_player*100,
    "Total Count": unique_players_age_group.count()["Age"]})



#Format

unique_players_age_group_df["Percentage of Players"]=unique_players_age_group_df["Percentage of Players"].map("{:.2f}".format)

unique_players_age_group_df
```

    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>9.42</td>
      <td>54</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>24.26</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>40.84</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>9.08</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Age)


```python
#Set bins and labels
bins = [0,10,15,20,25,30,35,40,45]
group_labels = ["< 10","10 - 14","15 - 19","20 - 24","25 - 29","30 - 34","35 - 39","40+"]

#Add Age Range row
unique_players_df["Age Range"]=pd.cut(unique_players_df["Age"], bins, labels=group_labels)
unique_players_df


a = unique_players_df.groupby(['Age Range']).count()["SN"] 
b = unique_players_df.groupby(['Age Range']).mean()["Price"]
c= unique_players_df.groupby(['Age Range']).sum()["Price"]
d =c/unique_players_age_group_df["Total Count"]
d
age_range_df=pd.DataFrame({"Purchase Count":a,
                           "Average Purchase Price":b,
                           "Total purchase value":c,
                           "Normalized total":d
    
})
age_range_df
age_range_df["Average Purchase Price"]=age_range_df["Average Purchase Price"].map("{:.2f}".format)
age_range_df["Normalized total"]=age_range_df["Normalized total"].map("{:.2f}".format)

age_range_df

age_range_df
```

    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





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
      <th>Average Purchase Price</th>
      <th>Normalized total</th>
      <th>Purchase Count</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>3.16</td>
      <td>3.16</td>
      <td>22</td>
      <td>69.50</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>2.89</td>
      <td>2.89</td>
      <td>54</td>
      <td>155.94</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>2.90</td>
      <td>2.90</td>
      <td>139</td>
      <td>403.12</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>2.99</td>
      <td>2.99</td>
      <td>234</td>
      <td>699.12</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>2.92</td>
      <td>2.92</td>
      <td>52</td>
      <td>151.65</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>3.33</td>
      <td>3.33</td>
      <td>44</td>
      <td>146.48</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>2.87</td>
      <td>2.87</td>
      <td>25</td>
      <td>71.64</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.88</td>
      <td>2.88</td>
      <td>3</td>
      <td>8.64</td>
    </tr>
  </tbody>
</table>
</div>



### Top Spenders


```python
#Group by 'SN'
purchase_bySN = purchase_df.groupby("SN")
topSpenders_df = pd.DataFrame({
    "Purchase Count" : purchase_bySN["Item ID"].count(),
    "Average Purchase Price": purchase_bySN["Price"].mean(),
    "Total Purchase Value": purchase_bySN["Price"].sum()
})

topSpenders_df["Average Purchase Price"]=topSpenders_df["Average Purchase Price"].map("{:.2f}".format)
topSpenders_df[[
    "Purchase Count",
    "Average Purchase Price",
    "Total Purchase Value"
]].sort_values(
    by=["Total Purchase Value"],
    ascending=False).head()
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
      <td>3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



### Most Popular Items


```python
#Group by itemID
purchase_by_item=purchase_df.groupby(["Item ID","Item Name"])
purchase_by_item.count()["SN"]

popular_item_df=pd.DataFrame({"Purchase Count":purchase_by_item.count()["SN"],
                              "Item Price":purchase_by_item["Price"].mean(),
                            "Total Purchase Value":purchase_by_item["Price"].sum()
})

popular_item_df[[
    "Purchase Count",
    "Item Price",
    "Total Purchase Value"
]].sort_values(
    by=["Purchase Count"],
    ascending=False).head()

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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



### Most Profitable Items


```python
most_profitable_df = popular_item_df
most_profitable_df[[
    "Purchase Count",
    "Item Price",
    "Total Purchase Value"
]].sort_values(
    by=["Total Purchase Value"],
    ascending=False
).head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>



### Observations


```python
#1.There are more male players than the female players playing this game.
#2.It looks like the age group 20-24 contributes the most profit to the game business.
#3.The game seems not that popular among the older demographics since it takes less than 1% to the total players.
```
