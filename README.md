Practice Exam
=============

This practice exam asks you to do several code wrangling tasks that we
have done in class so far.

Clone this repo into Rstudio and fill in the necessary code. Then,
commit and push to github. Finally, turn in a link to canvas.

    ## Registered S3 methods overwritten by 'ggplot2':
    ##   method         from 
    ##   [.quosures     rlang
    ##   c.quosures     rlang
    ##   print.quosures rlang

    ## Registered S3 method overwritten by 'rvest':
    ##   method            from
    ##   read_xml.response xml2

    ## -- Attaching packages ------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.1.0     v purrr   0.3.0
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts ---------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    ## Warning: package 'nycflights13' was built under R version 3.6.2

Make a plot with three facets, one for each airport in the weather data.
The x-axis should be the day of the year (1:365) and the y-axis should
be the mean temperature recorded on that day, at that airport.

    library(lubridate)

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

    w=weather %>% 
      mutate(day_of_year = yday(time_hour)) %>% 
      group_by(origin,day_of_year) %>% 
      summarise(mt=mean(temp))
    p <- ggplot(data = w,
                mapping = aes(x = day_of_year,
                              y =mt,
                              color=origin)) #####

    p + geom_point(alpha = 0.3)

    ## Warning: Removed 1 rows containing missing values (geom_point).

![](README_files/figure-markdown_strict/unnamed-chunk-2-1.png)

Make a non-tidy matrix of that data where each row is an airport and
each column is a day of the year.

    w %>%
      pivot_wider(names_from = day_of_year, values_from = mt)

    ## # A tibble: 3 x 365
    ## # Groups:   origin [3]
    ##   origin   `1`   `2`   `3`   `4`   `5`   `6`   `7`   `8`   `9`  `10`  `11`
    ##   <chr>  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1 EWR     36.8  28.7  29.6  34.3  36.6  39.9  40.3  38.6  42.1  43.6  42.0
    ## 2 JFK     36.9  28.6  30.1  34.7  36.8  39.3  40.1  39.4  42.7  43.6  41.3
    ## 3 LGA     37.2  28.8  30.3  35.8  38.3  41.0  41.4  42.3  44.9  44.3  40.3
    ## # ... with 353 more variables: `12` <dbl>, `13` <dbl>, `14` <dbl>,
    ## #   `15` <dbl>, `16` <dbl>, `17` <dbl>, `18` <dbl>, `19` <dbl>,
    ## #   `20` <dbl>, `21` <dbl>, `22` <dbl>, `23` <dbl>, `24` <dbl>,
    ## #   `25` <dbl>, `26` <dbl>, `27` <dbl>, `28` <dbl>, `29` <dbl>,
    ## #   `30` <dbl>, `31` <dbl>, `32` <dbl>, `33` <dbl>, `34` <dbl>,
    ## #   `35` <dbl>, `36` <dbl>, `37` <dbl>, `38` <dbl>, `39` <dbl>,
    ## #   `40` <dbl>, `41` <dbl>, `42` <dbl>, `43` <dbl>, `44` <dbl>,
    ## #   `45` <dbl>, `46` <dbl>, `47` <dbl>, `48` <dbl>, `49` <dbl>,
    ## #   `50` <dbl>, `51` <dbl>, `52` <dbl>, `53` <dbl>, `54` <dbl>,
    ## #   `55` <dbl>, `56` <dbl>, `57` <dbl>, `58` <dbl>, `59` <dbl>,
    ## #   `60` <dbl>, `61` <dbl>, `62` <dbl>, `63` <dbl>, `64` <dbl>,
    ## #   `65` <dbl>, `66` <dbl>, `67` <dbl>, `68` <dbl>, `69` <dbl>,
    ## #   `70` <dbl>, `71` <dbl>, `72` <dbl>, `73` <dbl>, `74` <dbl>,
    ## #   `75` <dbl>, `76` <dbl>, `77` <dbl>, `78` <dbl>, `79` <dbl>,
    ## #   `80` <dbl>, `81` <dbl>, `82` <dbl>, `83` <dbl>, `84` <dbl>,
    ## #   `85` <dbl>, `86` <dbl>, `87` <dbl>, `88` <dbl>, `89` <dbl>,
    ## #   `90` <dbl>, `91` <dbl>, `92` <dbl>, `93` <dbl>, `94` <dbl>,
    ## #   `95` <dbl>, `96` <dbl>, `97` <dbl>, `98` <dbl>, `99` <dbl>,
    ## #   `100` <dbl>, `101` <dbl>, `102` <dbl>, `103` <dbl>, `104` <dbl>,
    ## #   `105` <dbl>, `106` <dbl>, `107` <dbl>, `108` <dbl>, `109` <dbl>,
    ## #   `110` <dbl>, `111` <dbl>, ...

