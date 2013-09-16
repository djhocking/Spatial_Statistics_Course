Homework 2
--------------------------------------------------

This is an R Markdown document. Markdown is a simple formatting syntax for authoring web pages (click the **MD** toolbar button for help on Markdown).

When you click the **Knit HTML** button a web page will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


```r
summary(cars)
```

```
##      speed           dist    
##  Min.   : 4.0   Min.   :  2  
##  1st Qu.:12.0   1st Qu.: 26  
##  Median :15.0   Median : 36  
##  Mean   :15.4   Mean   : 43  
##  3rd Qu.:19.0   3rd Qu.: 56  
##  Max.   :25.0   Max.   :120
```


You can also embed plots, for example:


```r
plot(cars)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


## Daniel J. Hocking
## Spatial Statistics
## Started: 16 September 2013

### P1(a) Show that the isotropic spherical correlation function ρ(t) equals 0 when t = θ , i.e. the correlation length = θ where θ denotes the range parameter. In other words, for the spherical model, the range parameter is the effective range (or correlation length).

### P1(b) Determine the correlation lengths (= effective ranges) of the following isotropic correlation functions parameterized using the range parameter θ : exponential, powered exponential, Gaussian, rational quadratic. Note: for all these the correlation length = the distance at which the correlation reaches 0.05.


### P2 Assume the following 6 locations in 2-d space: 

```r
sx = c(2, 3, 4, 3.5, 2, 2.5)
sy = c(1, 1.5, 2, 3, 3.5, 4)
```


### a) First: plot the locations. Then, use R to define a function that creates a pairwise distance matrix as follows:

The S (or R) function “outer” calculates generalized outer products of arrays. Applying this function to two vectors a ,b, does the following: It creates a matrix with # rows = length of a, and # columns = length of b, and each element (i,j) of this matrix contains the combination of ai , bj plus any operation applied to these two values. For example stating: FUN = “+” will add these two values, FUN = “*” will multiply, etc.


```r

twod.dist = function(sx, sy) {
    sqrt(outer(sx, sx, "-")^2 + outer(sy, sy, "-")^2)
}
```


Execute the function, and then apply it to the two vectors above. This creates the pairwise distance matrix for the above data. Print this matrix with a two digit precision (use the round (...,2) command) as follows:


```r
distmat = twod.dist(sx, sy)
round(distmat, 2)
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6]
## [1,] 0.00 1.12 2.24 2.50 2.50 3.04
## [2,] 1.12 0.00 1.12 1.58 2.24 2.55
## [3,] 2.24 1.12 0.00 1.12 2.50 2.50
## [4,] 2.50 1.58 1.12 0.00 1.58 1.41
## [5,] 2.50 2.24 2.50 1.58 0.00 0.71
## [6,] 3.04 2.55 2.50 1.41 0.71 0.00
```



### b) At the above locations we observe a response which is a realization from an isotropic spatial random field with a constant mean, and constant variance that is equals to 1, and some correlation function with correlation length 2.5. In this exercise we use the matrix of pairwise distances above and calculate the variance – covariance matrix according to a given covariance function model. Write a script that calculates the following (use only elementary operations, do not use geoR specific functions)
(1) the exponential covariance matrix with the above parameters,
(2) the Whittle’s covariance matrix with the above parameters. Note for the Whittle correlation function we have the relation: correlation length = 4.744 * θ , where θ is the range parameter.
For statistics Ph.D. students only: Prove this relation (hint: you may need to solve this numerically)
(3) the Gaussian covariance matrix with the above parameters.
Hint for (1),(2), and (3): You need to first find the range parameter for each model (use results from P1 b and d) ) and then apply the covariance function to the entries in the pairwise distance matrix of a). Print the results with two digits. (use round again). This may look like this (eg: for the exponential cov model)
r.expo = 2.5 / 3 # calculates the range parameter cov.expo = exp(-distmat/r.expo); round(cov.expo,2)
1

### c) Obtain an overlay line plot of the three models in b over a grid of pairwise distances from 0 to 4) eg: dists = seq(.05,4,by=.05);
plot(dists,exp(-dists/r.expo),type='l',ylim=c(0,1)) # expo model lines(dists,... ) # Whittle model
lines(dists,... ) # Gaussian model

### d) Use the geoR function cov.spatial to obtain the same plots as in c). Note that the Whittle model equals the Matern model with smoothness parameter ν = 1.5 (called kappa in geoR). Read the help file for cov.spatial for all the specifics. This may look like this:
plot(dists,cov.spatial(dists,"expo",cov.pars=c(1,r.expo)),type='l',ylim=c(0,1))
etc. for the lines command.

### P3: Consider the Calcium Data in geoR. They consist of soil calcium measurements in Brazil in three regions. Please read the helpfile on “ca20”.

### a) Read the data and subset by only using region 3. This is the experimental region with 116 data points. Plot the data.
Hint: use: data(ca20);
ca.reg3 = subset(ca20,area==3)
plot(ca.reg3)
Interpret the resulting graph.

### b) Use the “variog” function to calculate the semivariogram. First obtain the omnidirectional (isotropic) semivariogram and compare the classical estimator with the robust estimator. Use:
plot(variog(ca.reg3,trend="cte", max.dist = 500,estim="classical") ) Use: estim=”modulus” for the robust estimator.
Is there a big difference between the two estimators?

### c) Try a different trend: How about trend= ~ altitude (This fits a linear regression to altitude as predictor and estimates the semivariogram from the residuals)
Use: plot(ca.reg3, trend= ~ altitude) # this plots the residuals plot(variog(ca.reg3,trend=~altitude, max.dist = 500,estim="classical") )
Next try the following:
trend = ="1st" (this fits a linear regression in the coordinates), or
trend = ="2nd" (this fits a quadratic regression in the coordinates). Interpret the graphs.
