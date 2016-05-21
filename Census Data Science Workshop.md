## US Census Bureau - Data Science Workshop


Technical Support:

Gitter Channel


### Exercise:  Demonstrate North Dakota's Demographic Changes Due to Fracking

* This exercise was contributed by Ari Lamstein, author of free email course Learn to Map Census Data in R.  Sign up for Ari's course [here](http://www.arilamstein.com/free-course/).
* The Census dataset used in this exercise is the American Community Survey.  The R package to provide access to the American Community Survey is called "acs" from author Ezra Haber Glenn.   

####Overview
North Dakota has been being transformed by the fracking boom. North Dakota sits on the Bakken formation which, due to fracking, is now able to be monetized.

In this exercise, you will create two maps which demonstrate North Dakota’s recent demographic changes. The first shows that between 2010 and 2013 North Dakota’s Per Capita Income grew at a rate of 15%, significantly above any other US state. The second one shows that North Dakota’s Median Age decreased by 2%, significantly below any other US state. The following steps describe how to create these maps in R.

####Step 1: Install and Load the Necessary Packages

We’ll use the choroplethr and choroplethrMaps packages to create these maps. To install and load them, type the following from an R console:

```
install.packages(c(“choroplethr”, “choroplethrMaps”))
library(choroplethr)
library(choroplethrMaps)
```

####Step 2: Get and Register a US Census API Key
The data we’ll be mapping comes from the US Census Bureau’s American Community Survey (ACS). To access their API you need an API key, which you can get for free [here](http://api.census.gov/data/key_signup.html). Once you have a key, type the following from an R console:

```
library(acs)
api.key.install(“<your key>”)
```

####Step 3: Get the Data

The ACS began in 2005, but only data from 2010 thru 2013 is available via the API. We can get 2010 and 2013 data by typing the following:

```
demo_2010 = get_state_demographics(2010)
demo_2013 = get_state_demographics(2013)
```

To see the values that are available, look at the column names:

```
colnames(demo_2013)
[1] "region" "total_population" "percent_white" "percent_black"   
[5] "percent_asian" "percent_hispanic" "per_capita_income"  
[8] "median_rent"  "median_age"
```

Before calculating the percent change, merge the two data frames together by region. Note that “region” here is just a state name:

```
demo_all = merge(demo_2010, demo_2013, by="region")
colnames(demo_all)

 [1] "region"              "total_population.x" "percent_white.x"   
 [4] "percent_black.x"     "percent_asian.x"    "percent_hispanic.x"
 [7] "per_capita_income.x" "median_rent.x"      "median_age.x"      
[10] "total_population.y"  "percent_white.y"    "percent_black.y"   
[13] "percent_asian.y"     "percent_hispanic.y" "per_capita_income.y"
[16] "median_rent.y"       "median_age.y" 
```

The 2010 values now have .x appended to them, and the 2013 values have .y appended to them.

####Step 4: Map the Percentage Change for Per Capita Income

We’ll use the function state_choropleth to create choropleth maps of the demographic changes. First we need to create a column called value which represents the percent change in Per Capita Income between the two years:

```
demo_all$value = (demo_all$per_capita_income.y - demo_all$per_capita_income.x) / demo_all$per_capita_income.x * 100
```

Now we can create the map:

```
state_choropleth(demo_all)
```

The main problem with this map is the scale: it does not allow us to distinguish between negative and positive values. A more appropriate scale is scale_fill_gradient2: it makes 0 white, negative values red and positive values blue. To change the scale we need to use the object oriented features of choroplethr. Here is code to change the scale as well as do other things, such as remove the state labels.

```
choro = StateChoropleth$new(demo_all)
choro$title = "State Per Capita Income\n2010-2013 Percent Change"
min = min(demo_all$value)
max = max(demo_all$value)
choro$show_labels = FALSE
choro$set_num_colors(1)
choro$ggplot_scale = scale_fill_gradient2(name="Percent Change", limits = c(min, max))
income_map = choro$render()
income_map
```

####Step 5: Map the Percentage Change for Median Age

We can create a similar map for changes in the median age using the same pattern:
```
demo_all$value = (demo_all$median_age.y - demo_all$median_age.x) / demo_all$median_age.x * 100
choro = StateChoropleth$new(demo_all)
choro$title = "State Median Age\n2010-2013 Percent Change"
choro$set_num_colors(1)
min = min(demo_all$value)
max = max(demo_all$value)
choro$show_labels = FALSE
choro$ggplot_scale = scale_fill_gradient2(name="Percent Change", limits=c(min, max))
age_map = choro$render()
age_map
```

####Step 6: Merge the Maps

Finally, we can merge the two maps using the gridExtra package:

```
library(gridExtra)
grid.arrange(income_map, age_map)
```

## Resources
<ol>
  <li><a href="https://uscensusbureau.github.io/citysdk/">CitySDK on Github from the US Census Bureau</a></li>
  <li><a href="http://www.arilamstein.com/free-course/">Email Course "Learn to Map Census Data in R" by Ari Lamstein</a>
    <ul>
    </ul>
  </li>
</ol>


### Thanks for joining our session!  Please follow Census on Github [here](http://www.arilamstein.com/free-course/) and email Jeff with any questions or suggestions for future improvements <jeffrey.meisel@census.gov>!