For each (airport, day) contruct a tidy data set of the airport’s
“performance” as the proportion of flights that departed less than an
hour late.

    w1=flights %>% 
      mutate(day_of_year = yday(time_hour)) %>% 
      group_by(origin,day_of_year) %>% 
      summarise(performance=sum(dep_delay<=60,na.rm = T)/n())
    w1

    ## # A tibble: 1,095 x 3
    ## # Groups:   origin [3]
    ##    origin day_of_year performance
    ##    <chr>        <dbl>       <dbl>
    ##  1 EWR              1       0.915
    ##  2 EWR              2       0.823
    ##  3 EWR              3       0.970
    ##  4 EWR              4       0.938
    ##  5 EWR              5       0.962
    ##  6 EWR              6       0.950
    ##  7 EWR              7       0.924
    ##  8 EWR              8       0.976
    ##  9 EWR              9       0.973
    ## 10 EWR             10       0.977
    ## # ... with 1,085 more rows

Construct a tidy data set to that give weather summaries for each
(airport, day). Use the total precipitation, minimum visibility, maximum
wind\_gust, and average wind\_speed.

    w2=weather %>% 
      mutate(day_of_year = yday(time_hour)) %>% 
      group_by(origin,day_of_year) %>% 
      summarise(total_precip=sum(precip,na.rm = T),
                minvis=min(visib,na.rm = T),
                #maxwindgust=max(wind_gust,na.rm = T),
                mean_windspeed=mean(wind_speed,na.rm = T))
    w2

    ## # A tibble: 1,092 x 5
    ## # Groups:   origin [3]
    ##    origin day_of_year total_precip minvis mean_windspeed
    ##    <chr>        <dbl>        <dbl>  <dbl>          <dbl>
    ##  1 EWR              1            0     10          13.2 
    ##  2 EWR              2            0     10          10.9 
    ##  3 EWR              3            0     10           8.58
    ##  4 EWR              4            0     10          14.0 
    ##  5 EWR              5            0     10           9.40
    ##  6 EWR              6            0      6           9.11
    ##  7 EWR              7            0     10           7.34
    ##  8 EWR              8            0      8           7.19
    ##  9 EWR              9            0      6           5.99
    ## 10 EWR             10            0     10           8.92
    ## # ... with 1,082 more rows

Construct a linear model to predict the performance of each
(airport,day) using the weather summaries and a “fixed effect” for each
airport. Display the summaries.

    w3=left_join(w1,w2,by=c('origin',"day_of_year"))
    m=lm(performance~origin+total_precip+minvis+mean_windspeed,data = w3)
    summary(m)

    ## 
    ## Call:
    ## lm(formula = performance ~ origin + total_precip + minvis + mean_windspeed, 
    ##     data = w3)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.48424 -0.02045  0.01945  0.04519  0.24542 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     0.8277301  0.0105353  78.568  < 2e-16 ***
    ## originJFK       0.0324497  0.0068000   4.772 2.07e-06 ***
    ## originLGA       0.0195900  0.0066880   2.929  0.00347 ** 
    ## total_precip   -0.0650017  0.0099935  -6.504 1.19e-10 ***
    ## minvis          0.0125367  0.0008955  14.000  < 2e-16 ***
    ## mean_windspeed -0.0030934  0.0006640  -4.659 3.57e-06 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.08958 on 1086 degrees of freedom
    ##   (3 observations deleted due to missingness)
    ## Multiple R-squared:  0.3029, Adjusted R-squared:  0.2997 
    ## F-statistic: 94.37 on 5 and 1086 DF,  p-value: < 2.2e-16

Repeat the above, but only for EWR. Obviously, exclude the fixed effect
for each airport.

    w4=filter(w3,origin=='EWR')
    m2=lm(performance~total_precip+minvis+mean_windspeed,data = w4)
    summary(m2)

    ## 
    ## Call:
    ## lm(formula = performance ~ total_precip + minvis + mean_windspeed, 
    ##     data = w4)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.37517 -0.03483  0.02249  0.05214  0.25440 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     0.824397   0.016821  49.010  < 2e-16 ***
    ## total_precip   -0.064035   0.016741  -3.825 0.000154 ***
    ## minvis          0.014323   0.001665   8.601 2.46e-16 ***
    ## mean_windspeed -0.004183   0.001116  -3.747 0.000209 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.09261 on 360 degrees of freedom
    ##   (1 observation deleted due to missingness)
    ## Multiple R-squared:  0.3203, Adjusted R-squared:  0.3147 
    ## F-statistic: 56.56 on 3 and 360 DF,  p-value: < 2.2e-16
