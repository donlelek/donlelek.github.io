---
layout: post
title: Using R for data wrangling
comments: true
---
## Get your dataset ready for the analysis

[Stata](www.stata.com) has been my main analysis tool for the last year, mainly because it is what everybody uses around [here](http://healthmgt.upei.ca/), recently **R** is becoming increasingly accesible with the arrival of the [`dplyr`](http://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html) and [`tidyr`](http://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html) packages.

Inspired by this [Data wrangling webinar](https://github.com/rstudio/webinars/blob/master/2015-01/wrangling-webinar.pdf) offered by [Rstudio](www.rstudio.com) I thought of trying my new abilities with a real dataset.

I found a great Ebola dataset compiled by [Caitlin Rivers](https://github.com/cmrivers/ebola) that provided sufficient challenge for me.

### Get the the data

The first challenge was getting the data. Even stored as a `csv` file this was not a straight forward task since 

{% highlight r %}

read.csv("https://raw.githubusercontent.com/cmrivers/ebola/master/country_timeseries.csv")

{% endhighlight %}	

resulted in **Error in file(file, "rt") : cannot open the connection**. After some research I found that the proper way of importing a `csv` from an *URL* is using `getURL` from the `RCurl` package. My final code for importing the data looks like this:

{% highlight r %}

library(RCurl)  

library(dplyr)  
x <- getURL("https://raw.githubusercontent.com/cmrivers/ebola/master
/country_timeseries.csv")    

ts <- tbl_df(read.csv(text = x))  #import as data table  

{% endhighlight %}	

Now lets get a look at what we have using `glimpse(ts)`

	Variables:  
	$ Date                (fctr) 1/5/2015, 1/4/2015, 1/3/...  
	$ Day                 (int) 289, 288, 287, 286, 284, ...  
	$ Cases_Guinea        (int) 2776, 2775, 2769, NA, 273...  
	$ Cases_Liberia       (int) NA, NA, 8166, 8157, 8115,...  
	$ Cases_SierraLeone   (int) 10030, 9780, 9722, NA, 96...  
	$ Cases_Nigeria       (int) NA, NA, NA, NA, NA, NA, N...  
	$ Cases_Senegal       (int) NA, NA, NA, NA, NA, NA, N...  
	$ Cases_UnitedStates  (int) NA, NA, NA, NA, NA, NA, N...  
	$ Cases_Spain         (int) NA, NA, NA, NA, NA, NA, N...  
	$ Cases_Mali          (int) NA, NA, NA, NA, NA, NA, N...  
	$ Deaths_Guinea       (int) 1786, 1781, 1767, NA, 173...  
	$ Deaths_Liberia      (int) NA, NA, 3496, 3496, 3471,...  
	$ Deaths_SierraLeone  (int) 2977, 2943, 2915, NA, 282...  
	$ Deaths_Nigeria      (int) NA, NA, NA, NA, NA, NA, N...  
	$ Deaths_Senegal      (int) NA, NA, NA, NA, NA, NA, N...  
	$ Deaths_UnitedStates (int) NA, NA, NA, NA, NA, NA, N...  
	$ Deaths_Spain        (int) NA, NA, NA, NA, NA, NA, N...  
	$ Deaths_Mali         (int) NA, NA, NA, NA, NA, NA, N...  


Well, it worked, but the table is in *wide* format!. I know that most of the analyses I would like to perform need that the data is in *long* format, let's do that.

### Prepare your data

What I want to do here can be described in the following steps:  
	
1. reshape the data from wide to long format
2. extract the name of the country and the type of observation (*Case* or *Death*)
3. get rid of missing values (flagged as *NA*)

Let's do it!

{% highlight r %}

library(tidyr)

# assign everything to a new table                       
cases <- ts %>%                                           
  # from columns 3 to 18 put the name of the variable in column "C" and the value of the cell in "N"  
  gather(., "C", "N", 3:18) %>%                           
  # split the string in "C" at the "_" sign and put the results in "Type" and "Country"    
  separate(., C, c("Type", "Country"), sep = "_") %>%     
  # get rid of the NA's  
  filter(N != "NA")                                       
  
  glimpse(cases)   # let's peek  

{% endhighlight %}	

	Variables:    
	$ Date    (fctr) 1/5/2015, 1/4/2015, 1/3/2015, 12/31/2014,...  
	$ Day     (int) 289, 288, 287, 284, 281, 280, 277, 273, 27...  
	$ Type    (chr) "Cases", "Cases", "Cases", "Cases", "Cases...  
	$ Country (chr) "Guinea", "Guinea", "Guinea", "Guinea", "G...  
	$ N       (int) 2776, 2775, 2769, 2730, 2706, 2695, 2630, ...  

I think I've managed to do what I wanted in a few lines of code so let's wrap up this post with a quick plot.

{% highlight r %}
  
library(ggplot2)  
qplot(data     = cases,  
        x      = Day,  
        y      = N,  
        color  = Country,  
        group  = Country,  
        facets = . ~ Type,  
        geom   = "line")  

{% endhighlight %}	


![Simple plot](https://dl.dropboxusercontent.com/u/128600/posts/Screenshot%202015-01-23%2014.45.15.png)


Thanks for reading, Cheers!

