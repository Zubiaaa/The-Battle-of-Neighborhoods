# Coursera Capstone-The Battle of Neighborhoods 
[TOC]

# Section 1: Introduction

In this section I will clearly define the idea of my choosing, where I leverage the Foursquare location data to solve the imagined business opportunity. 

## Project Idea

My idea for the Capstone Project is to show that when driven by venue and location data from FourSquare, backed up with open source crime data, that it is possible to present the cautious and nervous traveller with a list of attractions to visit supplementd with a graphics showing the occurance of crime in the region of the venue.

A high level approach is as follows:

1. The travellers decides on a city location [in this case Chicago]
2. The ForeSquare website is scrapped for the top venues in the city
3. From this list of top venues the list is augmented with additional grographical data
4. Using this additional geographical data the top nearby restaurents are selects

## Target Audience

This solution is targeted at the cautious traveller. The want to see all the main sites of a city that they have never visited before but at the same time, for whatever reaons unknown, they want to be able to do all that they can to make sure that they stay clear of trouble i.e. is it safe to visit this venue and this restaurant at 4:00 pm in the afternoon.

Some examples of envisioned users include:

* A single white female traveller
* An elderly traveller that has had previous back experiences when travelling

There are many data science aspect of this project including:

1. Data Acquisition
2. Data Cleansing
3. Data Analysis
4. Machine Learning
5. Prediction

Now that the conference is over the Data Sceintist can explore Chigago and feel much safer.



# Section 2: Data

## Data Description

In this section, I will describe the data used to solve the problem as described previously. 

As noted below in the Further Development Section, it is possible to attempt quite complex and sophisticated scenarios when approaching this problem. However, given the size of the project and for simplicity only the following scenario will be addressed:

1. Query the FourSqaure website for the top sites in Chicago
2. Use the FourSquare API to get supplemental geographical data about the top sites
3. Use the FourSquare API to get top restaurent recommendations closest to each of the top site

## Top Sites from FourSquare Website

This data, however, is easily available directly from the FourSquare Website. To do this simply go to www.foursquare.com, enter the city of your choic and select Top Picks from _I'm Looking For_ selection field.

A sample of the extracted data is given below:

| id                       | score | category      | name                         | href                                              |
| ------------------------ | ----- | ------------- | ---------------------------- | ------------------------------------------------- |
| 42b75880f964a52090251fe3 | 9.7   | Park          | Millennium Park              | /v/millennium-park/42b75880f964a52090251fe3       |
| 4b9511c7f964a520f38d34e3 | 9.6   | Trail         | Chicago Lakefront Trail      | /v/chicago-lakefront-trail/4b9511c7f964a520f38... |
| 49e9ef74f964a52011661fe3 | 9.6   | Art Museum    | The Art Institute of Chicago | /v/the-art-institute-of-chicago/49e9ef74f964a5... |
| 4f2a0d0ae4b0837d0c4c2bc3 | 9.6   | Deli / Bodega | Publican Quality Meats       | /v/publican-quality-meats/4f2a0d0ae4b0837d0c4c... |
| 4aa05f40f964a520643f20e3 | 9.6   | Theater       | The Chicago Theatre          | /v/the-chicago-theatre/4aa05f40f964a520643f20e3   |

## Final FourSquare Top Sites Data

A sample of the final FourSquare Top Sites data is shown below:

| id                       | score | category      | name                         | address                 | postalcode | city    | href                                              | latitude  | longitude  |
| ------------------------ | ----- | ------------- | ---------------------------- | ----------------------- | ---------- | ------- | ------------------------------------------------- | --------- | ---------- |
| 42b75880f964a52090251fe3 | 9.7   | Park          | Millennium Park              | 201 E Randolph St       | 60601      | Chicago | /v/millennium-park/42b75880f964a52090251fe3       | 41.882662 | -87.623239 |
| 4b9511c7f964a520f38d34e3 | 9.6   | Trail         | Chicago Lakefront Trail      | Lake Michigan Lakefront | 60611      | Chicago | /v/chicago-lakefront-trail/4b9511c7f964a520f38... | 41.967053 | -87.646909 |
| 49e9ef74f964a52011661fe3 | 9.6   | Art Museum    | The Art Institute of Chicago | 111 S Michigan Ave      | 60603      | Chicago | /v/the-art-institute-of-chicago/49e9ef74f964a5... | 41.879665 | -87.623630 |
| 4f2a0d0ae4b0837d0c4c2bc3 | 9.6   | Deli / Bodega | Publican Quality Meats       | 825 W Fulton Market     | 60607      | Chicago | /v/publican-quality-meats/4f2a0d0ae4b0837d0c4c... | 41.886642 | -87.648718 |
| 4aa05f40f964a520643f20e3 | 9.6   | Theater       | The Chicago Theatre          | 175 N State St          | 60601      | Chicago | /v/the-chicago-theatre/4aa05f40f964a520643f20e3   | 41.885578 | -87.627286 |

#### Data Analysis and Visualisation

