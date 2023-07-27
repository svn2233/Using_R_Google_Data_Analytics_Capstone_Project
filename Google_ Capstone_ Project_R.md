Google Data Analytics Capstone Project Using R 

By: Steven Nguyen

# Case Study: How Does a Bike-Share Navigate Speedy Success? 

## Introduction

This case study is a part of the Google Data Analytics Professional certification. It focuses on Cyclist, a bikeshare company that records customer trips over a 12-month period (October 21 to September 22). The data is made available by Motivate International Inc under this license: https://ride.divvybikes.com/data-license-agreement. To conduct the analysis, I will follow the six steps of data analysis learned in this certification, which are: Ask, Prepare, Process, Analyze, Share, and Act (A.P.P.A.S.A).

## Scenario

I am a data analyst working in the marketing analyst team at Cyclistic, a bike-share company based in Chicago. The company's objective is to maximize the number of annual members. To achieve this goal, we need to gain a comprehensive understanding of how casual riders and annual members utilize the bike-sharing service differently. By analyzing their usage patterns and behaviors, we can identify potential strategies to encourage more customers to become annual members and thereby boost overall membership.

## 1. ASK

**Data Set Range**

I have chosen 12 datasets for this project from October 2021 to
September 2022. Each dataset shows the details of every ride of the
customers of Cyclists.

## Objective

The objective is to analyze how annual members and non-annual members use the service differently. Through in-depth analysis and visualization, we aim to identify key differences in their usage patterns and behaviors. Based on these insights, we will develop a targeted marketing strategy to maximize annual memberships. 

## Stakeholders

    [] Lily Moreno- Director Of Marketing

    [] Cyclistic Executive Team

## 2. PREPARE

R will be the primary tool used for cleaning, documenting, and preparing the data for analysis. Its efficiency makes it more suitable compared to other methods, as it significantly reduces time-consuming tasks. Additionally, R's versatility allows for not only data analysis but also data visualization and data wrangling. This comprehensive approach will enable me to gain deeper insights from the data and effectively present the findings to stakeholders.

### \[1\] Check Working Directory

``` r
getwd()
```

    ## [1] "/Users/stevennguyen/Documents/GitHub/Google-Capstone-R"

### \[2\] Install Packages:

    Install Required Packages and Libraries
    
    install.packages("tidyverse")
    install.packages("ggplot2")
    install.packages("lubridate")
    install.packages("dplyr")
    install.packages("readr")
    install.packages("janitor")
    install.packages("data.table")
    install.packages("tidyr")
    
    1.tidyverse is used for metapackage of all tidyverse packages
    2.lubridate is used for date functions
    3.ggplot2 is used for visualization
    4.scales is used for values in data frame 
    5.dplyr provides a uniform set of verbs, helping to resolve the most frequent data manipulation hurdles
    6.janitor is used to examine and clean dirty data
    7.readr will be used to read the csv files
    8.data.table is used for fast aggregation of large datasets and low latency add/update/remove of columns

