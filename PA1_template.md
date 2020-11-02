
---
title: "Reproducible Research Project 1"
author: "MAribel"
date: "30/10/2020"
output:
  html_document: default
  pdf_document: default
keep_md: TRUE
---

```
library(ggplot2)


```

## Loading and preprocessing the data
Code for reading in the dataset and/or processing the data

```

activity<-read.csv("activity.csv")

```


Histogram of the total number of steps taken each day
```


StepsPerDay <- tapply(activity$steps, activity$date, sum)


hist(StepsPerDay, xlab = "Number of Steps", main = "Histogram of  steps per day", breaks = 10, col = "purple", border = "red" )
```



## What is mean total number of steps taken per day?

Mean and median number of steps taken each day


```
mean(StepsPerDay, na.rm = TRUE)
median(StepsPerDay, na.rm = TRUE)


```




## What is the average daily activity pattern?

Time series plot of the average number of steps taken

```
Steps_PerInterval <- tapply(activity$steps, activity$interval, mean, na.rm = TRUE)

plot(as.numeric(names(Steps_PerInterval)), 
     Steps_PerInterval, xlab = "interval", ylab = "Steps",  main = "Average Daily Activity Pattern",  type = "l", col="red", lwd=2)







```





## The 5-minute interval that, on average, contains the maximum number of steps



```
Interval_max <- names(sort(Steps_PerInterval, decreasing = TRUE)[1])
Steps_max <- sort(Steps_PerInterval, decreasing = TRUE)[1]
```


## Imputing missing values
Code to describe and show a strategy for imputing missing data

```
nrow(activity[is.na(activity$steps),])


```

Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated



```


Steps_PerInterval <- tapply(activity$steps, activity$interval, mean, na.rm = TRUE)

activitysplit <- split(activity, activity$interval)

for(i in 1:length(activitysplit)){
    activitysplit[[i]]$steps[is.na(activitysplit[[i]]$steps)] <- Steps_PerInterval[i]
}
activityimputed <- do.call("rbind", activitysplit)
activityimputed <- activityimputed[order(activityimputed$date) ,]
```



##   Histogram of the total number of steps taken each day after missing values are imputed

```
StepsPerDayimputed <- tapply(activityimputed$steps, activityimputed$date, sum)
hist(StepsPerDayimputed, xlab = "Steps", main = "Steps per Day", col = "aquamarine1", border = "pink")

```

```
mean(StepsPerDayimputed, na.rm = TRUE)
median(StepsPerDayimputed, na.rm = TRUE)


```


## Are there differences in activity patterns between weekdays and weekends?


```
library(chron)
names(activity)
meanIntervaWeekends <-
  aggregate(steps ~ interval, activity[is.weekend(activity$date),], mean)


meanIntervaWeekdays <-
  aggregate(steps ~ interval, activity[!is.weekend(activity$date),], mean)
par(mfrow=c(1,2))
plot(
  steps ~ interval,meanIntervaWeekends,type = "l",main = "Average steps on weekends",
  col="red"
)
plot(
  steps ~ interval,meanIntervaWeekdays,type = "l",main = "Average steps on weekdays",
  col="red"
)

```










