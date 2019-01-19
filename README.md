# Analysis of Citibike - New York bike sharing system and researching dependencies between the time of ride and distance travelled.


## Introduction
Bike sharing systems becoming more and more popular in the last years. There are thousands of rides happening every day. In this research, I’ve tried to investigate dependencies between the time of ride and distance travelled, which factors could influence this dependency and if there is an influence of weather during the ride.

## Methodology
The methodology implies the combined use of two approaches:

1st : statistical analysis of data: graph of bike sharing system’s rides and investigation of most popular stations

2nd: ML model to predict distance of ride based on different input values.


## Results

![Average daily use of bike-sharing across all months](total_rides/rides.png?raw=true "Average daily use of bike-sharing across all months")


##### Popular Stations

![The most popular stations](popular_stations/the_most_popular.png?raw=true "10 red dots - the most popular stations")

The most popular stations are all located in the economic and administrative centre of New York - Manhattan.
Such stations mainly located at crossroads of main streets, such as Broadway, 8 Ave & W 31 St, 8 Ave & W 33 St, University Pl & E 14 St, or at very lively places.
Those are Pershing Square North(nearby there is Grand Central Terminal), crossroads of 8 Ave(nearby there is Pennsylvania Station), Central Park, World Trade Center, etc.
The least popular stations mainly located outside Manhattan or at not lively streets.

There were also missing data: in some rows values of "start station id", "start station name", "end station id", "end station name" columns were undefined.
I've tried to solve this using GoogleMaps API, by getting the name of location by station's latitude and longitude and assign new station id.
But as those stations were not popular and corresponding columns were not used in further work, the results are not included in the project.

##### Dependency between time and distance

The first step of the experiment was the prediction of distance just based on the time of ride(seconds).
In the second step to the time of ride was added information about cycler(a type of user: subscriber/customer, age, sex).
In the third step to all previous features weather data was added: temperature(C), humidity(%), precipation(mm), wind speed(m/s) and cloudiness(%).

During training linear regression on small part of dataset(100 - 1000 samples) accuracy on train was 50-60% while on test 20-40%. While increasing the number of samples to 10'000 accuracy has felt to 8-10% both on train and test. On the whole dataset, accuracy is 0.1%-0.8% depending on the number of features included and if weather data used.

Such a bad accuracy is gained probably because of distance which was predicted. It was Euclidean distance between stations but not a distance travelled by cycler. Two stations could be nearby and cycler could move between them with a large hook: distance travelled and time are large but the distance between stations is not big what causes the unexpected outcome.

![Results](linear_regression/results.png?raw=true "Results of model training")

## Conclusions

Average daily use of bikes is higher during May – Oct, while the most popular bike stations are all located in Manhattan.

Time of the ride and distance travelled (distance between the start point and the end point)  are almost not correlated.

Using additional features (information about cycler and weather data) can increase coefficient of determination by up to 0,7%




Bike sharing system data:
citibikenyc.com
s3.amazonaws.com/tripdata/index.html

Weather data:
rp5.ua
