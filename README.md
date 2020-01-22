---
title: "gisData"
author: "Michael Kahle"
date: "22 1 2020"
output:
  html_document: 
    keep_md: true
  pdf_document: default
---



GISS Surface Temperature Analysis (GISTEMP v4)

https://data.giss.nasa.gov/gistemp/

The GISS Surface Temperature Analysis (GISTEMP v4) is an estimate of global surface temperature change. 

GISTEMP Team, 2020: GISS Surface Temperature Analysis (GISTEMP), version 4. NASA Goddard Institute for Space Studies. Dataset accessed 20YY-MM-DD at https://data.giss.nasa.gov/gistemp/.

Lenssen, N., G. Schmidt, J. Hansen, M. Menne, A. Persin, R. Ruedy, and D. Zyss, 2019: Improvements in the GISTEMP uncertainty model. J. Geophys. Res. Atmos., 124, no. 12, 6307-6326, doi:10.1029/2018JD029522.




```r
require("ggplot2")
```

```
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 3.5.3
```

```r
require("extrafont")
```

```
## Loading required package: extrafont
```

```
## Warning: package 'extrafont' was built under R version 3.5.2
```

```
## Registering fonts with R
```

## Load and Visialize Monthly Global Temperature Data 



```r
tempGlobal <- read.csv("https://data.giss.nasa.gov/gistemp/tabledata_v4/GLB.Ts+dSST.csv", sep=",", na = "NA", skip = 1)

tempNorth <- read.csv("https://data.giss.nasa.gov/gistemp/tabledata_v4/NH.Ts+dSST.csv", sep=",", na = "NA", skip = 1)

tempSouth <- read.csv("https://data.giss.nasa.gov/gistemp/tabledata_v4/SH.Ts+dSST.csv", sep=",", na = "NA", skip = 1)

tempZones <- read.csv("https://data.giss.nasa.gov/gistemp/tabledata_v4/ZonAnn.Ts+dSST.csv", sep=",", na = "NA", skip = 1)

currMonth <- 1
tG <- tempGlobal[,c('Year', 'Jan')]
names(tG)[names(tG) == "Jan"] <- "temperature"
tG$month = currMonth

for (month in c("Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec")) {
  currMonth <- currMonth+1
  temp <- tempGlobal[,c('Year', month)]
  names(temp)[names(temp) == month] <- "temperature"
  temp$month = currMonth
  tG <- rbind(tG, temp)
}
names(tG)[names(tG) == 'Year'] <- 'year'
tG$ts <- signif(tG$year + (tG$month-0.5)/12, digits=6)
tG$time <- paste(tG$year,tG$month, '15 00:00:00', sep='-')

write.table(tG, file = "csv/monthly_temperature_global.csv", append = FALSE, quote = TRUE, sep = ",",
            eol = "\n", na = "NA", dec = ".", row.names = FALSE,
            col.names = TRUE, qmethod = "escape", fileEncoding = "UTF-8")
```