### \[3\] Load Packages:

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggplot2)
library(lubridate)
library(dplyr)
library(readr)
library(janitor)
```

    ## 
    ## Attaching package: 'janitor'
    ## 
    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

``` r
library(data.table)
```

    ## 
    ## Attaching package: 'data.table'
    ## 
    ## The following objects are masked from 'package:lubridate':
    ## 
    ##     hour, isoweek, mday, minute, month, quarter, second, wday, week,
    ##     yday, year
    ## 
    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     transpose

``` r
library(tidyr)
library(knitr)
```

### \[4\]Load Datasets

    I will name each CSV according to their month and year for easier readability.

``` r
Oct21 <- read.csv("/Users/stevennguyen/project/202110-divvy-tripdata.csv")
Nov21<- read.csv("/Users/stevennguyen/project/202111-divvy-tripdata.csv")
Dec21<- read.csv("/Users/stevennguyen/project/202112-divvy-tripdata.csv")
Jan22<- read.csv("/Users/stevennguyen/project/202201-divvy-tripdata.csv")
Feb22<- read.csv("/Users/stevennguyen/project/202202-divvy-tripdata.csv")
Mar22<- read.csv("/Users/stevennguyen/project/202203-divvy-tripdata.csv")
Apr22 <- read.csv("/Users/stevennguyen/project/202204-divvy-tripdata.csv")
May22<- read.csv("/Users/stevennguyen/project/202205-divvy-tripdata.csv")
Jun22<- read.csv("/Users/stevennguyen/project/202206-divvy-tripdata.csv")
Jul22<- read.csv("/Users/stevennguyen/project/202207-divvy-tripdata.csv")
Aug22<- read.csv("/Users/stevennguyen/project/202208-divvy-tripdata.csv")
Sep22<- read.csv("/Users/stevennguyen/project/202209-divvy-tripdata.csv")
```

### \[5\] Check columns to see if all are consistent

    colnames(Oct21)
    colnames(Nov21)
    colnames(Dec21)
    colnames(Jan22)
    colnames(Feb22)
    colnames(Mar22)
    colnames(Apr22)
    colnames(May22)
    colnames(Jun22)
    colnames(Jul22)
    colnames(Aug22)
    colnames(Sep22)

## \[6\] Check data structures and data types for all data frames

``` r
str(Oct21)
```

    ## 'data.frame':    631226 obs. of  13 variables:
    ##  $ ride_id           : chr  "620BC6107255BF4C" "4471C70731AB2E45" "26CA69D43D15EE14" "362947F0437E1514" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2021-10-22 12:46:42" "2021-10-21 09:12:37" "2021-10-16 16:28:39" "2021-10-16 16:17:48" ...
    ##  $ ended_at          : chr  "2021-10-22 12:49:50" "2021-10-21 09:14:14" "2021-10-16 16:36:26" "2021-10-16 16:19:03" ...
    ##  $ start_station_name: chr  "Kingsbury St & Kinzie St" "" "" "" ...
    ##  $ start_station_id  : chr  "KA1503000043" "" "" "" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

``` r
str(Nov21)
```

    ## 'data.frame':    359978 obs. of  13 variables:
    ##  $ ride_id           : chr  "7C00A93E10556E47" "90854840DFD508BA" "0A7D10CDD144061C" "2F3BE33085BCFF02" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2021-11-27 13:27:38" "2021-11-27 13:38:25" "2021-11-26 22:03:34" "2021-11-27 09:56:49" ...
    ##  $ ended_at          : chr  "2021-11-27 13:46:38" "2021-11-27 13:56:10" "2021-11-26 22:05:56" "2021-11-27 10:01:50" ...
    ##  $ start_station_name: chr  "" "" "" "" ...
    ##  $ start_station_id  : chr  "" "" "" "" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 42 42 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.7 -87.7 -87.8 -87.6 ...
    ##  $ end_lat           : num  42 41.9 42 41.9 41.9 ...
    ##  $ end_lng           : num  -87.7 -87.7 -87.7 -87.8 -87.6 ...
    ##  $ member_casual     : chr  "casual" "casual" "casual" "casual" ...

``` r
str(Dec21)
```

    ## 'data.frame':    247540 obs. of  13 variables:
    ##  $ ride_id           : chr  "46F8167220E4431F" "73A77762838B32FD" "4CF42452054F59C5" "3278BA87BF698339" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2021-12-07 15:06:07" "2021-12-11 03:43:29" "2021-12-15 23:10:28" "2021-12-26 16:16:10" ...
    ##  $ ended_at          : chr  "2021-12-07 15:13:42" "2021-12-11 04:10:23" "2021-12-15 23:23:14" "2021-12-26 16:30:53" ...
    ##  $ start_station_name: chr  "Laflin St & Cullerton St" "LaSalle Dr & Huron St" "Halsted St & North Branch St" "Halsted St & North Branch St" ...
    ##  $ start_station_id  : chr  "13307" "KP1705001026" "KA1504000117" "KA1504000117" ...
    ##  $ end_station_name  : chr  "Morgan St & Polk St" "Clarendon Ave & Leland Ave" "Broadway & Barry Ave" "LaSalle Dr & Huron St" ...
    ##  $ end_station_id    : chr  "TA1307000130" "TA1307000119" "13137" "KP1705001026" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.6 -87.6 -87.6 -87.7 ...
    ##  $ end_lat           : num  41.9 42 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.7 -87.7 -87.6 -87.6 -87.6 ...
    ##  $ member_casual     : chr  "member" "casual" "member" "member" ...

``` r
str(Jan22)
```

    ## 'data.frame':    103770 obs. of  13 variables:
    ##  $ ride_id           : chr  "C2F7DD78E82EC875" "A6CF8980A652D272" "BD0F91DFF741C66D" "CBB80ED419105406" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2022-01-13 11:59:47" "2022-01-10 08:41:56" "2022-01-25 04:53:40" "2022-01-04 00:18:04" ...
    ##  $ ended_at          : chr  "2022-01-13 12:02:44" "2022-01-10 08:46:17" "2022-01-25 04:58:01" "2022-01-04 00:33:00" ...
    ##  $ start_station_name: chr  "Glenwood Ave & Touhy Ave" "Glenwood Ave & Touhy Ave" "Sheffield Ave & Fullerton Ave" "Clark St & Bryn Mawr Ave" ...
    ##  $ start_station_id  : chr  "525" "525" "TA1306000016" "KA1504000151" ...
    ##  $ end_station_name  : chr  "Clark St & Touhy Ave" "Clark St & Touhy Ave" "Greenview Ave & Fullerton Ave" "Paulina St & Montrose Ave" ...
    ##  $ end_station_id    : chr  "RP-007" "RP-007" "TA1307000001" "TA1309000021" ...
    ##  $ start_lat         : num  42 42 41.9 42 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.7 -87.7 -87.7 -87.6 ...
    ##  $ end_lat           : num  42 42 41.9 42 41.9 ...
    ##  $ end_lng           : num  -87.7 -87.7 -87.7 -87.7 -87.6 ...
    ##  $ member_casual     : chr  "casual" "casual" "member" "casual" ...

``` r
str(Feb22)
```

    ## 'data.frame':    115609 obs. of  13 variables:
    ##  $ ride_id           : chr  "E1E065E7ED285C02" "1602DCDC5B30FFE3" "BE7DD2AF4B55C4AF" "A1789BDF844412BE" ...
    ##  $ rideable_type     : chr  "classic_bike" "classic_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2022-02-19 18:08:41" "2022-02-20 17:41:30" "2022-02-25 18:55:56" "2022-02-14 11:57:03" ...
    ##  $ ended_at          : chr  "2022-02-19 18:23:56" "2022-02-20 17:45:56" "2022-02-25 19:09:34" "2022-02-14 12:04:00" ...
    ##  $ start_station_name: chr  "State St & Randolph St" "Halsted St & Wrightwood Ave" "State St & Randolph St" "Southport Ave & Waveland Ave" ...
    ##  $ start_station_id  : chr  "TA1305000029" "TA1309000061" "TA1305000029" "13235" ...
    ##  $ end_station_name  : chr  "Clark St & Lincoln Ave" "Southport Ave & Wrightwood Ave" "Canal St & Adams St" "Broadway & Sheridan Rd" ...
    ##  $ end_station_id    : chr  "13179" "TA1307000113" "13011" "13323" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.6 -87.6 -87.7 -87.6 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 42 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.7 -87.6 -87.6 -87.6 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

``` r
str(Mar22)
```

    ## 'data.frame':    284042 obs. of  13 variables:
    ##  $ ride_id           : chr  "47EC0A7F82E65D52" "8494861979B0F477" "EFE527AF80B66109" "9F446FD9DEE3F389" ...
    ##  $ rideable_type     : chr  "classic_bike" "electric_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2022-03-21 13:45:01" "2022-03-16 09:37:16" "2022-03-23 19:52:02" "2022-03-01 19:12:26" ...
    ##  $ ended_at          : chr  "2022-03-21 13:51:18" "2022-03-16 09:43:34" "2022-03-23 19:54:48" "2022-03-01 19:22:14" ...
    ##  $ start_station_name: chr  "Wabash Ave & Wacker Pl" "Michigan Ave & Oak St" "Broadway & Berwyn Ave" "Wabash Ave & Wacker Pl" ...
    ##  $ start_station_id  : chr  "TA1307000131" "13042" "13109" "TA1307000131" ...
    ##  $ end_station_name  : chr  "Kingsbury St & Kinzie St" "Orleans St & Chestnut St (NEXT Apts)" "Broadway & Ridge Ave" "Franklin St & Jackson Blvd" ...
    ##  $ end_station_id    : chr  "KA1503000043" "620" "15578" "TA1305000025" ...
    ##  $ start_lat         : num  41.9 41.9 42 41.9 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.6 -87.7 -87.6 -87.6 ...
    ##  $ end_lat           : num  41.9 41.9 42 41.9 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.6 -87.7 -87.6 -87.7 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

``` r
str(Apr22)
```

    ## 'data.frame':    371249 obs. of  13 variables:
    ##  $ ride_id           : chr  "3564070EEFD12711" "0B820C7FCF22F489" "89EEEE32293F07FF" "84D4751AEB31888D" ...
    ##  $ rideable_type     : chr  "electric_bike" "classic_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2022-04-06 17:42:48" "2022-04-24 19:23:07" "2022-04-20 19:29:08" "2022-04-22 21:14:06" ...
    ##  $ ended_at          : chr  "2022-04-06 17:54:36" "2022-04-24 19:43:17" "2022-04-20 19:35:16" "2022-04-22 21:23:29" ...
    ##  $ start_station_name: chr  "Paulina St & Howard St" "Wentworth Ave & Cermak Rd" "Halsted St & Polk St" "Wentworth Ave & Cermak Rd" ...
    ##  $ start_station_id  : chr  "515" "13075" "TA1307000121" "13075" ...
    ##  $ end_station_name  : chr  "University Library (NU)" "Green St & Madison St" "Green St & Madison St" "Delano Ct & Roosevelt Rd" ...
    ##  $ end_station_id    : chr  "605" "TA1307000120" "TA1307000120" "KA1706005007" ...
    ##  $ start_lat         : num  42 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.6 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num  42.1 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.7 -87.6 -87.6 -87.6 -87.6 ...
    ##  $ member_casual     : chr  "member" "member" "member" "casual" ...

``` r
str(May22)
```

    ## 'data.frame':    634858 obs. of  13 variables:
    ##  $ ride_id           : chr  "EC2DE40644C6B0F4" "1C31AD03897EE385" "1542FBEC830415CF" "6FF59852924528F8" ...
    ##  $ rideable_type     : chr  "classic_bike" "classic_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2022-05-23 23:06:58" "2022-05-11 08:53:28" "2022-05-26 18:36:28" "2022-05-10 07:30:07" ...
    ##  $ ended_at          : chr  "2022-05-23 23:40:19" "2022-05-11 09:31:22" "2022-05-26 18:58:18" "2022-05-10 07:38:49" ...
    ##  $ start_station_name: chr  "Wabash Ave & Grand Ave" "DuSable Lake Shore Dr & Monroe St" "Clinton St & Madison St" "Clinton St & Madison St" ...
    ##  $ start_station_id  : chr  "TA1307000117" "13300" "TA1305000032" "TA1305000032" ...
    ##  $ end_station_name  : chr  "Halsted St & Roscoe St" "Field Blvd & South Water St" "Wood St & Milwaukee Ave" "Clark St & Randolph St" ...
    ##  $ end_station_id    : chr  "TA1309000025" "15534" "13221" "TA1305000030" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.6 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.6 -87.7 -87.6 -87.7 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

``` r
str(Jun22)
```

    ## 'data.frame':    769204 obs. of  13 variables:
    ##  $ ride_id           : chr  "600CFD130D0FD2A4" "F5E6B5C1682C6464" "B6EB6D27BAD771D2" "C9C320375DE1D5C6" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2022-06-30 17:27:53" "2022-06-30 18:39:52" "2022-06-30 11:49:25" "2022-06-30 11:15:25" ...
    ##  $ ended_at          : chr  "2022-06-30 17:35:15" "2022-06-30 18:47:28" "2022-06-30 12:02:54" "2022-06-30 11:19:43" ...
    ##  $ start_station_name: chr  "" "" "" "" ...
    ##  $ start_station_id  : chr  "" "" "" "" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.8 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.6 -87.7 -87.7 -87.6 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.8 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.6 -87.6 -87.7 -87.6 ...
    ##  $ member_casual     : chr  "casual" "casual" "casual" "casual" ...

``` r
str(Jul22)
```

    ## 'data.frame':    823488 obs. of  13 variables:
    ##  $ ride_id           : chr  "954144C2F67B1932" "292E027607D218B6" "57765852588AD6E0" "B5B6BE44314590E6" ...
    ##  $ rideable_type     : chr  "classic_bike" "classic_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : chr  "2022-07-05 08:12:47" "2022-07-26 12:53:38" "2022-07-03 13:58:49" "2022-07-31 17:44:21" ...
    ##  $ ended_at          : chr  "2022-07-05 08:24:32" "2022-07-26 12:55:31" "2022-07-03 14:06:32" "2022-07-31 18:42:50" ...
    ##  $ start_station_name: chr  "Ashland Ave & Blackhawk St" "Buckingham Fountain (Temp)" "Buckingham Fountain (Temp)" "Buckingham Fountain (Temp)" ...
    ##  $ start_station_id  : chr  "13224" "15541" "15541" "15541" ...
    ##  $ end_station_name  : chr  "Kingsbury St & Kinzie St" "Michigan Ave & 8th St" "Michigan Ave & 8th St" "Woodlawn Ave & 55th St" ...
    ##  $ end_station_id    : chr  "KA1503000043" "623" "623" "TA1307000164" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.6 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.8 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.6 -87.6 -87.6 -87.7 ...
    ##  $ member_casual     : chr  "member" "casual" "casual" "casual" ...

``` r
str(Aug22)
```

    ## 'data.frame':    785932 obs. of  13 variables:
    ##  $ ride_id           : chr  "550CF7EFEAE0C618" "DAD198F405F9C5F5" "E6F2BC47B65CB7FD" "F597830181C2E13C" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2022-08-07 21:34:15" "2022-08-08 14:39:21" "2022-08-08 15:29:50" "2022-08-08 02:43:50" ...
    ##  $ ended_at          : chr  "2022-08-07 21:41:46" "2022-08-08 14:53:23" "2022-08-08 15:40:34" "2022-08-08 02:58:53" ...
    ##  $ start_station_name: chr  "" "" "" "" ...
    ##  $ start_station_id  : chr  "" "" "" "" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 42 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.6 -87.7 -87.7 -87.7 ...
    ##  $ end_lat           : num  41.9 41.9 42 42 41.8 ...
    ##  $ end_lng           : num  -87.7 -87.6 -87.7 -87.7 -87.7 ...
    ##  $ member_casual     : chr  "casual" "casual" "casual" "casual" ...

``` r
str(Sep22)
```

    ## 'data.frame':    701339 obs. of  13 variables:
    ##  $ ride_id           : chr  "5156990AC19CA285" "E12D4A16BF51C274" "A02B53CD7DB72DD7" "C82E05FEE872DF11" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2022-09-01 08:36:22" "2022-09-01 17:11:29" "2022-09-01 17:15:50" "2022-09-01 09:00:28" ...
    ##  $ ended_at          : chr  "2022-09-01 08:39:05" "2022-09-01 17:14:45" "2022-09-01 17:16:12" "2022-09-01 09:10:32" ...
    ##  $ start_station_name: chr  "" "" "" "" ...
    ##  $ start_station_id  : chr  "" "" "" "" ...
    ##  $ end_station_name  : chr  "California Ave & Milwaukee Ave" "" "" "" ...
    ##  $ end_station_id    : chr  "13084" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.6 -87.6 -87.7 -87.7 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.7 -87.6 -87.6 -87.7 -87.7 ...
    ##  $ member_casual     : chr  "casual" "casual" "casual" "casual" ...

## 3.PROCESS

### \[7\] Now Data is ready to process. I will now bind all the datasets into a single data frame

``` r
all_trips <- bind_rows(Oct21,Nov21,Dec21,Jan22,Feb22,Mar22,Apr22,May22,Jun22,Jul22,Aug22,Sep22)
```

### \[8\] Now that the datasets are a single data frame we will check the data structure and data types.

``` r
str(all_trips)
```

    ## 'data.frame':    5828235 obs. of  13 variables:
    ##  $ ride_id           : chr  "620BC6107255BF4C" "4471C70731AB2E45" "26CA69D43D15EE14" "362947F0437E1514" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2021-10-22 12:46:42" "2021-10-21 09:12:37" "2021-10-16 16:28:39" "2021-10-16 16:17:48" ...
    ##  $ ended_at          : chr  "2021-10-22 12:49:50" "2021-10-21 09:14:14" "2021-10-16 16:36:26" "2021-10-16 16:19:03" ...
    ##  $ start_station_name: chr  "Kingsbury St & Kinzie St" "" "" "" ...
    ##  $ start_station_id  : chr  "KA1503000043" "" "" "" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

### \[9\] Analyzing the data I find that “started_at” and “ended_at” are are actually in “char.” As “started_at” and ended_at should be in data time datatype.

``` r
all_trips[['started_at']] <- ymd_hms(all_trips[['started_at']])
all_trips[['ended_at']] <- ymd_hms(all_trips[['ended_at']])
```

### \[10\] Now we recheck data structure to ensure that the changes have been made

``` r
str(all_trips)
```

    ## 'data.frame':    5828235 obs. of  13 variables:
    ##  $ ride_id           : chr  "620BC6107255BF4C" "4471C70731AB2E45" "26CA69D43D15EE14" "362947F0437E1514" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct, format: "2021-10-22 12:46:42" "2021-10-21 09:12:37" ...
    ##  $ ended_at          : POSIXct, format: "2021-10-22 12:49:50" "2021-10-21 09:14:14" ...
    ##  $ start_station_name: chr  "Kingsbury St & Kinzie St" "" "" "" ...
    ##  $ start_station_id  : chr  "KA1503000043" "" "" "" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.6 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.6 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

### \[11\] Now to remove empty columns that are not required.

``` r
all_trips <- all_trips %>%
select(-c(start_lat:end_lng))
```

### \[12\] Now I will use glimpse() function to review the changes.

``` r
glimpse(all_trips)
```

    ## Rows: 5,828,235
    ## Columns: 9
    ## $ ride_id            <chr> "620BC6107255BF4C", "4471C70731AB2E45", "26CA69D43D…
    ## $ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", …
    ## $ started_at         <dttm> 2021-10-22 12:46:42, 2021-10-21 09:12:37, 2021-10-…
    ## $ ended_at           <dttm> 2021-10-22 12:49:50, 2021-10-21 09:14:14, 2021-10-…
    ## $ start_station_name <chr> "Kingsbury St & Kinzie St", "", "", "", "", "", "",…
    ## $ start_station_id   <chr> "KA1503000043", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_name   <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_id     <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ member_casual      <chr> "member", "member", "member", "member", "member", "…

### \[13\] It’s time to rename columns for better readability.

``` r
all_trips <- all_trips %>%
    rename(ride_type = rideable_type, 
           start_time = started_at,
           end_time = ended_at,
           customer_type = member_casual)
