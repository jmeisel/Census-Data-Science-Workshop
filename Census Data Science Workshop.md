## US Census Bureau - Data Science Workshop


Technical Support:

Gitter Channel


### Exercise:  Demonstrate North Dakota's Demographic Changes Due to Fracking

Contributed by Ari Lamstein, author of free email course Learn to Map Census Data in R.  Sign up for Ari's course [here](http://www.arilamstein.com/free-course/). 

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


If you're new to CartoDB, **making an account is easy.** Just click [THIS LINK](http://goo.gl/forms/PfhLNxXidL) and follow the instructions. These accounts are a level-up from our standard free accounts because we <3 you!

#### Welcome to your CartoDB Dashboard!

**The Dasboard** is like the command center for managing your datasets, maps, and account.

![Dashboard](http://i62.tinypic.com/bgr9et.png)

The blue bar on top allows you to:
* Navigate between your maps and datasets
  * Datasets are the backbone of your map visualization. It is important to note that by switching to the **datasets view**, you can work with the data stored on the CartoDB database directly.
* Visit our [CartoDB gallery](https://cartodb.com/gallery/)
* Learn new skills on our [documentation page](http://docs.cartodb.com/).


By clicking the icon on the far right you can:
* Manage your **account settings**
* Visit your **public profile**
* View your **API key**
  * Which will be important for using our APIs and some external tools such as OGR.

![image](http://i61.tinypic.com/16mnbt.png)

### Time to make our first map!

Let's begin by clicking the **green New Map button**. Then choosing to **Make a map from scratch** in the popup.

![button](http://i60.tinypic.com/242ty7k.png)



<ol>
  <li><a href="http://academy.cartodb.com">Map Academy</a>
    <ul>
      <li><a href="http://academy.cartodb.com/courses/01-beginners-course.html">Beginner</a></li>
      <li><a href="http://academy.cartodb.com/courses/02-design-for-beginners.html">Map design</a></li>
      <li><a href="http://academy.cartodb.com/courses/03-cartodbjs-ground-up/lesson-3.html">CartoDB.js</a> – build a web app to visualize your data, allowing for more user interaction</li>
      <li><a href="http://academy.cartodb.com/courses/04-sql-postgis.html">SQL and PostGIS</a> – slice and dice your geospatial data</li>
    </ul>
  </li>
  <li><a href="http://docs.cartodb.com/tutorials.html">CartoDB Tutorials</a></li>
  <li><a href="http://docs.cartodb.com/cartodb-editor.html">CartoDB Editor Documentation</a></li>
  <li><a href="http://docs.cartodb.com/cartodb-platform.html">CartoDB APIs</a></li>
  <li><a href="http://gis.stackexchange.com/questions/tagged/cartodb">Community help on StackExchange</a></li>
  <li><a href="http://cartodb.com/gallery/">CartoDB Map Gallery</a></li>
</ol>


### Thanks for joining our session!  Please follow Census on Github [here](http://www.arilamstein.com/free-course/) and email Jeff with any questions or suggestions for future improvements <jeffrey.meisel@census.gov>!
