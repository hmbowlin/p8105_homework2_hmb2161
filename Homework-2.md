Homework 2
================

# Problem 1

  - Mr. Trash Wheel Sheet

<!-- end list -->

``` r
#Load tidyverse library
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
# import in the Mr. Trash Wheel sheet from mrtrashwheel dataset
# first row is skipped as it contains company logo, blank celsl are dropped, and the names are cleaned up
# drop any blank cells, select everything but homes_powered and x15, change sports_balls to an integer format
mrtrashwheel_data = 
  readxl::read_excel(
    "./data_hw2/mrtrashwheel.xlsx", 
    sheet = 1, 
    skip = 1,
    col_names = TRUE,
    col_types = NULL) %>%
janitor::clean_names() %>%
drop_na() %>%
select(-homes_powered, -x15) %>%
mutate(sport_balls = as.integer(sports_balls)) %>%
select(-sports_balls)
```

    ## New names:
    ## * `` -> ...15

``` r
mrtrashwheel_data
```

    ## # A tibble: 18 x 13
    ##    dumpster month  year date                weight_tons volume_cubic_ya…
    ##       <dbl> <chr> <dbl> <dttm>                    <dbl>            <dbl>
    ##  1       57 April  2015 2015-04-20 00:00:00        3.03               15
    ##  2       58 April  2015 2015-04-20 00:00:00        2.64               18
    ##  3       71 June   2015 2015-06-09 00:00:00        3.83               15
    ##  4       73 June   2015 2015-06-10 00:00:00        3.85               18
    ##  5       74 June   2015 2015-06-10 00:00:00        4.48               15
    ##  6       75 June   2015 2015-06-10 00:00:00        4.18               15
    ##  7       76 June   2015 2015-06-10 00:00:00        3.38               18
    ##  8       78 June   2015 2015-06-12 00:00:00        3.95               15
    ##  9       97 Augu…  2015 2015-08-25 00:00:00        4.3                15
    ## 10       98 Augu…  2015 2015-08-26 00:00:00        3.39               15
    ## 11       99 Sept…  2015 2015-09-11 00:00:00        4.08               18
    ## 12      100 Sept…  2015 2015-09-11 00:00:00        3.62               15
    ## 13      101 Sept…  2015 2015-09-22 00:00:00        2.4                 7
    ## 14      102 Sept…  2015 2015-09-30 00:00:00        4.24               18
    ## 15      104 Octo…  2015 2015-10-12 00:00:00        2.9                15
    ## 16      107 Nove…  2015 2015-11-20 00:00:00        4.46               15
    ## 17      108 Dece…  2015 2015-12-02 00:00:00        3.42               18
    ## 18      109 Dece…  2015 2015-12-02 00:00:00        3.56               15
    ## # … with 7 more variables: plastic_bottles <dbl>, polystyrene <dbl>,
    ## #   cigarette_butts <dbl>, glass_bottles <dbl>, grocery_bags <dbl>,
    ## #   chip_bags <dbl>, sport_balls <int>

  - Precipitation Sheets

<!-- end list -->

``` r
precipitation_2017 = 
  readxl::read_excel(
    "./data_hw2/mrtrashwheel.xlsx", 
    sheet = 3,
    range = "A2:B14",
    col_names = TRUE,
    col_types = NULL) %>%
  mutate(year = "2017") %>%
  janitor::clean_names() %>%
  drop_na()

precipitation_2017
```

    ## # A tibble: 7 x 3
    ##   month total year 
    ##   <dbl> <dbl> <chr>
    ## 1     1  0.96 2017 
    ## 2     2  5.3  2017 
    ## 3     3  2.18 2017 
    ## 4     4  3.2  2017 
    ## 5     5  9.27 2017 
    ## 6     6  0.2  2017 
    ## 7     7  2.39 2017

``` r
precipitation_2018 = 
  readxl::read_excel(
    "./data_hw2/mrtrashwheel.xlsx", 
    sheet = 4,
    range = "A2:B14",
    col_names = TRUE,
    col_types = NULL) %>%
  mutate(year = "2018") %>%
  janitor::clean_names() %>%
  drop_na()

precipitation_2018
```

    ## # A tibble: 12 x 3
    ##    month total year 
    ##    <dbl> <dbl> <chr>
    ##  1     1  2.34 2018 
    ##  2     2  1.46 2018 
    ##  3     3  3.57 2018 
    ##  4     4  3.99 2018 
    ##  5     5  5.64 2018 
    ##  6     6  1.4  2018 
    ##  7     7  7.09 2018 
    ##  8     8  4.44 2018 
    ##  9     9  1.95 2018 
    ## 10    10  0    2018 
    ## 11    11  0.11 2018 
    ## 12    12  0.94 2018
