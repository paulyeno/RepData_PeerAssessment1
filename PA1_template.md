Reproducible Research Assignment 1
==================================

The code needed to load the data is

```r
data<-read.csv("activity.csv")
```
The code needed to plot a histogram of the number of steps taken each day is

```r
steps.tot.day<-as.matrix(by(data[,1],data[,2],sum))
hist(steps.tot.day,breaks=15,xlab="Total Steps per Day",
        main="Histogram of Total Steps per Day")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

The code needed to calculate the mean and median of the total number of steps taken each day is

```r
steps.mean<-mean(steps.tot.day,na.rm=T)
steps.median<-median(steps.tot.day,na.rm=T)
```
This mean and median are

```r
steps.mean
```

```
## [1] 10766
```

```r
steps.median
```

```
## [1] 10765
```
The code needed to plot the average number of steps taken vs. each 5 minute interval is

```r
dirty<-is.na(data[,1])
cdata<-data[!dirty,]
steps.mean.int<-as.matrix(by(cdata[,1],cdata[,3],mean))
plot(unique(cdata[,3]),steps.mean.int,type="l",xlab="Time Interval",
        ylab="Average Number of Steps",
        main="Average Number of Steps per Time Interval")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 

The code needed to calculate the interval containing the maximum number of steps is

```r
steps.tot.int<-(by(cdata[,1],cdata[,3],sum))
steps.max.int<-unique(data[steps.tot.int%in%max(steps.tot.int),3])
```
The interval containing the maximum nuber of steps is

```r
steps.max.int
```

```
## [1] 835
```
The code needed to calculate the number of missing values is

```r
na.rows<-sum(dirty)
```
The number of missing values is

```r
na.rows
```

```
## [1] 2304
```
The code needed to replace the missing values with the mean of the same interval, rounded to the nearest whole number, is

```r
rdata<-data
for (i in 1:nrow(rdata)) {
        if(is.na(rdata[i,1])==TRUE){
                rdata[i,1]<-round(mean(subset(rdata,rdata[,3]==rdata[i,3])
                       [,1],na.rm=T))
        }
        next
}
```
The code needed to plot a histogram of the number of steps taken each day, when calculated with the data where NAs were replaced, is

```r
nona.steps.tot.day<-as.matrix(by(rdata[,1],rdata[,2],sum))
hist(nona.steps.tot.day,breaks=15,
        xlab="Total Steps per Day",
        main="Histogram of Total Steps per Day")
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11.png) 

The code needed to calculate the mean and median of the total number of steps taken each day, when calculated with the data where NAs were replaced, is

```r
nona.steps.mean<-mean(nona.steps.tot.day,na.rm=T)
nona.steps.median<-median(nona.steps.tot.day,na.rm=T)
```
This mean and median are

```r
nona.steps.mean
```

```
## [1] 10766
```

```r
nona.steps.median
```

```
## [1] 10762
```
These measurements differ in that changing the data to change the NAs into the means of other measurements taken in a given interval on other days is

```r
steps.mean-nona.steps.mean
```

```
## [1] 0.5493
```

```r
steps.median-nona.steps.median
```

```
## [1] 3
```
showing that the mean and median both drop when the the NAs are replaced with the means of hte given intervals as noted above.

The code needed to calculate whether a date is part of the weekday or weekend, and then assign it to a new column in the data, is

```r
for (i in 1:nrow(rdata)) {
        if(weekdays(as.Date(rdata[i,2]))=="Monday"|
                weekdays(as.Date(rdata[i,2]))=="Tuesday"|
                weekdays(as.Date(rdata[i,2]))=="Wednesday"|
                weekdays(as.Date(rdata[i,2]))=="Thursday"|
                weekdays(as.Date(rdata[i,2]))=="Friday") {
                rdata[i,4]<-"weekday"
        }
        else{
                rdata[i,4]<-"weekend"
        }
        next
}
```
The code needed to plot a histogram of the number of steps taken each day, when splitting the data into weekdays and weekends, is

```r
wdy<-subset(rdata,rdata[,4]=="weekday",select=steps&interval)[,c("steps",
        "interval")]
wnd<-subset(rdata,rdata[,4]=="weekend",select=steps&interval)[,c("steps",
        "interval")]
wdy.mean<-as.matrix(by(wdy[,1],wdy[,2],mean))
wnd.mean<-as.matrix(by(wnd[,1],wnd[,2],mean))
oldpar<-par
par(mfrow=c(2,1))
plot(unique(wdy[,2]),wdy.mean[,1],type="l",xlab="Time Interval",
        ylab="Average Number of Steps",
        main="Average Number of Steps \nper Time Interval on Weekdays")
plot(unique(wnd[,2]),wnd.mean[,1],type="l",xlab="Time Interval",
        ylab="Average Number of Steps",
        main="Average Number of Steps \nper Time Interval on Weekends")
```

![plot of chunk unnamed-chunk-16](figure/unnamed-chunk-16.png) 

```r
par<-oldpar
```
