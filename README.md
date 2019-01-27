# Analysis of Citibike - New York bike sharing system and researching dependencies between the time of ride and distance travelled.


## Introduction
Bike sharing systems becoming more and more popular in the last years. There are thousands of rides happening every day. In this research, I’ve tried to investigate dependencies between the time of ride and distance travelled, which factors could influence this dependency and if there is an influence of weather during the ride.

## Methodology
The methodology implies the combined use of two approaches:

1st : statistical analysis of data: graph of bike sharing system’s rides and investigation of most popular stations

2nd: ML model to predict distance of ride based on different input values. Firstly, model adequacy was checked: if all conditions for linear regression are satisfied. Those are linearity, nearly normal residuals, constant variability. Also, investigation of outliers was made.
Then, linear regression model was trained on three datasets: the first one was just two columns: time and distance, the second has included information about cycler, the third one has included weather data.


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

##### Dealing with outliers

![Data](check_conditions/original_data.png?raw=true "Data")

There are some outliers on this plot. The biggest value is nearly equal to 7.5 months of ride and there are other huge values. As we cannot deal with these large numbers we could delete them from our data. We left entries where time of ride was smaller than 1 hour. If time was bigger - we do not include that entry. There are only 0.3% of such entries.

![Data without outliers](check_conditions/hour_data.png?raw=true "Data without outliers")

##### Model adequacy
Conditions for linear regression:

1. Linearity: relationship between x and y should be linear, checking using a scatterplot of data or residual plots.
![Data with least squares line](check_conditions/data_line.png?raw=true "Data with least squares line")

As we can see from the plot there is a lot of scatter around the data, so it is more probable that there is no linearity

2. Nearly normal residuals: residuals should be nearly normally distributed centred at 0, checking using a histogram or normal probability plot of residuals

![Residual plot and histogram](check_conditions/residuals_plot_histogram.png?raw=true "Residual plot and histogram")

![Normal probability plot](check_conditions/normal_probability_plot.png?raw=true "Normal probability plot")

As we could see from plots above, residuals are symmetric and centred at 0 but normal probability plot showed that they save normality close to 0, but at tails, they are going away from normality

3. Constant variability: variability of points around the least squares line should be constant, checking using residual plot.

As we could see from residuals plot, variability of residuals around 0 is not constant. As x increases, y increases as well.

So, we observe fan-shaped data: when the value of x is low, the variability of y is also low, but while x increases, variability of y is increases as well; the histogram of residuals look symmetric and it is centred at 0; normal probability plot showed that we actually going away from normality.


##### Dependency between time and distance

The first step of the experiment was the prediction of distance just based on the time of ride(seconds).
In the second step to the time of ride was added information about cycler(a type of user: subscriber/customer, age, sex).
In the third step to all previous features weather data was added: temperature(C), humidity(%), precipation(mm), wind speed(m/s) and cloudiness(%).

During training linear regression on small part of dataset(100 - 1000 samples) accuracy on train was 50-60% while on test 20-40%. While increasing the number of samples to 10'000 accuracy has felt to 8-10% both on train and test. On the whole dataset, accuracy is 0.1%-0.8% depending on the number of features included and if weather data used.

Such a bad accuracy is gained probably because of distance which was predicted. It was Euclidean distance between stations but not a distance travelled by cycler. Two stations could be nearby and cycler could move between them with a large hook: distance travelled and time are large but the distance between stations is not big what causes the unexpected outcome. One more reason - training model with outliers. There were not a lot of them, but they influenced accuracy a lot. Training Step 1 without outliers gave accuracy 61% on test data.

![Results](linear_regression/results.png?raw=true "Results of model training")

## Conclusions

Average daily use of bikes is higher during May – Oct, while the most popular bike stations are all located in Manhattan.

Time of the ride and distance travelled (distance between the start point and the end point)  are almost not correlated.

Using additional features (information about cycler and weather data) can increase coefficient of determination by up to 0,7%



##### Additionaly:
Bike sharing system data:

citibikenyc.com

s3.amazonaws.com/tripdata/index.html

Weather data:
rp5.ua

Literature:
D.C. Montgomery "Introduction to Linear Regression Analysis"

