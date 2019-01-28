# Popular Venue Modeling

#### Michael Harreld

## Introduction

Investors and developers must choose cities and venue types that will be successful and return on their investments.
Choosing what to build in a particular city is a difficult prospect and could involve many factors.
Location data services such as Foursquare will show what current venues exist in a particular city, but they do not generally tell what venue types _should_ be popular in that city.

This project explores the development of a model that will predict what venue types would be popular based on analyzing popular types in similar cities.
Such a model would help developers see what venue types would be popular in a particular city and whether those types might be underrepresented there.
The developer could use this information to make better project decisions and increase their return.
Also, a model of venue popularity could expose trends and patterns.
For example, it may be possible to find venue types that are popular in a certain region or city size.
Having this information could lead to better long term planning.

## Data

Two data sources will be used: Wikipedia for city location and population data; and Foursquare for city venue data.
These two data sources both use Latitude and Longitude which can be used to relate their data sets.

### Wikipedia City Data

The web page https://en.wikipedia.org/wiki/List_of_United_States_cities_by_population contains a table with the top 311 United States cities by population.
The table includes city and state name, population data, land area, population density, and city coordinates.
Sample data for the first row is
| Column Name    | Value       |
| -----------    | -----       |
| 2017 rank      | 1           |
| City           | New York    |
| State          | New York    |
| 2017 estimate  | 8,622,698   |
| 2010 Census    | 8,175,133   |
| Change         | +5.47%      |
| 2016 land area | 301.5 sq mi |
| 2016 land area | 780.9 km2   |
| 2016 population density | 28.317/sq mi |
| 2016 population density | 10.933/km2 |
| Location | 40.6635 N 73.9387 W |

Some observations and usage decisions on this table:
* There are 2 values for population, we will use the 2017 estimate and discard the 2010 value.
* We will discard the Change value since it seems like it would be secondary to venue popularity.
* Both the land area and population density values are given in both imperial and metric, we will discard the imperial versions.
* Since land area can be determined based on population density and population, we will discard it.
* Location data is given as a string and within the table data includes both the decimal and minutes-seconds formats - we will have to extract the decimal form.
* Overall, the data in the table is messy and includes superscripts, units, and other noise that will need to be cleaned.

After all of the cleanup we will be left with the following data as candidates to our data model:
City, State, Population, Population Density, Latitude, and Longitude.

### Foursquare Venue Data

The Foursquare web site provides many APIs for interacting with city location data.
The API we will be using is venue search and is documented here https://developer.foursquare.com/docs/api/venues/search
The API takes a location as an input and returns a list of venues close to that location.
Each returned venue contains a name, address info, and a venue category.
A venue category could be, for example, Mexican Restaurant or Bank.
We will use the Foursquare venue data to determine the frequencies of various venue categories within a particular city.
The returned data is in json format and will have to have relevant values extracted and structured.

## Methodology

Intro to methodology

Pull in city population, density, and location data

Clean data to City	State	Population	Density	Latitude	Longitude

Pull and process venue data

top 75 cities, 50 venues nearest to city center

City	City Latitude	City Longitude	Venue	Venue Latitude	Venue Longitude	Venue Category

Calculate venue category frequency per city

Grouped by venue category to find counts of each

Found unique categories

Created a one hot table to have a column for each category

grouped one hot table by city and found mean of each venue category to find fractionals

examined the top 5 venue categories for each city

Perform clustering of cities based on similarity of top venue categories
to see if there was likely a location to top venue correlation
also looked at each cluster to characterize

Methodology section which represents the main component of the report where you discuss and describe any exploratory data analysis that you did, any inferential statistical testing that you performed, and what machine learnings were used and why.

Exploration of relationships of population and density to venue popularity
Looking first at population and density, create a new dataframe containing the number one venue category of each city along with its population and density
Group this data by venue category and find the mean of population and density for each category.

Graphed Mean City Population and Density per venue categores

Modeling of venue popularity from population, density, and location
categorical model Since population, density, and location all showed feasible relationships to venue popularity, use them as independent variables in the modeling.
The target variable is most popular venue category.
Choose a decision tree since the independent variable is categorical.

held out 20% of data 60/15
trained to tree depth 4



## Results

section where you discuss the results.

389 unique categories

map-seems like some correlation - e.g. red and green coastal and midwest, blue tending towards north east, purple san francisco only, orange corpus cristi only
red (0) diverse restaurants and coffe shops
purple(1) san fran harbor related venues
blue(2) bars
green(3) mexican restaurants
orange(4) corpus christi resort related venues

Graphed Mean City Population and Density per venue categores
Table of data for venue rank 1
maximum for both pop and density caribbean restaurant
minimum pop portuguese restaurant
minimum density discount store

tree picture

oveall most likely coffee shop
  south of long beach CA- mexican restaurant
    smaller than san antonio tx - sandwich place
    larger  than san antonio tx - mexican restaurant
  north of long beach CA- coffee shop
    west of rockford IL- coffee shop 
    east of rockford IL - bar

Predicted 8 out of 15 in the top ten venues

stats table

## Discussion

section where you discuss any observations you noted and any recommendations you can make based on the results.

lots of restaurants
only small range of downtown, limited to 50
model predict top 10 instead of 1
cluster distance based on set inclusion instead of exact rank match

## Conclusion section where you conclude the report.

2. A link to your Notebook on your Github repository pushed showing your code. (15 marks)

3. Your choice of a presentation or blogpost. (10 marks)


**
lots of restaurants
only small range of downtown, limited to 50
model predict top 10 instead of 1
cluster distance based on set inclusion instead of exact rank match
