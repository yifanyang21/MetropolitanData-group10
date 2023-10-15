---
title: Assignment 3  –  Housing
next: /
toc: false
type: about
---


## Background:

Paris is hosting the Paralympics in 2024. One of the events at the Paralympics is an open water swimming event in the Seine (apparently the water is clean or so the French say). Since the City of Amsterdam thinks it is better than Paris, they want to host an event before the Paralympics, snubbing the Parisians. The idea is to host a 5km. open water swimming event through the canals of Amsterdam. You are asked by the municipality of Amsterdam to advise on the feasibility of the event from the perspective of the safety of the partaking athletes from an environmental perspective. The event is going to be hosted in May. 
For this exercise you need the data from this website. Download the Basis Bestand Gebieden Amsterdam (BBGA), both the data and the documentation. You already have the AirBnB data from Amsterdam from the workshop exercise.
The Municipality of Amsterdam is in a love hate relationship with AirBnb, see for example this and this article. Amsterdam wants to get a bit of insight in the number of tourists that will make use of AirBnB. 
Can you advise on or calculate for Amsterdam:
- What Amsterdam will receive from tourist tax if the event lasts a week and you will have 30.000 visitors?
- Plot the amount of AirBnB locations per neighbourhood.
- Which street in Amsterdam has the most AirBnB apartments?
- Try to cross reference the data from the AirBnB dataset with the BBGA. Can you figure out if all apartments of AirBnB are designated as housing? Which number of apartments are not rented out all the time but are also used as normal housing?
- How many hotel rooms should be built if Amsterdam wants to accommodate the same number of tourists?
- How many different licenses are issued?
Additionally, you will:
- Add the link to your Github project in the Excel in Teams.
- Read chapters 1, 2, 3 and 5, 6, 7 from the Think Python book.
---

## Our Solutions:

```python
#import libraries
import pandas as pd
import os.path
import matplotlib.pyplot as plt
from geopy.geocoders import Nominatim
import plotly.express as px

#open the files
bnb_csv = os.path.join(os.getcwd(), 'listings.csv')
bnb_df = pd.read_csv(bnb_csv)
```
### Question 1:  What Amsterdam will receive from tourist tax if the event lasts a week and you will have 30.000 visitors?

In order to calculate the tax income Amsterdam will receive if the event lasts for a week and has 30.000 visitors, we need to know the average cost of a hotel room, as well as that of holiday rentals, bed & breakfasts and short-stay accommodation. This is because the tourist tax is based on the cost of different types of accommodation. According to the website of the Amsterdam municipality the tourist tax for a hotel stay is 7% of the turnover, and 3 euros per day. For holiday rentals, bed & breakfast and short-stay accommodation this is 10% of the turnover. There is a different tourist tax for a stay on a campsite, but we are assuming that the visitors of this event will stay in the other types of accommodation since they are usually located in the city, whereas campsites would be located more outside the city.    
According to Broke Backpacker the average hotel price is around 180 USD, which equals around 171 Euros. The average price of an Airbnb would be around 80 USD for a one-bedroom apartment, which equals around 76 euros.  

```python
#define variables
#Average Hotel Price
H = 171
#Average AIRBNB Price
BNB = 76
#Number of days
Days = 7
#Number of visitors
ppl = 30000

#Calculate Tax for Hotel for number of days and people
Hotel_Tax = (0.07 * H + 3) * Days * ppl

#Calculate Tax for AirBNB for number of day and people
BNB_Tax = (0.1 * BNB) * Days * ppl

print("The Hotel Tax will be", format(Hotel_Tax,'.2f'))
print("The AirBNB Tax will be", format(BNB_Tax,'.2f'))
```

So, the amount of income generated from this event in terms of tourist tax highly depends on the type of accommodation visitors are staying in. If the event attracts 30.000 visitors, and all of these visitors stay in a hotel for 7 nights, total revenue in tourist tax will be <b>€3.143.700</b>. If these visitors stay in Airbnb's, which can be classified as holiday-rentals or short-stay accommodations, total revenue in tourist tax will be <b>€1.596.000</b>

Links:  
https://www.thebrokebackpacker.com/is-amsterdam-expensive/  
https://www.amsterdam.nl/en/municipal-taxes/tourist-tax-(toeristenbelasting)/#:~:text=If%20you%20offer%20paid%20accommodation,holiday%20rentals%20or%20bed%20%26%20breakfasts.&text=camping%20sites%3A%207%25%20and%20%E2%82%AC1%20per%20person%20per%20night.  

