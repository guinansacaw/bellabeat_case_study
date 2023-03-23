Bellabeat Case Study
================
William Guiñansaca
2023-03-02

![](images/bellabeat_v2.png)

# Bellabeat

## About the company

Bellabeat is a high-tech manufacturer of health-focused products for
women. The aim is to unlock new growth opportunities for the company by
focusing on one of Bellabeat’s products and analyzing smart device data
to gain insights into how consumers are using their devices.

## Characters and products

- Characters
  - Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer
  - Sando Mur: Mathematician and Bellabeat’s cofounder; key member of
    the Bellabeat executive team
  - Bellabeat marketing analytics team
- Products
  - Bellabeat app: The Bellabeat app provides users with health data
    related to their activity, sleep, stress, menstrual cycle, and
    mindfulness habits. The Bellabeat app connects to their line of
    smart wellness products.
  - Leaf: Bellabeat’s classic wellness tracker can be worn as a
    bracelet, necklace, or clip. The Leaf tracker connects to the
    Bellabeat app to track activity, sleep, and stress.
  - Time: This wellness watch combines the timeless look of a classic
    timepiece with smart technology to track user activity, sleep, and
    stress. The Time watch connects to the Bellabeat app to provide you
    with insights into your daily wellness.
  - Spring: This is a water bottle that tracks daily water intake using
    smart technology to ensure that you are appropriately hydrated
    throughout the day. The Spring bottle connects to the Bellabeat app
    to track your hydration levels.
  - Bellabeat membership: Bellabeat also offers a subscription-based
    membership program for users.Membership gives users 24/7 access to
    fully personalized guidance on nutrition, activity, sleep, health
    and beauty, and mindfulness based on their lifestyle and goals.

# Ask

This phase clearly defines the goal of this analysis, which is to focus
on a Bellabeat product and analyze smart device usage data to gain
insights into how people are already using their smart devices.

# Prepare

The data used for this analysis is from FitBit Fitness Tracker Data.
This Kaggle data set contains personal fitness tracker data from thirty
Fitbit users. Thirty eligible Fitbit users consented to the submission
of personal tracker data, including minute-level output for physical
activity, heart rate, and sleep monitoring. It includes information
about daily activity, steps, and heart rate that can be used to explore
users’ habits.