```

### \[14\] Now I will use the function glimpse() to review changes.

``` r
glimpse(all_trips)
```

    ## Rows: 5,828,235
    ## Columns: 9
    ## $ ride_id            <chr> "620BC6107255BF4C", "4471C70731AB2E45", "26CA69D43D…
    ## $ ride_type          <chr> "electric_bike", "electric_bike", "electric_bike", …
    ## $ start_time         <dttm> 2021-10-22 12:46:42, 2021-10-21 09:12:37, 2021-10-…
    ## $ end_time           <dttm> 2021-10-22 12:49:50, 2021-10-21 09:14:14, 2021-10-…
    ## $ start_station_name <chr> "Kingsbury St & Kinzie St", "", "", "", "", "", "",…
    ## $ start_station_id   <chr> "KA1503000043", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_name   <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_id     <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ customer_type      <chr> "member", "member", "member", "member", "member", "…

### \[15\] Now it is time to add new columns that can be used for aggregate functions.

#### Column for day of the week the trip started

``` r
all_trips$day_of_the_week <- format(as.Date(all_trips$start_time),'%a')
```

#### Column for month when trip started

``` r
all_trips$month <- format(as.Date(all_trips$start_time),'%b_%y')
```

#### A column for time and day for the trip started will be created. To do this, time element needs to be extracted from start_time. BUT times has to be in POSIXct. Since only times of class POSIXct are supported in ggplot2. There needs to be a 2 step conversion. First the time needs to be converted to a character vector which strips all the date information. Then the time is converted back into POSIXct with today’s date. BUT this has no use for us as only the hours and minutes are.

``` r
all_trips$time <- format(all_trips$start_time, format = "%H:%M")
all_trips$time <- as.POSIXct(all_trips$time, format = "%H:%M")
```

#### Column for trip duration in min.

``` r
all_trips$trip_duration <- (as.double(difftime(all_trips$end_time, all_trips$start_time)))/60
```

### \[16\] Now I use the glimpse() function to review the changes.

``` r
glimpse(all_trips)
```

    ## Rows: 5,828,235
    ## Columns: 13
    ## $ ride_id            <chr> "620BC6107255BF4C", "4471C70731AB2E45", "26CA69D43D…
    ## $ ride_type          <chr> "electric_bike", "electric_bike", "electric_bike", …
    ## $ start_time         <dttm> 2021-10-22 12:46:42, 2021-10-21 09:12:37, 2021-10-…
    ## $ end_time           <dttm> 2021-10-22 12:49:50, 2021-10-21 09:14:14, 2021-10-…
    ## $ start_station_name <chr> "Kingsbury St & Kinzie St", "", "", "", "", "", "",…
    ## $ start_station_id   <chr> "KA1503000043", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_name   <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_id     <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ customer_type      <chr> "member", "member", "member", "member", "member", "…
    ## $ day_of_the_week    <chr> "Fri", "Thu", "Sat", "Sat", "Wed", "Thu", "Thu", "W…
    ## $ month              <chr> "Oct_21", "Oct_21", "Oct_21", "Oct_21", "Oct_21", "…
    ## $ time               <dttm> 2023-07-12 12:46:00, 2023-07-12 09:12:00, 2023-07-…
    ## $ trip_duration      <dbl> 3.133333, 1.616667, 7.783333, 1.250000, 8.266667, 1…

### \[17\] Now it is time to check if there is any negative values in the column trip_duration as this may create issues with when we make visualization. And we don’t need any trips that were apart of a quality test.

#### Checking for trip lengths that are less than 0

``` r
nrow(subset(all_trips,trip_duration < 0))
```

    ## [1] 108

#### Checking test rides that were made by company for quality checks

``` r
nrow(subset(all_trips, start_station_name %like% "TEST"))
```

    ## [1] 0

``` r
nrow(subset(all_trips, start_station_name %like% "test"))
```

    ## [1] 0

``` r
nrow(subset(all_trips, start_station_name %like% "Test"))
```

    ## [1] 1

#### As there are 8845 rows with trip_duration less than 0 mins and 3220 trips that were test rides, we will remove these observations from our dataframe as they contribute to only about 0.3% of the total rows.

### \[18\] We will create a new dataframe without of these observations without making any changes to the existing dataframe.

#### Remove negative trip duration

``` r
all_trips_v2 <- all_trips[!(all_trips$trip_duration < 0),]
```

#### Remove Test Rides

``` r
all_trips_v2<- all_trips_v2[!((all_trips_v2$start_station_name %like% "TEST" | all_trips_v2$start_station_name %like% "Test")),]
```

#### Check Data Frame

``` r
glimpse(all_trips_v2)
```

    ## Rows: 5,828,126
    ## Columns: 13
    ## $ ride_id            <chr> "620BC6107255BF4C", "4471C70731AB2E45", "26CA69D43D…
    ## $ ride_type          <chr> "electric_bike", "electric_bike", "electric_bike", …
    ## $ start_time         <dttm> 2021-10-22 12:46:42, 2021-10-21 09:12:37, 2021-10-…
    ## $ end_time           <dttm> 2021-10-22 12:49:50, 2021-10-21 09:14:14, 2021-10-…
    ## $ start_station_name <chr> "Kingsbury St & Kinzie St", "", "", "", "", "", "",…
    ## $ start_station_id   <chr> "KA1503000043", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_name   <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ end_station_id     <chr> "", "", "", "", "", "", "", "", "", "", "", "", "",…
    ## $ customer_type      <chr> "member", "member", "member", "member", "member", "…
    ## $ day_of_the_week    <chr> "Fri", "Thu", "Sat", "Sat", "Wed", "Thu", "Thu", "W…
    ## $ month              <chr> "Oct_21", "Oct_21", "Oct_21", "Oct_21", "Oct_21", "…
    ## $ time               <dttm> 2023-07-12 12:46:00, 2023-07-12 09:12:00, 2023-07-…
    ## $ trip_duration      <dbl> 3.133333, 1.616667, 7.783333, 1.250000, 8.266667, 1…

### \[19\] Now it is time to make sure that customer_type has only two distinct values.

#### Checking the count of distinct values

``` r
table(all_trips_v2$customer_type)
```

    ## 
    ##  casual  member 
    ## 2401225 3426901

#### Aggregating total trip duration by customer type

``` r
setNames(aggregate(trip_duration ~ customer_type, all_trips_v2, sum), c("customer_type", "total_trip_duration(mins)"))
```

    ##   customer_type total_trip_duration(mins)
    ## 1        casual                  70501793
    ## 2        member                  43756816

## 4. Analyze

    Now the dataframe is ready for descriptive analysis. 

### \[20\] We will now look at the statistical summary of trip_duration for all trips

``` r
summary(all_trips_v2$trip_duration)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ##     0.00     5.93    10.48    19.60    18.85 40705.02

