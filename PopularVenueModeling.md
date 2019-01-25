# Popular Venue Modeling

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
