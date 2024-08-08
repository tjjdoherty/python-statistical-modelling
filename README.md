# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
- The goal of the project was to statistically model if there was a relationship between the bike availability and the number of Points of Interest (POIs herein) in the vicinity, their specific categories, and location of the bike station itself.
- To do this we can use the City Bike API, the Foursquare API and Yelp API, combine the data and extract points of interest and the locations and bike availability of the stations and do some data visualization and linear regression modelling.

## Process
- Step 1: CityBikes: I used the City Bikes API to find the 826 Bike stations in BikeShare Toronto's dataframe, and each station's individual bike availability (the number of free bikes divided by the total number of bike slots) as it was live when I requested the data on Saturday Aug 3, 2024. See city_bikes.ipynb for more.

- Step 2: Foursquare/Yelp: I used Foursquare's API to obtain various places of interest in the stations' vicinity (I chose 800m radius). I began playing with a sample request of one bike station latitude/longitude location and manipulating the data returned by Foursquare. I recorded the number of **bars/restaurants, parks/outdoor spaces, live venues and cafes** as well as the total number of POIs that fit those four categories, for each bike station. I used a number of small samples of the Foursquare API call to build up a list of category names for the venues/POIs that Foursquare/Yelp would return. See yelp_foursquare_EDA

- Step 3: Joining the Data: I joined the City Bikes and Foursquare data by importing the City Bikes data frame and calling the foursquare API on every bike station in the set. This means that for each of the 826 stations, Foursquare obtained up to 50 POIs in an 800 metre radius in the bar/restaurant, park, cafe and live venue categories. This gave me data on each bike station's number of nearby bars/restaurants, cafes, parks and live venues against the bike availability and latitude/longitude to train the model and perform a linear regression later.
- Step 3b: I also saved the individual venue data such as name, address, venue category for saving into a SQL .db file, as I did with the city bikes stations too. This leaves us with a database where we can join bike stations and tens of thousands of POIs across the city of Toronto in a SQL table/database by the latitude/longitude string.

- Step 4: Building a model: I used the data for some EDA and find correlations between the numeric data, using Seaborn correlation heatmaps and scatter graphs. I identified the strongest correlations, ran Pearson tests to find p-values to determine if the relationships were statistically significant, then fit a model to those features.

## Results
- The strongest correlation found in my model that was not a clear case of collinearity was latitude correlating with the number of POIs (-0.51 correlation) and latitude correlating with the bike availability (-0.31). These mean that as you travel south in Toronto (latitude is decreasing), the number of POIs increase as does the number of available bikes. This first finding is to be expected because Toronto is south-facing onto Lake Ontario so southernmost Downtown would be more densely packed with POIs. The second finding is of considerable value though as it suggests it is harder to find an available bike the further north you are in the city.


## Challenges 
- I decided not to run with Yelp as the API leaned heavily towards bar/restaurants rather than the other categories for the same location calls, and this would have introduced significant complexity for matching establishments from Foursquare and Yelp together, for the only tangible benefit being perhaps having data on the rating of individual establishments/venues. Catching the sub-category of venue was also more difficult in Yelp.
- Both Yelp and Foursquare limit the API call to 50 establishments maximum in the returning JSON data.
    
## Future Goals
- I would like to run the model on each individual category of POI which would be a simple step forward to make.
- The city_bike dataset should be gathered at different times of the week and ran to capture trends e.g. weekday rush hour (commuters), lunchtime in the week day (quiet time), late night on the weekend (nightlife), early morning weekend (few commutes happening).
- The Yelp API may need further exploration because it provided different returns to the Foursquare API, which was more generalized and caught many more non bar-restaurant establishments. Yelp offered far more specific restaurant/bar information, which I find it hard to believe that this would statistically influence bike_availability if the number of bars/restaurants themselves did not overall, but it would be interesting nonetheless.