### \[21\] We will now look at the statistical summary of trip_duration by customer_type

``` r
all_trips_v2 %>%
    group_by(customer_type) %>%
    summarise(min_trip_duration = min(trip_duration),max_trip_duration = max(trip_duration),
              median_trip_duration = median(trip_duration), mean_trip_duration = mean(trip_duration))
```

    ## # A tibble: 2 × 5
    ##   customer_type min_trip_duration max_trip_duration median_trip_duration
    ##   <chr>                     <dbl>             <dbl>                <dbl>
    ## 1 casual                        0            40705.                13.4 
    ## 2 member                        0             1560.                 8.88
    ## # ℹ 1 more variable: mean_trip_duration <dbl>

#### The mean trip duration of member riders is lower than the mean trip duration of all trips, it is exactly the opposite for casual riders, whose mean trip duration is higher than the the mean trip duration of all trips. This tells us that casual riders usually take the bikes out for a longer duration compared to members.

### \[22\] Now to find the total number of trips by customer type and day of the week.

We need to fix the order for the day_of_the_week and month variable so
they will show up in the same sequence in output tables and
visualizations

``` r
all_trips_v2$day_of_the_week <- ordered(all_trips_v2$day_of_the_week, levels=c("Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"))
all_trips_v2$month <- ordered(all_trips_v2$month, levels=c("Oct_21", "Nov_21", "Dec_21", "Jan_22", "Feb_22", "Mar_22", "Apr_22", "May_22", "Jun_22", "Jul_22", "Aug_22", "Sep_22"))
 all_trips_v2 %>% 
  group_by(customer_type, day_of_the_week) %>%  
  summarise(number_of_rides = n(),average_duration_mins = mean(trip_duration)) %>% 
  arrange(customer_type, desc(number_of_rides))
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 14 × 4
    ## # Groups:   customer_type [2]
    ##    customer_type day_of_the_week number_of_rides average_duration_mins
    ##    <chr>         <ord>                     <int>                 <dbl>
    ##  1 casual        Sat                      499801                  32.7
    ##  2 casual        Sun                      405009                  34.4
    ##  3 casual        Fri                      352507                  28.0
    ##  4 casual        Thu                      306691                  25.7
    ##  5 casual        Wed                      281660                  25.0
    ##  6 casual        Mon                      279785                  29.7
    ##  7 casual        Tue                      275772                  25.8
    ##  8 member        Tue                      541519                  12.2
    ##  9 member        Wed                      538488                  12.1
    ## 10 member        Thu                      530547                  12.3
    ## 11 member        Fri                      491465                  12.5
    ## 12 member        Mon                      473059                  12.3
    ## 13 member        Sat                      458222                  14.3
    ## 14 member        Sun                      393601                  14.2

## 5.Share

    Time to use R to Visualize our data:

### \[23\] We will now visualize Total trips by customer type by the day of the week.

``` r
all_trips_v2 %>%
  group_by(customer_type, day_of_the_week) %>% 
  summarise(number_of_rides = n()) %>% 
  arrange(customer_type, day_of_the_week)  %>% 
  ggplot(aes(x = day_of_the_week, y = number_of_rides, fill = customer_type)) +
  labs(title ="Total trips by customer type Vs. Day of the week") +
  geom_col(width=0.5, position = position_dodge(width=0.5)) +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

