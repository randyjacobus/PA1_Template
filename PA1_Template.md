PA1Template
========================================================

This is the first Peer Assesment for Reproducible Data. In this analysis we will be studying the number of steps taken by an individual in October and November of 2012

Load needed libraries

```r
library(plyr)
```

Load the step data

```r
setwd("/Users/rolandjacobus/Desktop/Coursera/Coursera - Reproducible Data/PA1")
steps<-read.csv("activity-4.csv")
```

Process the Data by changing dates to date class and removing rows with NAs

```r
Steps<-data.frame(steps$steps,as.Date(steps$date),steps$interval)
names(Steps)<-c("steps","date","interval")
cleanSteps<-Steps[complete.cases(Steps),]
names(cleanSteps)<-c("steps","date","interval")
```

Sum the total number of steps taken each day and make a histogram

```r
stepsperday<-ddply(cleanSteps,c("date"),summarize, dailysteps=sum(steps))
hist(stepsperday$dailysteps)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

Calculate and report the mean and median total number of steps taken each day

```r
mean(stepsperday$dailysteps)
```

```
## [1] 10766
```

```r
median(stepsperday$dailysteps)
```

```
## [1] 10765
```

Make a time series x = 5 minute and y= average across all days

```r
intervalmean<-aggregate(steps~interval,cleanSteps,mean)
plot(intervalmean$interval,intervalmean$steps,type="l",xlab="Interval", ylab="average steps")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 

Which 5 minute interval on average contains the maximum steps

```r
max(intervalmean$steps)
```

```
## [1] 206.2
```

Caclulate and report the total the total number of rows with NAs

```r
nrow(Steps)-nrow(cleanSteps)
```

```
## [1] 2304
```

Replace NAs with mean the mean steps and create a new dataset ...a very simple replacement process.

```r
NewSteps<-Steps
NewSteps[is.na(NewSteps)]<- 37.38
```

Make a histogram of the total number of steps taken each day.

```r
newstepsperday<-ddply(NewSteps,c("date"),summarize, dailysteps=sum(steps))
hist(newstepsperday$dailysteps)
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 

Calculate and report the mean and median total steps taken each day

```r
mean(newstepsperday$dailysteps)
```

```
## [1] 10766
```

```r
median(newstepsperday$dailysteps)
```

```
## [1] 10765
```

Comment on the difference of between the results in the data with and without the NAs. What is the impact of adding the missing data.

There is a higher frequency of days with more steps.

Add a new column to the data table indicating if the date is a weekday or weekend (weekdays() function)

```r
NewSteps$day<-weekdays(as.Date(NewSteps$date))
```

Make a panel plot comparing the the avg steps taken in each 5 minute interval on the weekdays and on the weekends.

```r
avgSteps<-ddply(NewSteps,c("day","interval"),summarize, mean(NewSteps$steps))
plot(avgSteps$interval,avgSteps$steps,type="l",xlab="Interval", ylab="average steps")
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13.png) 
