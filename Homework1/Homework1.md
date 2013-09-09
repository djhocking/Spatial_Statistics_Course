# Homework 1: Using R for Distances and Mapping
Homework 1 (lecture 1 slide 31)
Spatial Statistics
Daniel J. Hocking
Started: 04 September 2013

## P0: Read Sections 3.1 and 3.2 of the text

## P1: Find the geo-coordinates (longitude, latitude) of New York, Seattle, and Los Angeles and calculate the pairwise distances:


```r
# Problem 1
library(maps)
library(mapproj)
library(knitr)

# From Google NYC = 40.6700° N, 73.9400° W LA = 34.0500° N, 118.2500° W

NYC.lat <- 40.67
NYC.lon <- -73.94
LA.lat <- 34.05
LA.lon <- -118.25

# Convert to Radians
NYC.lat.rad <- NYC.lat * pi/180
NYC.lon.rad <- NYC.lon * pi/180
LA.lat.rad <- LA.lat * pi/180
LA.lon.rad <- LA.lon * pi/180
```


### a) naively: by using Euclidean distances directly on the longitude/latitude
Use Pythagorean theorem to solve

```r
dist.euclid.rad <- sqrt((NYC.lat.rad - LA.lat.rad)^2 + (NYC.lon.rad - LA.lon.rad)^2)
dist.euclid <- sqrt((NYC.lat - LA.lat)^2 + (NYC.lon - LA.lon)^2)
```


The Euclidean distance = 44.8018 degrees. Given the lack of projection, it is impossible to correctly convert this to relevant distances such as kilometers. In radians the distance = 0.78194 radians.

### b) correctly by using the geodesic distances

```r
dist.geo <- 3678 * acos(sin(NYC.lat.rad) * sin(LA.lat.rad) + cos(NYC.lat.rad) * 
    cos(LA.lat.rad) * cos(NYC.lon.rad - LA.lon.rad))
```


The geodesic distance between NYC and LA is 2,276 km. For comparison with the previous answer, this can be divided by 3678 to convert to radians `dist.geo/3678` = 0.6188 radians.

### c) by using straight line connections that cut through the earth (apply some geometry here)


## P2: Obtain a map of the US states and project it using the three popular
projections: Albers, Lambert and Bonne. Comment on the resulting graphs. Use:

```r
library(maps)
library(mapproj)
par(mfrow = c(2, 2), mar = c(0, 0, 1, 0))  # puts multiple graphs on page
map("state")
title("long and lat")
map("state", proj = "albers", par = c(30, 40))
title("albers")
map("state", proj = "bonne", param = 35)
title("bonne")
map("state", proj = "lambert", par = c(30, 40))
title("lambert")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


## P3: Obtain a map that contains Greenland and the contiguous USA. Plot a long/lat map and then choose 3 projections – select appropriate parameters for these. Comment on the effect of the projections. R code for unprojected map:
map("world", xlim=c(-130, -20),ylim=c(25,85)); title("long,lat") # map.axes(); # optional: adds axes to the map!