![](/unnamed-chunk-29-1.png)<!-- -->

    [] We can see that more members ride during the weekday than casuals 
    [] Casuals ride more than members on the weekday. 
    [] Members ride the most during midweek and casuals towards the end of the week

### \[24\] Now to find the average trips by customer type and month

``` r
unique(all_trips$month)
```

    ##  [1] "Oct_21" "Nov_21" "Dec_21" "Jan_22" "Feb_22" "Mar_22" "Apr_22" "May_22"
    ##  [9] "Jun_22" "Jul_22" "Aug_22" "Sep_22"

``` r
all_trips_v2 %>% 
  group_by(customer_type, month) %>%  
  summarise(number_of_rides = n(),`average_duration_(mins)` = mean(trip_duration)) %>% 
arrange(customer_type,desc(number_of_rides))
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 24 × 4
    ## # Groups:   customer_type [2]
    ##    customer_type month  number_of_rides `average_duration_(mins)`
    ##    <chr>         <ord>            <int>                     <dbl>
    ##  1 casual        Jul_22          406046                      29.3
    ##  2 casual        Jun_22          369044                      32.1
    ##  3 casual        Aug_22          358917                      29.3
    ##  4 casual        Sep_22          296694                      28.0
    ##  5 casual        May_22          280414                      30.9
    ##  6 casual        Oct_21          257242                      28.7
    ##  7 casual        Apr_22          126417                      29.5
    ##  8 casual        Nov_21          106898                      23.1
    ##  9 casual        Mar_22           89880                      32.6
    ## 10 casual        Dec_21           69738                      23.5
    ## # ℹ 14 more rows

### \[25\] We will now visualize Total trips by customer type by the month.

``` r
all_trips_v2 %>%  
  group_by(customer_type, month) %>% 
  summarise(number_of_rides = n()) %>% 
  arrange(customer_type, month)  %>% 
  ggplot(aes(x = month, y = number_of_rides, fill = customer_type)) +
  labs(title ="Total trips by customer type Vs. Month") +
  theme(axis.text.x = element_text(angle = 30)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

![](/unnamed-chunk-31-1.png)<!-- -->

    [] During the winter months both members and casuals number of rides is less than summer and fall months. 
    [] Summer months are the most busiest as both customer types ride the most. 
    [] Throughout the year members ride more than casuals
    [] Members ride more every month than casuals.

### \[26\] We will now visualize average trip duration by customer type on each day of the week.

``` r
all_trips_v2 %>%  
  group_by(customer_type, day_of_the_week) %>% 
  summarise(average_trip_duration = mean(trip_duration)) %>%
  ggplot(aes(x = day_of_the_week, y = average_trip_duration, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Average trip duration by customer type Vs. Day of the week")
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

![](/unnamed-chunk-32-1.png)<!-- -->

    [] Casuals take longer trips than members 
    [] Casuals take the longest trips on the weekend 
    [] Members during the weekday trip duration is very constant and increases during the weekend
    [] Casuals trip duration starts high on Monday, with not much change from Tuesday-Wednesday and than increasing from Friday to Sunday 
    [] Casuals throughout the week take twice as long as members on trips duration 

### \[27\] Now we will visual average trip duration by customer type during each month.

``` r
all_trips_v2 %>%
group_by(customer_type, month) %>% 
summarise(average_trip_duration = mean(trip_duration)) %>%
ggplot(aes(x = month, y = average_trip_duration, fill = customer_type)) +
geom_col(width=0.5, position = position_dodge(width=0.5)) + 
labs(title ="Average trip duration by customer type Vs. Month") +
theme(axis.text.x = element_text(angle = 30))
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

![](/unnamed-chunk-33-1.png)<!-- -->

    [] Casuals take twice as long on trips as members throughout the year. 
    [] Members take usually take more time in the summer and fall on their trips 

### \[28\] Now we will visualize the demand of bikes over a 24 hour period.

``` all_trips_v2
  group_by(customer_type, time) %>% 
  summarise(number_of_trips = n()) %>%
  ggplot(aes(x = time, y = number_of_trips, color = customer_type, group = customer_type)) +
  geom_line() +
  scale_x_datetime(date_breaks = "1 hour", minor_breaks = NULL,
                   date_labels = "%H:%M", expand = c(0,0)) +
  theme(axis.text.x = element_text(angle = 90)) +
  labs(title ="Demand over 24 hours of a day", x = "Time of the day")
```

    [] Peak time for trips for both is between 5-6pm and then # of trips decreases after every hour
    [] # of trips are low from 12-5 am and 10-11 pm 
    [] # of trips start increasing after 5am
    [] From 7 to 9 am there is a sharp increases in members trip duration 

### \[29\] Now we will visual ride type VS the number of trips by each type of customer

``` r
all_trips_v2 %>%
  group_by(ride_type, customer_type) %>%
  summarise(number_of_trips = n()) %>%  
  ggplot(aes(x= ride_type, y=number_of_trips, fill= customer_type))+
              geom_bar(stat='identity') +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE)) +
  labs(title ="Ride type Vs. Number of trips")
```

    ## `summarise()` has grouped output by 'ride_type'. You can override using the
    ## `.groups` argument.

![](/unnamed-chunk-34-1.png)<!-- -->

    [] No members used docked bikes.
    [] Members took classic and electric bikes more than casuals 

### \[30\] Now we will create a csv file for further analysis or visualization.

``` r
write.csv(all_trips, "google_capstone_R.csv")
```

## 6. Act

### Key takeaways

* Casual customers use the bike share services more frequently from Friday to Sunday, with higher usage on weekends compared to weekdays. On the other hand, members use the services more consistently throughout the entire week.
* Casual riders tend to prefer docked bikes over members, as no members use docked bikes. However, members show a preference for classic bikes and electric bikes compared to casual riders.
* Among the bike options, casual riders prefer electric bikes the most, followed by classic bikes and then docked bikes.
* Members, on the other hand, prefer classic bikes over electric bikes.

### Recommendations

* Implement a "happy hour" format where discounted rates are offered during non-busy hours, especially on weekends. This strategy aims to increase ridership during slower periods, thereby boosting overall ridership.
* Introduce a reward-based system for memberships, such as offering a free ride for every 10 rides taken within a month. Additionally, provide special discounts exclusively for members. These initiatives will incentivize existing members to remain loyal and attract new customers to sign up for memberships.
* Offer a one-time discount for casual customers to encourage them to sign up for a new membership. This promotional offer can act as an incentive for casual riders to become members.
* Introduce a referral system where individuals receive a certain number of free rides for every successful referral. This referral program will not only increase membership sign-ups but also encourage existing members to refer friends and family.
