downscalingPortal
=================

downscalingPortal

# Here you can find some examples of the analysis of the downscaled data.
# The loadDownscaling.R function allows you to read .csv files produced in the portal.

source('/the/directory/where/you/have/the/script/loadDownscaling.R')
downscaling <- loadDownscaling('yourDownscaling.csv')
# display the structure
str(downscaling)

# this data frame contains the dates in the first column and the downscaled values for different stations in the following ones.
precip<-downscaling[[2]]
dates<-downscaling[[1]]
# plot a histogram
hist(precip)
# compute basic statistics
mean(precip,na.rm=TRUE)
sd(precip)
quantile(precip, c(.9,.95,.99))

# load this package, it is useful for time series
install.packages("zoo")
require(zoo)

# plot the original series
series <- zoo(downscaling[[2]], downscaling[[1]])
plot(series)

# plot the monthly mean aggregated data
seriesmmean <- aggregate(series, as.Date(as.yearmon(time(series))), mean)
plot(seriesmmean)

# plot the yearly mean aggregated data
as.year <- function(x){
  as.Date(cut(x, 'year'))
}
seriesymean <- aggregate(series, as.Date(as.year(time(series))), mean)
plot(seriesymean)

# plot the monthly 95th percentile aggregated data
p95<- function(x){
  quantile(x,.95)
}
seriesmmean95p <- aggregate(series, as.Date(as.yearmon(time(series))), p95)
plot(seriesmmean95p)

# plot the yearly 95th percentile aggregated data
seriesymean95p <- aggregate(series, as.Date(as.year(time(series))), p95)
plot(seriesymean95p)
