# Traffic heatmap

We will build some more advanced data visualization. It willl be useres engagement heatmap. The darker color the highest user engagement \(avgSessionDuration\) was on this time of day. Inspired by **[Todd Moy.](http://toddmoy.com/)**

```r
# traffic heatmap
# based on https://github.com/toddmoy/Google-Analytics-Heatmap/blob/master/traffic_heatmap.R

# install libraries
# install.packages("googleAuthR")
# install.packages("googleAnalyticsR")
# install.packages("ggplot2")
# install.packages("RColorBrewer")

# load libraries
library("googleAuthR")
library("googleAnalyticsR")
library("ggplot2")
library("RColorBrewer")

# authorize connection with Google Analytics servers
ga_auth()

## pick a profile with data to query
#ga_id <- account_list[275,'viewId']

# or give it explicite using tool http://michalbrys.github.io/ga-tools/table-id.html in format 99999999
ga_id <- 00000000

gadata <- google_analytics(id = ga_id, 
                           start="2012-01-01", end="2016-06-30", 
                           metrics = c("avgSessionDuration"),
                           dimensions = c("dayOfWeekName", "hour"),
                           max = 5000)


# order data
gadata$dayOfWeekName <- factor(gadata$dayOfWeekName, levels = c("Sunday", 
                                                          "Monday", 
                                                          "Tuesday", 
                                                          "Wednesday", 
                                                          "Thursday", 
                                                          "Friday", 
                                                          "Saturday"))
gadata[order(gadata$dayOfWeekName),]

# convert data frame to xtab
heatmap_data <- xtabs(avgSessionDuration ~ dayOfWeekName + hour, data=gadata)


```

When data is prepared, we'll prepare plot:

```r
# plot heatmap
heatmap(heatmap_data, 
        col=colorRampPalette(brewer.pal(9,"Blues"))(100), 
        revC=TRUE,                                        
        scale="none",                                     
        Rowv=NA, Colv=NA,                                 
        main="avgSessionDuration by Day and Hour",                 
        xlab="Hour") 
```

And the result is:

![](/assets/6_heatmap.png)

In this case - wednesday morning is the most engaging for users time of the day :\)

## Source code

Complete code for this example in GitHub repository:

[github.com\/michalbrys\/R-Google-Analytics\/blob\/master\/6\_heatmap.R](https://github.com/michalbrys/R-Google-Analytics/blob/master/6_heatmap.R)

