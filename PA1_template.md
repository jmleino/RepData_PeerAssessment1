# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

Load the data and convert the date column into date data type.


```r
Sys.setlocale("LC_TIME", "C")
```

```
## [1] "C"
```

```r
unzip("activity.zip")
data  <- read.csv("activity.csv")
data$date <- as.Date(data$date)
```




## What is mean total number of steps taken per day?

Calculate the total daily steps.

```r
daydata <- aggregate(steps ~ date, data, FUN = sum)
```

Draw a histogram of the daily step amounts.

```r
hist(daydata$steps, xlab = "Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

Calculate the mean and median number of steps.

```r
mean(daydata$steps)
```

```
## [1] 10766.19
```

```r
median(daydata$steps)
```

```
## [1] 10765
```



## What is the average daily activity pattern?

Calculate the mean number of steps in an interval.

```r
intervaldata <- aggregate(steps ~ interval, data, FUN = mean)
```

Create a time series plot.

```r
plot(intervaldata$interval, intervaldata$steps, type = "l", xlab = "Interval", ylab = "Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png) 


Find the interval containing maximum average number of steps.

```r
intervaldata[ max( intervaldata$steps ) == intervaldata$steps,]
```

```
##     interval    steps
## 104      835 206.1698
```


## Imputing missing values

Missing values is the TRUE column.

```r
table( is.na( data$steps ) )
```

```
## 
## FALSE  TRUE 
## 15264  2304
```

Replace the missing values with the mean for that interval.


## Are there differences in activity patterns between weekdays and weekends?


Create a factor separating weekend from weekdays.

```r
data$day[ weekdays( data$date ) %in% c("Saturday", "Sunday")] <- "weekend"
data$day[ is.na( data$day ) ] <- "weekday"
data$day  <- as.factor(data$day)
```


