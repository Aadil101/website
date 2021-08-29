---
layout: single
title: Viscosity Data Analysis
date: 20 October 2017
author_profile: true
share: false
categories: coding
show_date: true
---

This is a homework assignment from Math 50 at Dartmouth College.

## Question-1 (Sample)

-   Read Example 3.1 Delivery Time Data.

### Part (a)

-   Graphics can be very useful in analyzing the data. Plot two useful visualization of the data. First plot three dimensional scatterplot of delivery time data. Then plot scatterplot matrix (which is an array of 2D plots where each plot is a scatter diagram between two variables).

``` r
# Loading the data
delivery <- read.table("https://math.dartmouth.edu/~m50f17/delivery.csv", header = TRUE)
plot(delivery)
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
x1Cases <- delivery$Cases
x2Distance <- delivery$Distance
yTime <- delivery$Time

# 3D scatter diagram  
library("plot3D")
library("scatterplot3d")
sc1 <- scatterplot3d(x1Cases, x2Distance, yTime, pch=17 , type = 'p', angle = 15 , highlight.3d = T ) # Plot scatterplot matrix
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-1-2.png)<!-- -->

``` r
plot(delivery[,-1])
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-1-3.png)<!-- -->

2.  Fit a regression model for the reduced model relating delivery time to number of cases. Plot the joint confidence region of the coefficients (slope and intercept). Also add a point to the plot to show the estimated slope and intercept.

``` r
library(ellipse)
```

    ## Attaching package: 'ellipse'

    ## The following object is masked from 'package:graphics':
    ## 
    ##     pairs

``` r
reducedFit <- lm(Time ~ x1Cases, data = delivery)
plot(ellipse(reducedFit), type = "l", xlab = "Intercept", ylab = "Slope", main = "Joint Confidence Region")
points (reducedFit$coeff[[1]] , reducedFit$coeff[[2]] )
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

3.  Calculate the extra sum of squares due to the regressor variable Distance.

``` r
fullFit <- lm(Time ~ Cases + Distance, data = delivery)
reducedSSR <- sum((predict(reducedFit) - mean(yTime))^2)
fullSSR <- sum((predict(fullFit) - mean(yTime))^2)
```

Extra sum of squares due to distance: 168.4021256

## Question-2

-   Load the kinematic viscosity data (explained in Problem 3.14 and table B-10 in the book) at <https://math.dartmouth.edu/~m50f17/kinematic.csv>  
-   Solve the parts (a) to (e) of the Problem 3.14 and use *α* = 0.05. In addition, do the following.

### Part (a)

``` r
kv_data<-read.table("https://math.dartmouth.edu/~m50f17/kinematic.csv", header=T, sep=",")
y = kv_data$y
x1 = kv_data$x1
x2 = kv_data$x2
regression_x1x2 = lm(y ~ x1 + x2, data = kv_data)
```

*β*<sub>0</sub>: 0.679439

*β*<sub>1</sub>: 1.4073309

*β*<sub>2</sub>: -0.0156288

### Part (b)

``` r
n = length(x1)
k = 2
SSr = sum((predict(regression_x1x2) - mean(y))^2)
SSres = sum((y - predict(regression_x1x2))^2)
f_stat = (SSr/k)/(SSres/(n-(k+1)))
f_val = qf(0.95, k,n-(k+1))
```

*F*<sub>stat</sub>: 85.461685

*F*<sub>val</sub>: 3.2519238

Reject null hyp, conclude at 5% level that at least one of temperature and ratio of solvents linearly predicts kinematic viscosity.

### Part (c)

``` r
c = rep(1,length(x1))
x = cbind(c,x1,x2)
save = t(x) %*% x
cjj = solve((t(x) %*% x))
t_stat_1 = abs(regression_x1x2$coeff[[2]]/(SSres/(n-(k+1))*cjj[2,2])^0.5)
t_stat_2 = abs(regression_x1x2$coeff[[3]]/(SSres/(n-(k+1))*cjj[3,3])^0.5)
t_val = qt(0.975,n-(k+1))
```

*β*<sub>1</sub> *t*<sub>stat</sub>: 7.146521

*t*\_value: 2.0261925

*t*<sub>val</sub>: -0.0156288

Reject null hyp, conclude at 5% level that ratio of solvents alone linearly predicts kinematic viscosity.

*β*<sub>2</sub> *t*<sub>stat</sub>: 10.9476302

*t*<sub>value</sub>: 2.0261925

*t*<sub>val</sub>: -0.0156288

Reject null hyp, conclude at 5% level that temperature alone linearly predicts kinematic viscosity.

### Part (d)

``` r
SSt = sum((y - mean(y))^2)
r_sq = SSr / SSt
r_sq_adj = 1 - (SSres / (n-(k+1)))/(SSt / (n-1))
```

*R*<sub>sq multiple</sub>: 0.8220498

*R*<sub>sq adj multiple</sub>: 0.8124309

``` r
regression_x2 = lm(y ~ x2, data = kv_data)
k = 1
SSr_temp = sum((predict(regression_x2) - mean(y))^2)
SSres_temp = sum((y - predict(regression_x2))^2)
r_sq_temp = SSr_temp / SSt
r_sq_adj_temp = 1 - (SSres_temp / (n-(k+1)))/(SSt / (n-1))
```

*R*<sub>sq simple</sub>: 0.5764172

*R*<sub>sq adj simple</sub>: 0.5652703

The multiple regression model clearly fits the data better than the simple regression model does when comparing their respective *R*<sub>sq</sub> and *R*<sub>sq adj</sub> values, indicating the ratio of the solvents matters to the prediction of viscosity.

### Part (e)

``` r
Sxx = sum((x2-mean(x2))^2)
lower_simple = regression_x2$coeff[[2]] - qt(0.995,n-(k+1))*(SSres_temp/(n-(k+1))/Sxx)^0.5
upper_simple = regression_x2$coeff[[2]] + qt(0.995,n-(k+1))*(SSres_temp/(n-(k+1))/Sxx)^0.5
k = 2
lower_multiple = regression_x1x2$coeff[[3]] - qt(0.995,n-(k+1))*(SSres/(n-(k+1))*cjj[3,3])^0.5
upper_multiple = regression_x1x2$coeff[[3]] + qt(0.995,n-(k+1))*(SSres/(n-(k+1))*cjj[3,3])^0.5
```

*temp*<sub>coeff conf int simple</sub>: (-0.0215221, -0.0097356)

*temp*<sub>coeff conf int multiple</sub>: (-0.0195054, -0.0117523)

The confidence interval for temperature coefficient in the multiple linear regression model is slightly narrower and therefore more concise than the interval from the simple linear regression model.

### Part (f)

-   Calculate the extra sum of squares due to the regressor variable x<sub>1</sub>.

``` r
SSr = sum((predict(regression_x1x2) - mean(y))^2)
SSr_b2 = sum((predict(regression_x2)-mean(y))^2)
SSr_b1b2 = SSr-SSr_b2
```

extra sum squares x<sub>1</sub>: 3.4349231

### Part (g)

-   Plot scatterplot matrix and scatter diagram in order to visualize the data. Can you make any connection between the visualization of data and the results you found in previous parts? Discuss.

``` r
library("plot3D")
library("scatterplot3d")
sc1 <- scatterplot3d(x1, x2, y, pch=17 , type = 'p', angle = 15 , highlight.3d = T)
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
plot(kv_data[,-1])
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

