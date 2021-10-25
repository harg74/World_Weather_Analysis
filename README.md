# World_Weather_Analysis

## Overview of the analysis:

We applied analysis, visualization and statistical skils while learning how to retrieve spacific data through Application Programming Interfaces (APIs), specifically Opean Weather Map and Google Maps APIs.

## Results:

### 1. Retreive information from Open Weather Maps and CitiPy module:

Along with another lines of code, in order to get our random coordinates (latitudes and longitudes) we used ```np.random.uniform``` function to generate them. These coordines are the ones that are going to be used to find the nearest city with the help of the CitiPy module and construct our Data Frame which was exported into a file named: WeatherPy_Database.csv 

See full code here: ---> [https://github.com/harg74/World_Weather_Analysis/blob/main/Weather_Database/Weather_Database.ipynb]

```
#Create a set of random lats y lngs combinations

lats=np.random.uniform(low=-90.000, high=90.000, size=2000)
lngs=np.random.uniform(low=-180.000, high=180.000, size=2000)
lat_lngs=zip(lats,lngs)
lat_lngs

```

### 2. Filter by preferred cities by Maximum Temperature:

<img width="748" alt="Screen Shot 2021-10-25 at 10 46 59" src="https://user-images.githubusercontent.com/78564912/138728227-b14a9065-8262-469a-bd52-a25eb1669133.png">

Once we retreived our random coordinates, we applied a filter for the cities that had a temparature above 70º F, but below 95º F and store it in a data frame.

See full code here: ---> [https://github.com/harg74/World_Weather_Analysis/blob/main/Vacation_Search/Vacation_Search.ipynb]

![temperatures](https://user-images.githubusercontent.com/78564912/138730297-2dbc8254-306e-4c42-a4fc-da944064f24a.png)

### 3. Add marker layer with gmaps:

Based on the coordinates we were able retreive, we added Hotel Name to our dataframe and search for hotels within a radius of 5,000 meters with gmaps from the nearest city.

<img width="1082" alt="Screen Shot 2021-10-25 at 12 32 58" src="https://user-images.githubusercontent.com/78564912/138742899-746de768-68d1-448c-8d0b-54462d0bbfb1.png">

```

# 6a. Set parameters to search for hotels within 5000 meters

#dictionary to drop the rows with no Hotel Name.
hotels_no_name=[]

params = {
    "radius": 5000,
    "type": "lodging",
    "key": g_key
}

# 6b. Iterate through the hotel DataFrame.
for index, row in hotel_df.iterrows():
    lat= row['Lat']
    lng=row['Lng']
    
    # Add the latitude and longitude to location key for the params dictionary.
    params["location"] = f"{lat},{lng}"

    # 6c. Get latitude and longitude from DataFrame
    hotel_df[["Lat", "Lng"]]
    
    # 6d. Set up the base URL for the Google Directions API to get JSON data.
    base_url = "https://maps.googleapis.com/maps/api/place/nearbysearch/json"

    # 6e. Make request and retrieve the JSON data from the search. 
    hotels = requests.get(base_url, params=params).json()
    
    # 6f. Get the first hotel from the results and store the name, if a hotel isn't found skip the city.
    try:
        hotel_df.loc[index, "Hotel Name"] = hotels["results"][0]["name"]
    except IndexError:
        pass
        print('IndexError',index)
        #indexes rows to be dropped with no Hotel Name
        if index not in hotels_no_name:
            hotels_no_name.append(index)

print(hotels_no_name)
hotel_df

```

After these lines we added a marker layer for each city found with the coordinates to the map with all the cities available to vacation.

![WeatherPy_vacation_map](https://user-images.githubusercontent.com/78564912/138747932-d9fe72c5-a243-4618-b5b6-862cff803c02.png)

### 4. Select four cities to vacation and make a itinerary:

Finally we selected four random cities to plan a vacation and four Brazil's cities were selected make a four stops in this cpuntry

1. Start at: Santa Maria
2. Visit Tapes city
3. VIsit Rio Grande
4. Visit Cidreira
5. End at: Santa Maria

See full code here: ---> https://github.com/harg74/World_Weather_Analysis/blob/main/Vacation_Itinerary/Vacation_Itinerary.ipynb

![WeatherPy_travel_map](https://user-images.githubusercontent.com/78564912/138750132-fb8a3705-945c-41a9-810a-44afb45a1f36.png)

![WeatherPy_travel_map_markers](https://user-images.githubusercontent.com/78564912/138750734-310f6589-19f0-4930-8ffe-59d02c67faf3.png)