The data source: [FitBit Fitness Tracker
Data](https://www.kaggle.com/datasets/arashnic/fitbit)

Dataset made available through:
[Mobius](https://www.kaggle.com/arashnic)

## Identify how data is organized.

The data contains files at different levels of granularity, such as
days, hours, and minutes. But each of these files contains the same
variables: calories, intensities, and steps. Additionally, there are
three more files that show the user’s heart rate, sleep duration, and
weight log.

- The calories file contains information about the number of calories
  burned by the user on a specific date.
- The intensities file contains information about physical activity in
  terms of distance and minutes spent in four different intensity
  levels: sedentary, lightly active, fairly active, and very active.
- The steps file contains information about the number of steps taken by
  a user in a day.
- The heart rate file contains information about the user’s heart rate.
- The sleep file shows the number of minutes a user sleeps in a day.
- The weight log file contains information about the user’s weight.

**The files containing the data related to daily steps, intensity, and
calories have already been merged into one file called “daily activity”.
Therefore, the analysis will use only the daily activity file.**

## Determine the credibility of the data.

The analysis uses public data; therefore, considering this fact the data
is:

- Reliability : LOW – dataset was collected from 33 individuals whose
  gender is unknown.
- Originality : LOW – third party data collect using Amazon Mechanical
  Turk.
- Comprehensive : MEDIUM – dataset contains multiple fields on daily
  activity intensity, calories, daily steps taken, daily sleep time and
  weight record.
- Current : MEDIUM – data is 7 years old
- Cited : HIGH – data collector and source is well documented

# Process

The selected tool for this analysis is RStudio. Prior to conducting the
analysis, the data will be cleaned, organized, and transformed.

## Load libraries and data.

``` r
# Load-packages
library("tidyverse")
library("modeest")
library("cowplot")
```

``` r
# Import data
d_activity <- read_csv("raw_data/dailyActivity_merged_v1.csv")
heart_rate <- read_csv("raw_data/heartrate_seconds_merged.csv")
d_sleep <- read_csv("raw_data/sleepDay_merged.csv")
weight <- read_csv("raw_data/weightLogInfo_merged.csv")
```

## Knowing the Data

``` r
# To know the structure
str(d_activity)
```

    ## spc_tbl_ [940 × 16] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id                      : num [1:940] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ ActivityDate            : chr [1:940] "4/12/2016" "4/13/2016" "4/14/2016" "4/15/2016" ...
    ##  $ TotalSteps              : num [1:940] 13162 10735 10460 9762 12669 ...
    ##  $ TotalDistance           : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
    ##  $ TrackerDistance         : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
    ##  $ LoggedActivitiesDistance: num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveDistance      : num [1:940] 1.88 1.57 2.44 2.14 2.71 ...
    ##  $ ModeratelyActiveDistance: num [1:940] 0.55 0.69 0.4 1.26 0.41 ...
    ##  $ LightActiveDistance     : num [1:940] 6.06 4.71 3.91 2.83 5.04 ...
    ##  $ SedentaryActiveDistance : num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveMinutes       : num [1:940] 25 21 30 29 36 38 42 50 28 19 ...
    ##  $ FairlyActiveMinutes     : num [1:940] 13 19 11 34 10 20 16 31 12 8 ...
    ##  $ LightlyActiveMinutes    : num [1:940] 328 217 181 209 221 164 233 264 205 211 ...
    ##  $ SedentaryMinutes        : num [1:940] 728 776 1218 726 773 ...
    ##  $ Calories                : num [1:940] 1985 1797 1776 1745 1863 ...
    ##  $ ActivityDate_v1         : num [1:940] 42472 42473 42474 42475 42476 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   ActivityDate = col_character(),
    ##   ..   TotalSteps = col_double(),
    ##   ..   TotalDistance = col_double(),
    ##   ..   TrackerDistance = col_double(),
    ##   ..   LoggedActivitiesDistance = col_double(),
    ##   ..   VeryActiveDistance = col_double(),
    ##   ..   ModeratelyActiveDistance = col_double(),
    ##   ..   LightActiveDistance = col_double(),
    ##   ..   SedentaryActiveDistance = col_double(),
    ##   ..   VeryActiveMinutes = col_double(),
    ##   ..   FairlyActiveMinutes = col_double(),
    ##   ..   LightlyActiveMinutes = col_double(),
    ##   ..   SedentaryMinutes = col_double(),
    ##   ..   Calories = col_double(),
    ##   ..   ActivityDate_v1 = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(heart_rate)
```

    ## spc_tbl_ [2,483,658 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id   : num [1:2483658] 2.02e+09 2.02e+09 2.02e+09 2.02e+09 2.02e+09 ...
    ##  $ Time : chr [1:2483658] "4/12/2016 7:21:00 AM" "4/12/2016 7:21:05 AM" "4/12/2016 7:21:10 AM" "4/12/2016 7:21:20 AM" ...
    ##  $ Value: num [1:2483658] 97 102 105 103 101 95 91 93 94 93 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   Time = col_character(),
    ##   ..   Value = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(d_sleep)
```

    ## spc_tbl_ [413 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id                : num [1:413] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ SleepDay          : chr [1:413] "4/12/2016 12:00:00 AM" "4/13/2016 12:00:00 AM" "4/15/2016 12:00:00 AM" "4/16/2016 12:00:00 AM" ...
    ##  $ TotalSleepRecords : num [1:413] 1 2 1 2 1 1 1 1 1 1 ...
    ##  $ TotalMinutesAsleep: num [1:413] 327 384 412 340 700 304 360 325 361 430 ...
    ##  $ TotalTimeInBed    : num [1:413] 346 407 442 367 712 320 377 364 384 449 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   SleepDay = col_character(),
    ##   ..   TotalSleepRecords = col_double(),
    ##   ..   TotalMinutesAsleep = col_double(),
    ##   ..   TotalTimeInBed = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(weight)
```

    ## spc_tbl_ [67 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id            : num [1:67] 1.50e+09 1.50e+09 1.93e+09 2.87e+09 2.87e+09 ...
    ##  $ Date          : chr [1:67] "5/2/2016 11:59:59 PM" "5/3/2016 11:59:59 PM" "4/13/2016 1:08:52 AM" "4/21/2016 11:59:59 PM" ...
    ##  $ WeightKg      : num [1:67] 52.6 52.6 133.5 56.7 57.3 ...
    ##  $ WeightPounds  : num [1:67] 116 116 294 125 126 ...
    ##  $ Fat           : num [1:67] 22 NA NA NA NA 25 NA NA NA NA ...
    ##  $ BMI           : num [1:67] 22.6 22.6 47.5 21.5 21.7 ...
    ##  $ IsManualReport: logi [1:67] TRUE TRUE FALSE TRUE TRUE TRUE ...
    ##  $ LogId         : num [1:67] 1.46e+12 1.46e+12 1.46e+12 1.46e+12 1.46e+12 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   Date = col_character(),
    ##   ..   WeightKg = col_double(),
    ##   ..   WeightPounds = col_double(),
    ##   ..   Fat = col_double(),
    ##   ..   BMI = col_double(),
    ##   ..   IsManualReport = col_logical(),
    ##   ..   LogId = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

## Cleaning Data

It can be observed that there are some problems with the “date” column
in every dataframe, so a data type change is required. Additionally,
columns with wrong dates will be deleted.

``` r
d_activity <- d_activity %>% # Add a date column to daily activities
  mutate(date = as.POSIXct(ActivityDate, format = "%m/%d/%Y")) %>% 
  select(-ActivityDate, -ActivityDate_v1)

d_sleep <- d_sleep %>% # Add a date column to daily sleep
  mutate(date = as.POSIXct(SleepDay, format = "%m/%d/%Y %I:%M:%S %p")) %>% 
  select(-SleepDay)

heart_rate <- heart_rate %>% # Add a date column to heart rate
  mutate(date = as.POSIXct(Time, format = "%m/%d/%Y %I:%M:%S %p")) %>% 
  select(-Time)

weight <- weight %>% # Add a date column to weight
  mutate(date = as.POSIXct(Date, format = "%m/%d/%Y %I:%M:%S %p")) %>% 
  select(-Date)
```

The column names were in upper case, so they were transformed to lower
case.

``` r
# Rename the column names to lowercase
d_activity <- rename_with(d_activity, tolower)
d_sleep <- rename_with(d_sleep, tolower)
heart_rate <- rename_with(heart_rate, tolower)
weight <- rename_with(weight, tolower)
```

The data frames are checked for any missing values.

``` r
# Checking Missing data
sum(is.na(d_activity))
```

    ## [1] 0

``` r
sum(is.na(d_sleep))
```

    ## [1] 0

``` r
sum(is.na(heart_rate))
```

    ## [1] 0

``` r
sum(is.na(weight))
```

    ## [1] 65

``` r
# Cheking for white spaces
sum(grepl("^\\s*$", d_activity))
```

    ## [1] 0

``` r
sum(grepl("^\\s*$", d_sleep))
```

    ## [1] 0

``` r
sum(grepl("^\\s*$", heart_rate))
```

    ## [1] 0

``` r
sum(grepl("^\\s*$", weight))
```

    ## [1] 0

There are 65 null values in the weight file, which correspond to the
“fat” column, and there are no white spaces. It is necessary to
determine how many id’s there are in every file.

``` r
# How many unique IDs are there?
length(unique(d_activity$id))
```

    ## [1] 33

``` r
length(unique(d_sleep$id))
```

    ## [1] 24

``` r
length(unique(heart_rate$id))
```

    ## [1] 14

``` r
length(unique(weight$id))
```

    ## [1] 8

Activity file has 33 ID numbers, and there are no negative or missing
values in the data frames. However, files such as sleep, heart rate, and
weight do not have the same number of IDs. The heart rate and weight
files only contain information about sixteen and eight IDs,
respectively, which is not enough to draw any conclusions. There are no
categorical variables. All date columns are in the correct data
type.Therefore, the weight and heart rate files won’t be used for the
analysis.

## Transform the Data

A column with the days was added to the activity file.

``` r
# Add a column with days from the date in the activity file 

d_activity <- d_activity %>% 
  mutate(day_of_week = weekdays(date))
```

A column with the days was added to the sleep file.

``` r
# Add a column with days from the date in the sleep file 

d_sleep <- d_sleep %>% 
  mutate(day_of_week = weekdays(date))
```

A categorized column is added to determine the quality of sleep.

``` r
d_sleep <- d_sleep %>% 
  mutate(type = case_when(
    totalminutesasleep < 300 ~ "unhealthy sleep",
    totalminutesasleep >= 300 & totalminutesasleep <= 420 ~ "average sleep",
    totalminutesasleep > 420 ~ "healthy sleep"
  ))
```

A categorized column is added to determine daily steps.

``` r
d_activity <- d_activity %>% 
  mutate(type = case_when(
    totalsteps < 5000 ~ "Sedentary",
    totalsteps >= 5000 & totalsteps < 8000 ~ "Fairly Active",
    totalsteps >= 8000 & totalsteps < 12000 ~ "Active",
    totalsteps > 12000 ~ "Highly Active"
  ))
```

# Analyze

## Descriptive analysis

Let’s get a general overview of the data and identify any patterns or
trends in the data frames.

``` r
# To know the data
summary(d_activity)
```

    ##        id              totalsteps    totaldistance    trackerdistance 
    ##  Min.   :1.504e+09   Min.   :    0   Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.:2.320e+09   1st Qu.: 3790   1st Qu.: 2.620   1st Qu.: 2.620  
    ##  Median :4.445e+09   Median : 7406   Median : 5.245   Median : 5.245  
    ##  Mean   :4.855e+09   Mean   : 7638   Mean   : 5.490   Mean   : 5.475  
    ##  3rd Qu.:6.962e+09   3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.: 7.710  
    ##  Max.   :8.878e+09   Max.   :36019   Max.   :28.030   Max.   :28.030  
    ##  loggedactivitiesdistance veryactivedistance moderatelyactivedistance
    ##  Min.   :0.0000           Min.   : 0.000     Min.   :0.0000          
    ##  1st Qu.:0.0000           1st Qu.: 0.000     1st Qu.:0.0000          
    ##  Median :0.0000           Median : 0.210     Median :0.2400          
    ##  Mean   :0.1082           Mean   : 1.503     Mean   :0.5675          
    ##  3rd Qu.:0.0000           3rd Qu.: 2.053     3rd Qu.:0.8000          
    ##  Max.   :4.9421           Max.   :21.920     Max.   :6.4800          
    ##  lightactivedistance sedentaryactivedistance veryactiveminutes
    ##  Min.   : 0.000      Min.   :0.000000        Min.   :  0.00   
    ##  1st Qu.: 1.945      1st Qu.:0.000000        1st Qu.:  0.00   
    ##  Median : 3.365      Median :0.000000        Median :  4.00   
    ##  Mean   : 3.341      Mean   :0.001606        Mean   : 21.16   
    ##  3rd Qu.: 4.782      3rd Qu.:0.000000        3rd Qu.: 32.00   
    ##  Max.   :10.710      Max.   :0.110000        Max.   :210.00   
    ##  fairlyactiveminutes lightlyactiveminutes sedentaryminutes    calories   
    ##  Min.   :  0.00      Min.   :  0.0        Min.   :   0.0   Min.   :   0  
    ##  1st Qu.:  0.00      1st Qu.:127.0        1st Qu.: 729.8   1st Qu.:1828  
    ##  Median :  6.00      Median :199.0        Median :1057.5   Median :2134  
    ##  Mean   : 13.56      Mean   :192.8        Mean   : 991.2   Mean   :2304  
    ##  3rd Qu.: 19.00      3rd Qu.:264.0        3rd Qu.:1229.5   3rd Qu.:2793  
    ##  Max.   :143.00      Max.   :518.0        Max.   :1440.0   Max.   :4900  
    ##       date                        day_of_week            type          
    ##  Min.   :2016-04-12 00:00:00.00   Length:940         Length:940        
    ##  1st Qu.:2016-04-19 00:00:00.00   Class :character   Class :character  
    ##  Median :2016-04-26 00:00:00.00   Mode  :character   Mode  :character  
    ##  Mean   :2016-04-26 06:53:37.01                                        
    ##  3rd Qu.:2016-05-04 00:00:00.00                                        
    ##  Max.   :2016-05-12 00:00:00.00

``` r
summary(d_sleep)
```

    ##        id            totalsleeprecords totalminutesasleep totaltimeinbed 
    ##  Min.   :1.504e+09   Min.   :1.000     Min.   : 58.0      Min.   : 61.0  
    ##  1st Qu.:3.977e+09   1st Qu.:1.000     1st Qu.:361.0      1st Qu.:403.0  
    ##  Median :4.703e+09   Median :1.000     Median :433.0      Median :463.0  
    ##  Mean   :5.001e+09   Mean   :1.119     Mean   :419.5      Mean   :458.6  
    ##  3rd Qu.:6.962e+09   3rd Qu.:1.000     3rd Qu.:490.0      3rd Qu.:526.0  
    ##  Max.   :8.792e+09   Max.   :3.000     Max.   :796.0      Max.   :961.0  
    ##       date                        day_of_week            type          
    ##  Min.   :2016-04-12 00:00:00.00   Length:413         Length:413        
    ##  1st Qu.:2016-04-19 00:00:00.00   Class :character   Class :character  
    ##  Median :2016-04-27 00:00:00.00   Mode  :character   Mode  :character  
    ##  Mean   :2016-04-26 12:40:05.80                                        
    ##  3rd Qu.:2016-05-04 00:00:00.00                                        
    ##  Max.   :2016-05-12 00:00:00.00

## Identify trends and relationships

According to the descriptive analysis the data has these trends:

- The average number of steps for all users is 7638.
- The average number of calories burned is 2304.
- The average amount of time spent in intense physical activity is 21
  minutes.
- The average amount of time spent in a sedentary state is 991 minutes,
  which is equivalent to 16 hours and 31 minutes.
- The average amount of sleep time is 419 minutes, which is equivalent
  to 6 hours and 59 minutes.

> According to the World Health Organization (WHO), it is recommended
> that adults engage in at least 150 minutes of moderate physical
> activity or 75 minutes of vigorous physical activity per week. This is
> equivalent to an average of 10,000 steps per day. According to
> estimations, a person can be considered sedentary when they burn
> around 1600-2000 calories per day, while a person who exercises
> regularly can burn between 3000-3500 calories per day.

It is necessary to consider the sample size and selection when
interpreting the data. The data is based on a sample of 33 individuals,
which may introduce bias into the analysis. Additionally, there are
limitations to the analysis due to a lack of information about how the
sample was obtained.

Therefore, it can be concluded that the 33 individuals may lead a
sedentary lifestyle, and they probably work in an office for 8 hours a
day. However, it should be noted that they also sleep for 7 hours a day.

# Share

Create effective data visualizations

## Daily steps

``` r
# Distance vs daily steps graph
p1 <- ggplot(d_activity, aes(totalsteps, totaldistance)) + geom_point() + geom_smooth(method = "lm") + labs(x = "Daily Steps", y = "Distance", title = "Distance vs Daily steps") + theme_minimal()

# Calories vs daily steps graph
p2 <- ggplot(d_activity, aes(totalsteps, calories)) + geom_point() + geom_smooth(method = "lm") + labs(x = "Daily Steps", y = "Calories", title = "Calories vs Daily steps") + theme_minimal()

plot_grid(p1, p2, ncol = 2) 
```

    ## `geom_smooth()` using formula = 'y ~ x'
    ## `geom_smooth()` using formula = 'y ~ x'

![](bellabeat_report_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

There is a positive correlation between distance, calories burned, and
daily steps. This means that as the number of daily steps taken
increases, both the distance traveled and the number of calories burned
also increase.

## Steps based on activity

``` r
p7<- d_activity %>% 
  group_by(type) %>% 
  summarize(avg_steps = mean(totalsteps)) %>% 
  arrange(avg_steps)

ggplot(p7, aes(type, avg_steps)) + geom_col() +  theme_minimal() + labs(y = "Average Steps", title = "Steps based on activity", x = "") + geom_text(aes(label = format(avg_steps, format = "%d")), vjust = -0.5)
```

![](bellabeat_report_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
The graph above shows us the average number of steps for each type of
activity.

## Activity types performed by users.

``` r
p8 <- d_activity %>% 
  group_by(type) %>% 
  summarize(count_users = length(unique(id)))
p8
```

    ## # A tibble: 4 × 2
    ##   type          count_users
    ##   <chr>               <int>
    ## 1 Active                 31
    ## 2 Fairly Active          30
    ## 3 Highly Active          21
    ## 4 Sedentary              33

``` r
ggplot(p8, aes(type, count_users)) + geom_col() + theme_minimal() + labs(y = "Users", title = "Activity types performed by users.", x = "") + geom_text(aes(label = count_users), vjust = -0.5)
```

![](bellabeat_report_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

All the individuals in the data set have sedentary activities.
Meanwhile, only 21 of them exercise regularly. Therefore, most of them
are willing to exercise and be mindful of their health and activity
levels.

## Daily calories and intensity

``` r
p3 <- ggplot(d_activity, aes(veryactiveminutes, calories)) + geom_point() + labs(x = "Highly Active [min]", y = "Calories", title = "Highly active minutes, Active minutes, Fairly Active minutes and Sedentary minutes vs Calories") + theme_minimal() + geom_smooth(method = "lm")


p4 <- ggplot(d_activity, aes(fairlyactiveminutes, calories)) + geom_point() + labs(x = "Active [min]", y = "", title = " ") + theme_minimal() + theme(axis.text.y = element_blank()) + geom_smooth(method = "lm")

p5 <- ggplot(d_activity, aes(lightlyactiveminutes, calories)) + geom_point() + labs(x = "Fairly Active [min]", y = "", title = " ") + theme_minimal() + theme(axis.text.y = element_blank()) + geom_smooth(method = "lm")

p6 <- ggplot(d_activity, aes(sedentaryminutes, calories)) + geom_point() + labs(x = "Sedentary [min]", y = "", title = " ") + theme_minimal() + theme(axis.text.y = element_blank()) + geom_smooth(method = "lm")

plot_grid(p3, p4, p5, p6, ncol = 4)  
```

    ## `geom_smooth()` using formula = 'y ~ x'
    ## `geom_smooth()` using formula = 'y ~ x'
    ## `geom_smooth()` using formula = 'y ~ x'
    ## `geom_smooth()` using formula = 'y ~ x'

![](bellabeat_report_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

There is a positive correlation between the type of activity and the
calories burned, meaning that the more highly active you are, the more
calories you burn, and vice versa. According to the data set results, 20
minutes of highly intense activity burns the same amount of calories as
8 hours of sedentary activity.

## Calories burned according to day of the week.

``` r
d_activity %>% 
  group_by(day_of_week) %>% 
  summarize(avg_calories = mean(calories)) %>% 
  ggplot(aes(day_of_week, avg_calories)) + geom_col() +  theme_minimal() + labs(y = "Average Calories", title = "Calories burned according to day of the week.", x = "") + geom_text(aes(label = format(avg_calories, format = "%d")), vjust = -0.5)
```

![](bellabeat_report_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

The calories burned remain constant throughout the week. However,
certain days, such as Sunday and Thursday, involve less physical
activity.

## Sleep time

``` r
q1 <- d_sleep %>% 
  group_by(type) %>% 
  summarize(avg_sleep = mean(totalminutesasleep), users = length(unique(id)))
q1
```

    ## # A tibble: 3 × 3
    ##   type            avg_sleep users
    ##   <chr>               <dbl> <int>
    ## 1 average sleep        371.    19
    ## 2 healthy sleep        497.    21
    ## 3 unhealthy sleep      192.    16

``` r
ggplot(q1, aes(type, users, fill = avg_sleep)) + geom_col() + theme_minimal() + labs(y = "Users", title = "How many users sleep well or poorly", x = "", fill = "Asleep minutes") + geom_text(aes(label = users), vjust = -0.5)
```

![](bellabeat_report_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

The graph above shows that most users sleep well, although many of them
had unhealthy sleep on some days during the period of data collection.

## Relationship between sleep and calories.

``` r
# se unen ambos archivos segun 'id' y 'date' 
q2 <- merge(d_activity, d_sleep, by = c('id', 'date')) 

# se hace un summary de las columnas que se va a graficar
q3 <- q2 %>% 
  select(calories, totalminutesasleep, type.y) %>% 
  group_by(type.y) %>% 
  summarize(mean(calories), max(calories), min(calories), median(calories), quantile(calories, probs = 0.25), quantile(calories, probs = 0.75))
q3
```

    ## # A tibble: 3 × 7
    ##   type.y     `mean(calories)` `max(calories)` `min(calories)` `median(calories)`
    ##   <chr>                 <dbl>           <dbl>           <dbl>              <dbl>
    ## 1 average s…            2512.            4546             257               2305
    ## 2 healthy s…            2376.            4900             741               2220
    ## 3 unhealthy…            2192.            3846            1407               1980
    ## # ℹ 2 more variables: `quantile(calories, probs = 0.25)` <dbl>,
    ## #   `quantile(calories, probs = 0.75)` <dbl>

``` r
# se grafica la correlacion entre calorias y sueno
ggplot(q2, aes(totalminutesasleep, calories,  color = type.y)) + geom_point() + geom_smooth(method = "lm") + facet_wrap(~type.y) + labs(x = "Asleep minutes", y = "Calories", title = "Asleep minutes vs Calories burned", color = "Quality of sleep") + theme_minimal()
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](bellabeat_report_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

This visualization shows us a positive correlation between calories and
sleep, which means that if you sleep for more than 8 hours or less than
5 hours, you burn fewer calories, while if you sleep between 5 and 8
hours, you tend to burn more calories. This means that sleep quality has
a positive impact on your physical activity and, therefore, on your
health.

<!-- Archivos guardados para hacer dashboard

```r
# guardar archivo 
write_csv(d_activity, "processed_data/dashboard_activity.csv")
write_csv(d_sleep, "processed_data/dashboard_sleep.csv")
```
-->

# Act

## Conclusion.

Most of the people within the dataset who use Bellabeat products lead
sedentary lives, but during the data collection period, it is also shown
that they engage in activities to take care of their health, although
this is not a daily habit for everyone. This causes their physical
activity based on the number of steps and calories burned to be only
slightly above that of a completely sedentary person. Therefore, it is
necessary to add highly active physical activity for at least 30 minutes
to achieve the goal proposed by the WHO of 3000 daily calories burned.

However, according to the data, it is also important to focus on the
amount of sleep. Sleeping more than necessary also has a negative impact
on our physical activity just as sleeping too little does. It is
important to get enough sleep, between 5 to 8 hours per day for adults,
to improve physical activity and therefore, overall health.

Finally, the missing data from the heart rate and weight files are
crucial for providing a deeper analysis of the products offered by
Bellabeat, yet they do not contain enough data. Therefore, it is
important to encourage the use of the application and its products.

## Recommendations based on your analysis.

- Weekly contests that allow users to create a more active community and
  become more familiar with Bellabeat.
- Contests based on physical activity and getting enough sleep.
- The Bellabeat app product can offer recognition to its users for
  meeting goals by rewarding them with discounts on their other products
  for their usage.
- Recommending a home workout routine to help achieve fitness goals.
- Recommending activities to improve sleep quality.
- Recommending activities to eat healthily.
- Newsletter providing information on how much to sleep, how much to
  exercise, and other tips.
- Simplifying the information entry process for users.

Contact information: <https://www.linkedin.com/in/guinansacaw>

![](images/bellabeat_screenshot.png)

<!-- Algunas funciones de las clases de COursera

> recordar: rename_with(<dataframe>, tolower) 

> clean_names(<dataframe>) se asegura que solo existan chr, num, underscores en los nombres  y elimina caracteres especiales como espacios en blanco, signos de puntuación, guiones y otros caracteres no alfanuméricos.

> librerias: here, janitor, skimr

> drop_na() elimina filas con valores faltantes, y puede usar argumentos

> separate(<df>, <column>, into=c('first_name', 'last_name'), sep=' ') es lo opuesto a concatenar

> unite(<df>, 'column', first_name, last_name, sep=' ')

> summarize(mean(x), sd(x), cor(x,y))

> SimDesing es el paquete para la funcion bias() la cual compara los resultados 

> trimws() para eliminar espacios enb blanco

> as.factor() para asegurarse que las columnas categoricas esten en el formato correcto

-->