An initial look at the data shows that there are 30 rows of data [as expected] each with 10 attributes. The variable types are all correct except the Venue Rating or Score which will be converted to a float. After converting the score column to a float it can clearly be seen that we have the top venues with a mean of `9.532`.

```python
df_top_venues.shape
(30, 10)

df_top_venues.dtypes
id             object
score          object
category       object
name           object
address        object
postalcode     object
city           object
href           object
latitude      float64
longitude     float64
dtype: object
    
df_top_venues.score.describe()
count    30.000000
mean      9.523333
std       0.072793
min       9.400000
25%       9.500000
50%       9.500000
75%       9.600000
max       9.700000
Name: score, dtype: float64
```



We are now ready to get the top restaurents within 500 meters of each of the top sites.

## FourSquare Restaurent Recommendation Data

Using the the list of all `id` values in the Top Sites DataFrame and the FourSquare `categoryID` that represents all food venues we now search for restaurants within a 500 meter radius.
```
Using just the data in this DataFrame we will be able to generate maps displaying the chosen Top List Venue and the best scored surrounding restaurants. A sample of this data is shown below:

| id                       | score | category        | categoryID               | name                      | address           | postalcode | city    | latitude  | longitude  | venue_name      | venue_latitude | venue_longitude |
| ------------------------ | ----- | --------------- | ------------------------ | ------------------------- | ----------------- | ---------- | ------- | --------- | ---------- | --------------- | -------------- | --------------- |
| 55669b9b498ee34e5249ea61 | 9.2   | Gastropubs      | 4bf58dd8d48988d155941735 | Cindy's                   | 12 S Michigan Ave | 60603      | Chicago | 41.881695 | -87.624600 | Millennium Park | 41.882662      | -87.623239      |
| 556509d6498e726bdec19fe9 | 8.4   | Burger Joints   | 4bf58dd8d48988d16c941735 | Shake Shack               | 12 S Michigan Ave | 60603      | Chicago | 41.881673 | -87.624455 | Millennium Park | 41.882662      | -87.623239      |
| 49e749fbf964a52086641fe3 | 9.1   | Gastropubs      | 4bf58dd8d48988d155941735 | The Gage                  | 24 S Michigan Ave | 60603      | Chicago | 41.881319 | -87.624642 | Millennium Park | 41.882662      | -87.623239      |
| 4e879cdc93adfd051d6d609e | 9.2   | Breakfast Spots | 4bf58dd8d48988d143941735 | Wildberry Pancakes & Cafe | 130 E Randolph St | 60601      | Chicago | 41.884599 | -87.623203 | Millennium Park | 41.882662      | -87.623239      |
| 49d8159cf964a520a05d1fe3 | 8.5   | Pubs            | 4bf58dd8d48988d11b941735 | Miller's Pub              | 134 S Wabash Ave  | 60603      | Chicago | 41.879911 | -87.625972 | Millennium Park | 41.882662      | -87.623239      |

Looking at the data we get an interesting insight into the range of restuarants that are included. From a list of 30 top venues only 28 actually had more than 10 to provide the user with a real choice. In total there were 387 restaurants found of which 240 were unique occuring only once in the data. There were 72 categories of restaurants. The mean score of all the restaurants wa `8.23` with a manimum value of `9.5` and a minimum value of `5.3`.  

Coffee Shops (52) and Pizza Places (29) were the top two most frequently occurring categories but Pie Shops (9.4000) and French Restaurants (9.4000) were the restaurant categories with the highest average score.

```python
# What is the shape of the Restaurants DataFrame
df_restaurant.shape
(387, 13)

# Get a count of the top venues that had more than 10 restaurant within 500 meters
#  The number of unique restaurants
#  The number of unique restaurant categories
df_restaurant.venue_name.nunique()
28
df_restaurant.name.nunique()
240
df_restaurant.category.nunique()
72

# Look at the data types
df_restaurant.dtypes
id                  object
score              float64
category            object
categoryID          object
name                object
address             object
postalcode          object
city                object
latitude           float64
longitude          float64
venue_name          object
venue_latitude     float64
venue_longitude    float64
dtype: object
    
# Describe the Score attribute
df_restaurant.score.describe()
count    387.000000
mean       8.286563
std        0.930138
min        5.300000
25%        7.800000
50%        8.500000
75%        9.000000
max        9.500000
Name: score, dtype: float64
        
df_restaurant.groupby('category')['name'].count().sort_values(ascending=False)[:10]
category
Coffee Shops                        52
Pizza Places                        29
Cafés                               24
Bakeries                            15
Burger Joints                       15
Gastropubs                          15
New American Restaurants            15
Mexican Restaurants                 14
Breakfast Spots                     13
Fast Food Restaurants               13


df_restaurant.groupby('category')['score'].mean().sort_values(ascending=False)[:10]
category
Pie Shops                           9.4000
French Restaurants                  9.4000
Molecular Gastronomy Restaurants    9.3000
Filipino Restaurants                9.2000
Cuban Restaurants                   9.1000
Ice Cream Shops                     9.0625
Mediterranean Restaurants           9.0600
Korean Restaurants                  9.0000
Latin American Restaurants          9.0000
Fish & Chips Shops                  9.0000
```
