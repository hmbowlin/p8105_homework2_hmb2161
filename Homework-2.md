Homework 2
================

# Problem 1

  - Mr. Trash Wheel Sheet

<!-- end list -->

``` r
#Load tidyverse library
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
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

``` r
precipitation_data = 
  bind_rows(precipitation_2017, precipitation_2018) %>%
  janitor::clean_names() %>%
  mutate(month = month.name[as.numeric(month)])

precipitation_data
```

    ## # A tibble: 19 x 3
    ##    month     total year 
    ##    <chr>     <dbl> <chr>
    ##  1 January    0.96 2017 
    ##  2 February   5.3  2017 
    ##  3 March      2.18 2017 
    ##  4 April      3.2  2017 
    ##  5 May        9.27 2017 
    ##  6 June       0.2  2017 
    ##  7 July       2.39 2017 
    ##  8 January    2.34 2018 
    ##  9 February   1.46 2018 
    ## 10 March      3.57 2018 
    ## 11 April      3.99 2018 
    ## 12 May        5.64 2018 
    ## 13 June       1.4  2018 
    ## 14 July       7.09 2018 
    ## 15 August     4.44 2018 
    ## 16 September  1.95 2018 
    ## 17 October    0    2018 
    ## 18 November   0.11 2018 
    ## 19 December   0.94 2018

The data in the mrtrashwheel\_data set has 18 number of observations.
The data describe the contents, weight, and volume of dumpsters during
2015 from April to December. The mean weight of the dumpsters is
3.6505556 and the median number of sports balls in each dumpster is
14.5. The data in precipitation\_data has 19 number of observations. The
data show the amount of precipitation per month in 2017 and 2018. For
2018 the amount of precipitation is

# Problem 2

\*Clean data in pols-month.csv

``` r
library(tidyverse)
library(dplyr)
pols_month_data =
  read_csv(file = "./data_hw2/fivethirtyeight_datasets/pols-month.csv") %>%
  janitor::clean_names(dat = .) %>%
  separate(mon, c("year", "month", "day"), convert = TRUE) %>%
  mutate(month = month.name[as.numeric(month)]) %>%
   pivot_longer(
        cols = starts_with("prez_"),
    names_to = "president",
    names_prefix = "prez_",
    values_to = c("gop","dem")
   ) %>%
  arrange(year, month) %>%
  select(-day)
```

    ## Parsed with column specification:
    ## cols(
    ##   mon = col_date(format = ""),
    ##   prez_gop = col_double(),
    ##   gov_gop = col_double(),
    ##   sen_gop = col_double(),
    ##   rep_gop = col_double(),
    ##   prez_dem = col_double(),
    ##   gov_dem = col_double(),
    ##   sen_dem = col_double(),
    ##   rep_dem = col_double()
    ## )

``` r
pols_month_data
```

    ## # A tibble: 1,644 x 11
    ##     year month gov_gop sen_gop rep_gop gov_dem sen_dem rep_dem president
    ##    <int> <chr>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl> <chr>    
    ##  1  1947 April      23      51     253      23      45     198 gop      
    ##  2  1947 April      23      51     253      23      45     198 dem      
    ##  3  1947 Augu…      23      51     253      23      45     198 gop      
    ##  4  1947 Augu…      23      51     253      23      45     198 dem      
    ##  5  1947 Dece…      24      51     253      23      45     198 gop      
    ##  6  1947 Dece…      24      51     253      23      45     198 dem      
    ##  7  1947 Febr…      23      51     253      23      45     198 gop      
    ##  8  1947 Febr…      23      51     253      23      45     198 dem      
    ##  9  1947 Janu…      23      51     253      23      45     198 gop      
    ## 10  1947 Janu…      23      51     253      23      45     198 dem      
    ## # … with 1,634 more rows, and 2 more variables: gop <dbl>, dem <dbl>

\*Clean data in snp.csv

``` r
snp_data =
  read_csv(file = "./data_hw2/fivethirtyeight_datasets/snp.csv") %>%
  janitor::clean_names() %>%
  separate(date, c("month","day","year"), convert = TRUE) %>%
  mutate(month = month.name[as.numeric(month)]) %>%
  arrange(year, month) %>%
  select(year, month, close, -day)
```

    ## Parsed with column specification:
    ## cols(
    ##   date = col_character(),
    ##   close = col_double()
    ## )

``` r
snp_data
```

    ## # A tibble: 787 x 3
    ##     year month    close
    ##    <int> <chr>    <dbl>
    ##  1  1950 April     18.0
    ##  2  1950 August    18.4
    ##  3  1950 December  20.4
    ##  4  1950 February  17.2
    ##  5  1950 January   17.0
    ##  6  1950 July      17.8
    ##  7  1950 June      17.7
    ##  8  1950 March     17.3
    ##  9  1950 May       18.8
    ## 10  1950 November  19.5
    ## # … with 777 more rows

  - Clean unemployment data

<!-- end list -->

``` r
unemployment_data = 
  read_csv(file = "./data_hw2/fivethirtyeight_datasets/unemployment.csv") %>%
   pivot_longer(
    Jan:Dec,
    names_to = "month",
    values_to = "rate") %>%
   mutate(month = month.name[match(month,month.abb)]) %>%
  janitor::clean_names()
