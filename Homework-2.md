Homework 2
================

``` r
#Load tidyverse library
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ───────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
#import in the Mr. Trash Wheel sheet from mrtrashwheel dataset
#first row is skipped as it contains company logo, blank celsl are dropped, and the names are cleaned up
mrtrashwheel_data = 
  readxl::read_excel(
    "./data_hw2/mrtrashwheel.xlsx", 
    sheet = 1, 
    skip = 1,
    col_names = TRUE,
    col_types = NULL) %>%
janitor::clean_names() %>%
select(-homes_powered) %>%
drop_na()
```

    ## New names:
    ## * `` -> ...15