``` r
plot(kv_data)
```

![](https://raw.githubusercontent.com/Aadil101/viscosity-data-analysis/master/README_files/figure-gfm/unnamed-chunk-11-3.png)<!-- -->

Clearly there exists a linear relationship between viscosity and temperature/ratio of solvents, however the relationship does not appear to be line-based. The model likely needs an interaction term because the 3d plot is not planar (there is curvature, non-constant slope of the graph). Thus the multiple linear regression model may be a better fit than the simple linear regression model as we found before, but the former still may not be the best fit.

## Question-3

-   Load the Mortality data (explained in Problem 3.15 and table B-15 in the book) at
-   <https://math.dartmouth.edu/~m50f17/mortality.csv>
-   Solve the parts (a) to (e) of the Problem 3.15 (use *α* = 0.05 if you need). In addition do the following.

### Part (a)

``` r
mort_data<-read.table("https://math.dartmouth.edu/~m50f17/mortality.csv",header=T)
y = mort_data$MORT
x1 = mort_data$PRECIP
x2 = mort_data$EDUC
x3 = mort_data$NONWHITE
x4 = mort_data$NOX
x5 = mort_data$SO2
regression = lm(y ~ x1+x2+x3+x4+x5, data = mort_data)
```

*β*<sub>0</sub>: 995.636455,

*β*<sub>1</sub>: 1.4073404

*β*<sub>2</sub>: -14.8013907

*β*<sub>3</sub>: 3.1990908

*β*<sub>4</sub>: -0.1079698

*β*<sub>5</sub>: 0.3551762

### Part (b)

``` r
n = length(x1)
k = 5
SSr = sum((predict(regression)-mean(y))^2)
SSres = sum((y-predict(regression))^2)
f_stat = (SSr/k)/(SSres/(n-(k+1)))
f_val = qf(0.95,k,n-(k+1))
```

*F*<sub>stat</sub>: 22.3862387

*F*<sub>val</sub>: 2.3860699

Reject null hyp, conclude at 5% level that at least one of precip, educ, nonwhite, NOX, and SO2 predicts mortality.

### Part (c)

``` r
x = cbind(c,x1,x2,x3,x4,x5)
```

    ## Warning in cbind(c, x1, x2, x3, x4, x5): number of rows of result is not a
    ## multiple of vector length (arg 1)

``` r
cjj = solve((t(x) %*% x))
t_stat_b1 = abs(regression$coeff[[2]]/(SSres/(n-(k+1)) * cjj[2,2])^0.5)
t_stat_b2 = abs(regression$coeff[[3]]/(SSres/(n-(k+1)) * cjj[3,3])^0.5)
t_stat_b3 = abs(regression$coeff[[4]]/(SSres/(n-(k+1)) * cjj[4,4])^0.5)
t_stat_b4 = abs(regression$coeff[[5]]/(SSres/(n-(k+1)) * cjj[5,5])^0.5)
t_stat_b5 = abs(regression$coeff[[6]]/(SSres/(n-(k+1)) * cjj[6,6])^0.5)
t_val = qt(0.975,n-(k+1))
```

*t*<sub>stat</sub>: 2.0421598, *t*<sub>value</sub>: 2.0048793, reject null hyp

*t*<sub>stat</sub>: 2.1062201, *t*<sub>value</sub>: 2.0048793, reject null hyp

*t*<sub>stat</sub>: 5.1406827, *t*<sub>value</sub>: 2.0048793, reject null hyp

*t*<sub>stat</sub>: 0.7996351, *t*<sub>value</sub>: 2.0048793, fail to reject null hyp

*t*<sub>stat</sub>: 3.9047402, *t*<sub>value</sub>: 2.0048793, reject null hyp

Conclude at 5% level that Precip, Educ, Nonwhite, and SO2 predict Mortality. Conclude at 5% level that NOX does not predict Mortality at the 5% level.

### Part (d)

``` r
SSt = sum((y-mean(y))^2)
r_sq = SSr/SSt
r_sq_adj = 1 - ((SSres)/(n-(k+1)))/((SSt)/(n-1))
```

*R*<sup>2</sup>: 0.6745639, *R*<sub>*a**d**j*</sub><sup>2</sup>:
0.6444309

### Part (e)

``` r
upper = regression$coeff[[6]] + qt(0.975,n-(k+1))*(SSres/(n-(k+1)) * cjj[6,6])^0.5
lower = regression$coeff[[6]] - qt(0.975,n-(k+1))*(SSres/(n-(k+1)) * cjj[6,6])^0.5
```

SO2\_coeff\_conf\_int: (0.1728118, 0.5375405)

### Part (f)

-   You want to quantify the contribution of regressors Educ, NOX, SO2 together to the model. Choose *α* = 0.01. Using F-test (the partial F-test given in equation 3.35) comment on this contribution to the model. (Note the different *α* value).

``` r
reg_reduced = lm(y~x1+x3, data = mort_data)
SSr_full = sum((predict(regression)-mean(y))^2)
SSr_reduced = sum((predict(reg_reduced)-mean(y))^2)
f_stat = ((SSr_full - SSr_reduced)/3)/(SSres/(n-(k+1)))
k = 5
r = 3
f_val = qf(0.99,3,n-(k+1))
```

*F*<sub>stat</sub>: 10.0446028, *F*<sub>val</sub>: 4.1665007, Reject null hyp, conclude at 1% level that at least one of education, NOX, and SO4 linearly predict mortality, given all other regressors are already in the model.

### Part (g)

-   Consider the individual contribution test you calculated in part (c). Now choose the two regressor variables with the lowest t-statistic values. Using partial F-test comment on their contribution to the model. Use *α* = 0.01.

``` r
k = 5
reg_reduced = lm(y~x2+x3+x5, data = mort_data)
SSr_reduced = sum((predict(reg_reduced)-mean(y))^2)
f_stat = ((SSr_full - SSr_reduced)/2)/(SSres/(n-(k+1)))
r = 2
f_val = qf(0.99,r,n-(k+1))
```

We use x<sub>1</sub> and x<sub>4</sub>.

*F*<sub>stat</sub>: 3.7138214, *F*<sub>val</sub>: 5.0212173, Fail to reject null hyp, conclude
that together precip and NOX do not linearly predict mortality, given
all other regressors are already in the model.