```

    ## Parsed with column specification:
    ## cols(
    ##   Year = col_double(),
    ##   Jan = col_double(),
    ##   Feb = col_double(),
    ##   Mar = col_double(),
    ##   Apr = col_double(),
    ##   May = col_double(),
    ##   Jun = col_double(),
    ##   Jul = col_double(),
    ##   Aug = col_double(),
    ##   Sep = col_double(),
    ##   Oct = col_double(),
    ##   Nov = col_double(),
    ##   Dec = col_double()
    ## )

``` r
 unemployment_data
```

    ## # A tibble: 816 x 3
    ##     year month      rate
    ##    <dbl> <chr>     <dbl>
    ##  1  1948 January     3.4
    ##  2  1948 February    3.8
    ##  3  1948 March       4  
    ##  4  1948 April       3.9
    ##  5  1948 May         3.5
    ##  6  1948 June        3.6
    ##  7  1948 July        3.6
    ##  8  1948 August      3.9
    ##  9  1948 September   3.8
    ## 10  1948 October     3.7
    ## # … with 806 more rows

  - Joining datasets

<!-- end list -->

``` r
fivethirtyeight_data = 
  bind_rows(pols_month_data, snp_data) %>%
  janitor::clean_names()

fivethirtyeight_data = 
  bind_rows(fivethirtyeight_data, unemployment_data) %>%
  janitor::clean_names()

fivethirtyeight_data
```

    ## # A tibble: 3,247 x 13
    ##     year month gov_gop sen_gop rep_gop gov_dem sen_dem rep_dem president
    ##    <dbl> <chr>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl> <chr>    
    ##  1  1947 April      23      51     253      23      45     198 gop      
    ##  2  1947 April      23      51     253      23      45     198 dem      
    ##  3  1947 Augu…      23      51     253      23      45     198 gop      
    ##  4  1947 Augu…      23      51     253      23      45     198 dem      
    ##  5  1947 Dece…      24      51     253      23      45     198 gop      
    ##  6  1947 Dece…      24      51     253      23      45     198 dem      
    ##  7  1947 Febr…      23      51     253      23      45     198 gop      
    ##  8  1947 Febr…      23      51     253      23      45     198 dem      
    ##  9  1947 Janu…      23      51     253      23      45     198 gop      
    ## 10  1947 Janu…      23      51     253      23      45     198 dem      
    ## # … with 3,237 more rows, and 4 more variables: gop <dbl>, dem <dbl>,
    ## #   close <dbl>, rate <dbl>

These data show various election results from months and years along
with certain SNP500 close values and unemployment rates. The
pols\_month\_data data show whether or not the governor, senators,
representatives, and president were democratic or republican in that
month and year from April 1947 to March 1988. The pols\_month\_data has
1644 rows and 11 columns. The snp\_data shows the month and year and the
SNP close value of how the S\&P500 stock market index did from April
1950 to May 2015. The average closing point of the S\&P over the 65
years is 474.8887404. The snp\_data has 787 rows and 3 columns.The
unemployment\_data dataset shows the unemployment rate for that month
and year from January 1948 to December 2015. The average unemployment
rate is NA. The unemployment\_data has 816 rows and 3columns. It’s
important to note that all three of these datasets have different
starting and ending months and years.The final combined dataset,
fivethirtyeight\_data has 3247 rows and 13 columns.

# Problem 3

``` r
popular_baby = 
  read_csv(file = "./data_hw2/Popular_Baby_Names.csv") %>%
  janitor::clean_names() %>%
  mutate(ethnicity = recode(ethnicity, 'ASIAN AND PACIFIC ISLANDER' = "asian and pacific islander",'BLACK NON HISPANIC' = "black", 'HISPANIC' = "hispanic", 'WHITE NON HISPANIC' = "white", 'ASIAN AND PACI' = "asian and pacific islander", 'BLACK NON HISP' = "black", 'WHITE NON HISP' = "white")) %>%
  group_by(childs_first_name, rank) %>%
  mutate("count" = row_number()) %>%
  pivot_wider(
    names_from = "ethnicity",
    values_from = "rank") %>%
  select(-count) %>%
  distinct()
```

    ## Parsed with column specification:
    ## cols(
    ##   `Year of Birth` = col_double(),
    ##   Gender = col_character(),
    ##   Ethnicity = col_character(),
    ##   `Child's First Name` = col_character(),
    ##   Count = col_double(),
    ##   Rank = col_double()
    ## )

``` r
popular_baby
```

    ## # A tibble: 8,698 x 7
    ## # Groups:   childs_first_name [3,021]
    ##    year_of_birth gender childs_first_na… `asian and paci… black hispanic
    ##            <dbl> <chr>  <chr>                       <dbl> <dbl>    <dbl>
    ##  1          2016 FEMALE Olivia                          1     8       13
    ##  2          2016 FEMALE Chloe                           2     7       24
    ##  3          2016 FEMALE Sophia                          3    15        2
    ##  4          2016 FEMALE Emily                           4    27        7
    ##  5          2016 FEMALE Emma                            4    29       NA
    ##  6          2016 FEMALE Mia                             5    17        3
    ##  7          2016 FEMALE Charlotte                       6    34       39
    ##  8          2016 FEMALE Sarah                           7    28       38
    ##  9          2016 FEMALE Isabella                        8    12        1
    ## 10          2016 FEMALE Hannah                          8    36       69
    ## # … with 8,688 more rows, and 1 more variable: white <dbl>