### Question 2： Plot the amount of AirBnB locations per neighbourhood.  

```python
fig = px.histogram(bnb_df, x="neighbourhood",text_auto=True)
fig.update_layout(
    height=800,
    title_text='The amount of AirBnb per neighbourhood'
)

fig.show()
```
![newplot](image\newplot.png)

### Question 3： Which street in Amsterdam has the most AirBnB apartments?  

```python
geolocator = Nominatim(user_agent= 'tryinams')
bnb_new = bnb_df[['id', 'latitude','longitude']]
bnb_new.insert(3,'street','')

# find out the location of per airbnb according to the coordinates
for i in range(len(bnb_new)):
    latitude, longitude = bnb_new.iloc[i]['latitude'], bnb_new.iloc[i]['longitude']
    location = geolocator.reverse(f"{latitude},{longitude}", timeout=None)
    try:
        street = location.raw['address']['road']
        bnb_new.loc[i, 'street'] = street
    except:
        pass

bnb_new.head()
```
<i>
id	latitude	longitude	street
0	761411	    52.40164	4.95106	Jisperveldstraat
1	768274	    52.38855	4.88521	Zoutkeetsplein
2	768737	    52.37824	4.86826	Centrale Groothandelsmarkt
3	771217	    52.34091	4.84802	IJsbaanpad
4	771343	    52.37641	4.88303	Derde Egelantiersdwarsstraa
</i>

```python
#find out the street with most airbnb apartment
street_top5 = bnb_new['street'].value_counts().sort_values(ascending=False).head(5)
street_top5
```
<i>
street
Nassaukade                          194
Derde Egelantiersdwarsstraat        80
Hoofdweg                            70
Prinsengracht                       60
Admiraal De Ruijterweg              54
Name: count, dtype: int64
</i>

According to the result above, we can say that the <b>Nassaukade</b> has most AirBnB

```python
# if we only focus on the "Entire home/apt" type, then the program can be like this:
bnb_apt = bnb_df[bnb_df.room_type == 'Entire home/apt'].copy()
bnb_apt_new = bnb_apt[['id', 'latitude','longitude']]
bnb_apt_new.insert(3,'street','')

geolocator = Nominatim(user_agent= 'tryapartment')

for i in range(len(bnb_apt_new)):
    latitude, longitude = bnb_apt_new.iloc[i]['latitude'], bnb_apt_new.iloc[i]['longitude']
    location = geolocator.reverse(f"{latitude},{longitude}", timeout=None)
    try:
        street = location.raw['address']['road']
        bnb_apt_new.loc[i, 'street'] = street
    except:
        pass

street_apt_top5 = bnb_apt_new['street'].value_counts().sort_values(ascending=False).head(5)
street_apt_top5
```

### Question 4： Try to cross reference the data from the AirBnB dataset with the BBGA. Can you figure out if all apartments of AirBnB are designated as housing? Which number of apartments are not rented out all the time but are also used as normal housing?

SSSorry, I do not understand this question yet....



### Question 5： How many hotel rooms should be built if Amsterdam wants to accommodate the same number of tourists?

According to BBGA dataset, there are 34758 hotel rooms in Amsterdam with 76823 beds, which means 2.2 beds are equal to a hotel room on average.

```python
#find out how many beds are there provided by Airbnb
detail_name = bnb_df['name'].str.split('·', expand=True)
detail_name.columns = ['property', 'rating', 'bedrooms', 'beds', 'baths']
num_beds = detail_name['beds'].str.extract('([\d.]+)').astype(float)
total_beds = num_beds[0].sum()
print(int(total_beds))
total_rooms = total_beds / 2.2
print(int(total_rooms))
```

So, <b>6687</b> hotel rooms should be built if if Amsterdam wants to accommodate the same number of tourists.

### Question 6： How many different licenses are issued?

solution 1:
```python
print(len(bnb_df['license'].value_counts()))
```
solution 2:
```python
print(len(bnb_df['license'].value_counts()))
```
Therefore, there are <b>7288</b> licenses issued.

solution 3:
But why it is 1 more than the results before?(7289)
```python
license = []

for i in bnb_df['license']:
    if i in license:
        pass
    else:
        license.append(i)

print(len(license))
```