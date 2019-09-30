Homework 2
================

# Problem 1

  - Mr. Trash Wheel Sheet

<!-- end list -->

``` r
#Load tidyverse library
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
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
    range = "A2:N338",
    col_names = TRUE,
    col_types = NULL) %>%
janitor::clean_names() %>%
drop_na() %>%
select(-homes_powered) %>%
mutate(sport_balls = as.integer(sports_balls)) %>%
select(-sports_balls)
mrtrashwheel_data
```

    ## # A tibble: 285 x 13
    ##    dumpster month  year date                weight_tons volume_cubic_ya…
    ##       <dbl> <chr> <dbl> <dttm>                    <dbl>            <dbl>
    ##  1        1 May    2014 2014-05-16 00:00:00        4.31               18
    ##  2        2 May    2014 2014-05-16 00:00:00        2.74               13
    ##  3        3 May    2014 2014-05-16 00:00:00        3.45               15
    ##  4        4 May    2014 2014-05-17 00:00:00        3.1                15
    ##  5        5 May    2014 2014-05-17 00:00:00        4.06               18
    ##  6        6 May    2014 2014-05-20 00:00:00        2.71               13
    ##  7        7 May    2014 2014-05-21 00:00:00        1.91                8
    ##  8        8 May    2014 2014-05-28 00:00:00        3.7                16
    ##  9        9 June   2014 2014-06-05 00:00:00        2.52               14
    ## 10       10 June   2014 2014-06-11 00:00:00        3.76               18
    ## # … with 275 more rows, and 7 more variables: plastic_bottles <dbl>,
    ## #   polystyrene <dbl>, cigarette_butts <dbl>, glass_bottles <dbl>,
    ## #   grocery_bags <dbl>, chip_bags <dbl>, sport_balls <int>

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

The data in the mrtrashwheel\_data set has 285 number of observations.
The data describe the contents, weight, and volume of dumpsters during
2015 from April to December. The mean weight of the dumpsters is
3.2804912 and the median number of sports balls in a dumpster in 2017 is
8. The data in precipitation\_data has 19 number of observations. The
data show the amount of precipitation per month in 2017 and 2018. For
2018 the amount of precipitation is 32.93 inches and for 2017 the amount
of precipitation is 23.5 inches.

# Problem 2

\*Clean data in pols-month.csv

``` r
pols_month_data =
  read_csv(file = "./data_hw2/fivethirtyeight_datasets/pols-month.csv") %>%
  janitor::clean_names(dat = .) %>%
  separate(mon, c("year", "month", "day"), convert = TRUE) %>%
   mutate(month = month.name[as.numeric(month)]) %>%
  pivot_longer(starts_with("prez_"),
               values_to = "binary") %>%
  filter(binary == 1) %>%
  arrange(year, month) %>%
  select(-day,-binary, president = name) %>%
  transform(president = str_replace(president, "prez_", ""))%>%
  distinct()
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

    ##     year     month gov_gop sen_gop rep_gop gov_dem sen_dem rep_dem
    ## 1   1947     April      23      51     253      23      45     198
    ## 2   1947    August      23      51     253      23      45     198
    ## 3   1947  December      24      51     253      23      45     198
    ## 4   1947  February      23      51     253      23      45     198
    ## 5   1947   January      23      51     253      23      45     198
    ## 6   1947      July      23      51     253      23      45     198
    ## 7   1947      June      23      51     253      23      45     198
    ## 8   1947     March      23      51     253      23      45     198
    ## 9   1947       May      23      51     253      23      45     198
    ## 10  1947  November      24      51     253      23      45     198
    ## 11  1947   October      23      51     253      23      45     198
    ## 12  1947 September      23      51     253      23      45     198
    ## 13  1948     April      22      53     253      24      48     198
    ## 14  1948    August      22      53     253      24      48     198
    ## 15  1948  December      22      53     253      24      48     198
    ## 16  1948  February      22      53     253      24      48     198
    ## 17  1948   January      22      53     253      24      48     198
    ## 18  1948      July      22      53     253      24      48     198
    ## 19  1948      June      22      53     253      24      48     198
    ## 20  1948     March      22      53     253      24      48     198
    ## 21  1948       May      22      53     253      24      48     198
    ## 22  1948  November      22      53     253      24      48     198
    ## 23  1948   October      22      53     253      24      48     198
    ## 24  1948 September      22      53     253      24      48     198
    ## 25  1949     April      18      45     177      29      58     269
    ## 26  1949    August      18      45     177      30      58     269
    ## 27  1949  December      18      45     177      30      58     269
    ## 28  1949  February      18      45     177      29      58     269
    ## 29  1949   January      18      45     177      29      58     269
    ## 30  1949      July      18      45     177      30      58     269
    ## 31  1949      June      18      45     177      29      58     269
    ## 32  1949     March      18      45     177      29      58     269
    ## 33  1949       May      18      45     177      29      58     269
    ## 34  1949  November      18      45     177      30      58     269
    ## 35  1949   October      18      45     177      30      58     269
    ## 36  1949 September      18      45     177      30      58     269
    ## 37  1950     April      18      44     177      29      57     269
    ## 38  1950    August      18      44     177      29      57     269
    ## 39  1950  December      18      44     177      29      57     269
    ## 40  1950  February      18      44     177      29      57     269
    ## 41  1950   January      18      44     177      29      57     269
    ## 42  1950      July      18      44     177      29      57     269
    ## 43  1950      June      18      44     177      29      57     269
    ## 44  1950     March      18      44     177      29      57     269
    ## 45  1950       May      18      44     177      29      57     269
    ## 46  1950  November      18      44     177      29      57     269
    ## 47  1950   October      18      44     177      29      57     269
    ## 48  1950 September      18      44     177      29      57     269
    ## 49  1951     April      24      47     207      22      51     242
    ## 50  1951    August      24      47     207      22      51     242
    ## 51  1951  December      25      47     207      22      51     242
    ## 52  1951  February      24      47     207      22      51     242
    ## 53  1951   January      24      47     207      22      51     242
    ## 54  1951      July      24      47     207      22      51     242
    ## 55  1951      June      24      47     207      22      51     242
    ## 56  1951     March      24      47     207      22      51     242
    ## 57  1951       May      24      47     207      22      51     242
    ## 58  1951  November      25      47     207      22      51     242
    ## 59  1951   October      25      47     207      22      51     242
    ## 60  1951 September      24      47     207      22      51     242
    ## 61  1952     April      24      50     207      22      50     242
    ## 62  1952    August      24      50     207      22      50     242
    ## 63  1952  December      24      50     207      22      50     242
    ## 64  1952  February      24      50     207      22      50     242
    ## 65  1952   January      24      50     207      22      50     242
    ## 66  1952      July      24      50     207      22      50     242
    ## 67  1952      June      24      50     207      22      50     242
    ## 68  1952     March      24      50     207      22      50     242
    ## 69  1952       May      24      50     207      22      50     242
    ## 70  1952  November      24      50     207      22      50     242
    ## 71  1952   October      24      50     207      22      50     242
    ## 72  1952 September      24      50     207      22      50     242
    ## 73  1953     April      29      50     222      17      49     220
    ## 74  1953    August      29      50     222      17      49     220
    ## 75  1953  December      30      50     222      18      49     220
    ## 76  1953  February      29      50     222      17      49     220
    ## 77  1953   January      29      50     222      17      49     220
    ## 78  1953      July      29      50     222      17      49     220
    ## 79  1953      June      29      50     222      17      49     220
    ## 80  1953     March      29      50     222      17      49     220
    ## 81  1953       May      29      50     222      17      49     220
    ## 82  1953  November      30      50     222      18      49     220
    ## 83  1953   October      30      50     222      18      49     220
    ## 84  1953 September      29      50     222      17      49     220
    ## 85  1954     April      29      55     222      18      53     220
    ## 86  1954    August      29      55     222      18      53     220
    ## 87  1954  December      29      55     222      19      53     220
    ## 88  1954  February      29      55     222      18      53     220
    ## 89  1954   January      29      55     222      18      53     220
    ## 90  1954      July      29      55     222      18      53     220
    ## 91  1954      June      29      55     222      18      53     220
    ## 92  1954     March      29      55     222      18      53     220
    ## 93  1954       May      29      55     222      18      53     220
    ## 94  1954  November      29      55     222      19      53     220
    ## 95  1954   October      29      55     222      18      53     220
    ## 96  1954 September      29      55     222      18      53     220
    ## 97  1955     April      21      47     204      26      48     237
    ## 98  1955    August      21      47     204      26      48     237
    ## 99  1955  December      21      47     204      26      48     237
    ## 100 1955  February      21      47     204      26      48     237
    ## 101 1955   January      21      47     204      26      48     237
    ## 102 1955      July      21      47     204      26      48     237
    ## 103 1955      June      21      47     204      26      48     237
    ## 104 1955     March      21      47     204      26      48     237
    ## 105 1955       May      21      47     204      26      48     237
    ## 106 1955  November      21      47     204      26      48     237
    ## 107 1955   October      21      47     204      26      48     237
    ## 108 1955 September      21      47     204      26      48     237
    ## 109 1956     April      21      49     204      26      50     237
    ## 110 1956    August      21      49     204      26      50     237
    ## 111 1956  December      21      49     204      26      51     237
    ## 112 1956  February      21      49     204      26      51     237
    ## 113 1956   January      21      49     204      26      51     237
    ## 114 1956      July      21      49     204      26      50     237
    ## 115 1956      June      21      49     204      26      50     237
    ## 116 1956     March      21      49     204      26      51     237
    ## 117 1956       May      21      49     204      26      50     237
    ## 118 1956  November      21      49     204      26      51     237
    ## 119 1956   October      21      49     204      26      50     237
    ## 120 1956 September      21      49     204      26      50     237
    ## 121 1957     April      19      47     203      28      52     242
    ## 122 1957    August      19      47     203      28      52     242
    ## 123 1957  December      20      47     203      28      52     242
    ## 124 1957  February      19      47     203      28      52     242
    ## 125 1957   January      19      47     203      28      52     242
    ## 126 1957      July      19      47     203      28      52     242
    ## 127 1957      June      19      47     203      28      52     242
    ## 128 1957     March      19      47     203      28      52     242
    ## 129 1957       May      19      47     203      28      52     242
    ## 130 1957  November      20      47     203      28      52     242
    ## 131 1957   October      20      47     203      28      52     242
    ## 132 1957 September      20      47     203      28      52     242
    ## 133 1958     April      20      47     203      28      52     242
    ## 134 1958    August      20      47     203      28      52     242
    ## 135 1958  December      20      47     203      28      52     242
    ## 136 1958  February      20      47     203      28      52     242
    ## 137 1958   January      20      47     203      28      52     242
    ## 138 1958      July      20      47     203      28      52     242
    ## 139 1958      June      20      47     203      28      52     242
    ## 140 1958     March      20      47     203      28      52     242
    ## 141 1958       May      20      47     203      28      52     242
    ## 142 1958  November      20      47     203      28      52     242
    ## 143 1958   October      20      47     203      28      52     242
    ## 144 1958 September      20      47     203      28      52     242
    ## 145 1959     April      15      35     159      35      65     289
    ## 146 1959    August      15      35     159      35      65     289
    ## 147 1959  December      15      35     159      35      65     289
    ## 148 1959  February      15      35     159      35      65     289
    ## 149 1959   January      15      35     159      35      65     289
    ## 150 1959      July      15      35     159      35      65     289
    ## 151 1959      June      15      35     159      35      65     289
    ## 152 1959     March      15      35     159      35      65     289
    ## 153 1959       May      15      35     159      35      65     289
    ## 154 1959  November      15      35     159      35      65     289
    ## 155 1959   October      15      35     159      35      65     289
    ## 156 1959 September      15      35     159      35      65     289
    ## 157 1960     April      16      35     159      34      70     289
    ## 158 1960    August      16      35     159      34      70     289
    ## 159 1960  December      17      35     159      34      70     289
    ## 160 1960  February      16      35     159      34      70     289
    ## 161 1960   January      16      35     159      34      70     289
    ## 162 1960      July      16      35     159      34      70     289
    ## 163 1960      June      16      35     159      34      70     289
    ## 164 1960     March      16      35     159      34      70     289
    ## 165 1960       May      16      35     159      34      70     289
    ## 166 1960  November      17      35     159      34      70     289
    ## 167 1960   October      17      35     159      34      70     289
    ## 168 1960 September      17      35     159      34      70     289
    ## 169 1961     April      16      37     176      34      64     273
    ## 170 1961    August      16      37     176      34      64     273
    ## 171 1961  December      16      37     176      34      64     273
    ## 172 1961  February      16      37     176      34      64     273
    ## 173 1961   January      16      37     176      34      64     273
    ## 174 1961      July      16      37     176      34      64     273
    ## 175 1961      June      16      37     176      34      64     273
    ## 176 1961     March      16      37     176      34      64     273
    ## 177 1961       May      16      37     176      34      64     273
    ## 178 1961  November      16      37     176      34      64     273
    ## 179 1961   October      16      37     176      34      64     273
    ## 180 1961 September      16      37     176      34      64     273
    ## 181 1962     April      16      42     176      34      65     273
    ## 182 1962    August      16      42     176      34      65     273
    ## 183 1962  December      16      42     176      34      65     273
    ## 184 1962  February      16      42     176      34      65     273
    ## 185 1962   January      16      42     176      34      65     273
    ## 186 1962      July      16      42     176      34      65     273
    ## 187 1962      June      16      42     176      34      65     273
    ## 188 1962     March      16      42     176      34      65     273
    ## 189 1962       May      16      42     176      34      65     273
    ## 190 1962  November      16      42     176      34      65     273
    ## 191 1962   October      16      42     176      34      65     273
    ## 192 1962 September      16      42     176      34      65     273
    ## 193 1963     April      16      34     182      34      68     262
    ## 194 1963    August      16      34     182      34      68     262
    ## 195 1963  December      16      34     182      34      68     262
    ## 196 1963  February      17      34     182      33      68     262
    ## 197 1963   January      17      34     182      33      68     262
    ## 198 1963      July      16      34     182      34      68     262
    ## 199 1963      June      16      34     182      34      68     262
    ## 200 1963     March      16      34     182      34      68     262
    ## 201 1963       May      16      34     182      34      68     262
    ## 202 1963  November      16      34     182      34      68     262
    ## 203 1963   October      16      34     182      34      68     262
    ## 204 1963 September      16      34     182      34      68     262
    ## 205 1964     April      16      34     182      34      71     262
    ## 206 1964    August      16      34     182      34      71     262
    ## 207 1964  December      16      34     182      34      71     262
    ## 208 1964  February      16      34     182      34      71     262
    ## 209 1964   January      16      34     182      34      71     262
    ## 210 1964      July      16      34     182      34      71     262
    ## 211 1964      June      16      34     182      34      71     262
    ## 212 1964     March      16      34     182      34      71     262
    ## 213 1964       May      16      34     182      34      71     262
    ## 214 1964  November      16      34     182      34      71     262
    ## 215 1964   October      16      34     182      34      71     262
    ## 216 1964 September      16      34     182      34      71     262
    ## 217 1965     April      17      32     141      33      69     301
    ## 218 1965    August      17      32     141      33      69     301
    ## 219 1965  December      17      32     141      33      69     301
    ## 220 1965  February      17      32     141      33      69     301
    ## 221 1965   January      17      32     141      33      69     301
    ## 222 1965      July      17      32     141      33      69     301
    ## 223 1965      June      17      32     141      33      69     301
    ## 224 1965     March      17      32     141      33      69     301
    ## 225 1965       May      17      32     141      33      69     301
    ## 226 1965  November      17      32     141      33      69     301
    ## 227 1965   October      17      32     141      33      69     301
    ## 228 1965 September      17      32     141      33      69     301
    ## 229 1966     April      18      33     141      33      70     301
    ## 230 1966    August      18      33     141      33      70     301
    ## 231 1966  December      18      33     141      33      70     301
    ## 232 1966  February      18      33     141      33      70     301
    ## 233 1966   January      18      33     141      33      70     301
    ## 234 1966      July      18      33     141      33      70     301
    ## 235 1966      June      18      33     141      33      70     301
    ## 236 1966     March      18      33     141      33      70     301
    ## 237 1966       May      18      33     141      33      70     301
    ## 238 1966  November      18      33     141      33      70     301
    ## 239 1966   October      18      33     141      33      70     301
    ## 240 1966 September      18      33     141      33      70     301
    ## 241 1967     April      25      36     188      27      64     251
    ## 242 1967    August      25      36     188      27      64     251
    ## 243 1967  December      25      36     188      27      64     251
    ## 244 1967  February      25      36     188      27      64     251
    ## 245 1967   January      25      36     188      27      64     251
    ## 246 1967      July      25      36     188      27      64     251
    ## 247 1967      June      25      36     188      27      64     251
    ## 248 1967     March      25      36     188      27      64     251
    ## 249 1967       May      25      36     188      27      64     251
    ## 250 1967  November      25      36     188      27      64     251
    ## 251 1967   October      25      36     188      27      64     251
    ## 252 1967 September      25      36     188      27      64     251
    ## 253 1968     April      26      39     188      26      65     251
    ## 254 1968    August      26      39     188      26      65     251
    ## 255 1968  December      26      39     188      26      65     251
    ## 256 1968  February      26      39     188      26      65     251
    ## 257 1968   January      26      39     188      26      65     251
    ## 258 1968      July      26      39     188      26      65     251
    ## 259 1968      June      26      39     188      26      65     251
    ## 260 1968     March      26      39     188      26      65     251
    ## 261 1968       May      26      39     188      26      65     251
    ## 262 1968  November      26      39     188      26      65     251
    ## 263 1968   October      26      39     188      26      65     251
    ## 264 1968 September      26      39     188      26      65     251
    ## 265 1969     April      31      43     199      22      57     250
    ## 266 1969    August      31      43     199      22      57     250
    ## 267 1969  December      31      43     199      22      57     250
    ## 268 1969  February      31      43     199      22      57     250
    ## 269 1969   January      31      43     199      22      57     250
    ## 270 1969      July      31      43     199      22      57     250
    ## 271 1969      June      31      43     199      22      57     250
    ## 272 1969     March      31      43     199      22      57     250
    ## 273 1969       May      31      43     199      22      57     250
    ## 274 1969  November      31      43     199      22      57     250
    ## 275 1969   October      31      43     199      22      57     250
    ## 276 1969 September      31      43     199      22      57     250
    ## 277 1970     April      32      43     199      20      58     250
    ## 278 1970    August      32      43     199      20      58     250
    ## 279 1970  December      32      43     199      20      58     250
    ## 280 1970  February      32      43     199      20      58     250
    ## 281 1970   January      32      43     199      20      58     250
    ## 282 1970      July      32      43     199      20      58     250
    ## 283 1970      June      32      43     199      20      58     250
    ## 284 1970     March      32      43     199      20      58     250
    ## 285 1970       May      32      43     199      20      58     250
    ## 286 1970  November      32      43     199      20      58     250
    ## 287 1970   October      32      43     199      20      58     250
    ## 288 1970 September      32      43     199      20      58     250
    ## 289 1971     April      21      44     185      30      55     259
    ## 290 1971    August      21      44     185      30      55     259
    ## 291 1971  December      21      44     185      30      55     259
    ## 292 1971  February      21      44     185      30      55     259
    ## 293 1971   January      21      44     185      30      55     259
    ## 294 1971      July      21      44     185      30      55     259
    ## 295 1971      June      21      44     185      30      55     259
    ## 296 1971     March      21      44     185      30      55     259
    ## 297 1971       May      21      44     185      30      55     259
    ## 298 1971  November      21      44     185      30      55     259
    ## 299 1971   October      21      44     185      30      55     259
    ## 300 1971 September      21      44     185      30      55     259
    ## 301 1972     April      20      44     185      31      57     259
    ## 302 1972    August      20      44     185      31      57     259
    ## 303 1972  December      20      44     185      31      57     259
    ## 304 1972  February      20      44     185      31      57     259
    ## 305 1972   January      20      44     185      31      57     259
    ## 306 1972      July      20      44     185      31      57     259
    ## 307 1972      June      20      44     185      31      57     259
    ## 308 1972     March      20      44     185      31      57     259
    ## 309 1972       May      20      44     185      31      57     259
    ## 310 1972  November      20      44     185      31      57     259
    ## 311 1972   October      20      44     185      31      57     259
    ## 312 1972 September      20      44     185      31      57     259
    ## 313 1973     April      19      42     195      32      56     249
    ## 314 1973    August      20      42     195      32      56     249
    ## 315 1973  December      20      42     195      32      56     249
    ## 316 1973  February      19      42     195      32      56     249
    ## 317 1973   January      19      42     195      32      56     249
    ## 318 1973      July      19      42     195      32      56     249
    ## 319 1973      June      19      42     195      32      56     249
    ## 320 1973     March      19      42     195      32      56     249
    ## 321 1973       May      19      42     195      32      56     249
    ## 322 1973  November      20      42     195      32      56     249
    ## 323 1973   October      20      42     195      32      56     249
    ## 324 1973 September      20      42     195      32      56     249
    ## 325 1974     April      18      45     195      34      59     249
    ## 326 1974  February      18      45     195      34      59     249
    ## 327 1974   January      18      45     195      34      59     249
    ## 328 1974      July      18      45     195      34      59     249
    ## 329 1974      June      18      45     195      34      59     249
    ## 330 1974     March      18      45     195      34      59     249
    ## 331 1974       May      18      45     195      34      59     249
    ## 332 1975     April      13      38     148      37      61     295
    ## 333 1975    August      13      38     148      37      61     295
    ## 334 1975  December      13      38     148      37      61     295
    ## 335 1975  February      13      38     148      37      61     295
    ## 336 1975   January      13      38     148      37      61     295
    ## 337 1975      July      13      38     148      37      61     295
    ## 338 1975      June      13      38     148      37      61     295
    ## 339 1975     March      13      38     148      37      61     295
    ## 340 1975       May      13      38     148      37      61     295
    ## 341 1975  November      13      38     148      37      61     295
    ## 342 1975   October      13      38     148      37      61     295
    ## 343 1975 September      13      38     148      37      61     295
    ## 344 1976     April      13      39     148      37      65     295
    ## 345 1976    August      13      39     148      37      65     295
    ## 346 1976  December      13      39     148      37      65     295
    ## 347 1976  February      13      39     148      37      65     295
    ## 348 1976   January      13      39     148      37      65     295
    ## 349 1976      July      13      39     148      37      65     295
    ## 350 1976      June      13      39     148      37      65     295
    ## 351 1976     March      13      39     148      37      65     295
    ## 352 1976       May      13      39     148      37      65     295
    ## 353 1976  November      13      39     148      37      65     295
    ## 354 1976   October      13      39     148      37      65     295
    ## 355 1976 September      13      39     148      37      65     295
    ## 356 1977     April      12      38     147      38      62     294
    ## 357 1977    August      12      38     147      40      62     294
    ## 358 1977  December      12      38     147      41      62     294
    ## 359 1977  February      12      38     147      38      62     294
    ## 360 1977   January      12      38     147      38      62     294
    ## 361 1977      July      12      38     147      40      62     294
    ## 362 1977      June      12      38     147      39      62     294
    ## 363 1977     March      12      38     147      38      62     294
    ## 364 1977       May      12      38     147      38      62     294
    ## 365 1977  November      12      38     147      41      62     294
    ## 366 1977   October      12      38     147      40      62     294
    ## 367 1977 September      12      38     147      40      62     294
    ## 368 1978     April      12      41     147      38      66     294
    ## 369 1978    August      12      41     147      39      66     294
    ## 370 1978  December      12      41     147      39      66     294
    ## 371 1978  February      12      41     147      38      66     294
    ## 372 1978   January      12      41     147      38      66     294
    ## 373 1978      July      12      41     147      38      66     294
    ## 374 1978      June      12      41     147      38      66     294
    ## 375 1978     March      12      41     147      38      66     294
    ## 376 1978       May      12      41     147      38      66     294
    ## 377 1978  November      12      41     147      39      66     294
    ## 378 1978   October      12      41     147      39      66     294
    ## 379 1978 September      12      41     147      39      66     294
    ## 380 1979     April      19      41     160      32      58     280
    ## 381 1979    August      19      41     160      32      58     280
    ## 382 1979  December      19      41     160      32      58     280
    ## 383 1979  February      19      41     160      32      58     280
    ## 384 1979   January      19      41     160      32      58     280
    ## 385 1979      July      19      41     160      32      58     280
    ## 386 1979      June      19      41     160      32      58     280
    ## 387 1979     March      19      41     160      32      58     280
    ## 388 1979       May      19      41     160      32      58     280
    ## 389 1979  November      19      41     160      32      58     280
    ## 390 1979   October      19      41     160      32      58     280
    ## 391 1979 September      19      41     160      32      58     280
    ## 392 1980     April      20      42     160      31      59     280
    ## 393 1980    August      20      42     160      31      59     280
    ## 394 1980  December      20      42     160      31      59     280
    ## 395 1980  February      19      42     160      32      59     280
    ## 396 1980   January      19      42     160      32      59     280
    ## 397 1980      July      20      42     160      31      59     280
    ## 398 1980      June      20      42     160      31      59     280
    ## 399 1980     March      20      42     160      31      59     280
    ## 400 1980       May      20      42     160      31      59     280
    ## 401 1980  November      20      42     160      31      59     280
    ## 402 1980   October      20      42     160      31      59     280
    ## 403 1980 September      20      42     160      31      59     280
    ## 404 1981     April      24      53     196      27      46     246
    ## 405 1981    August      24      53     196      27      46     246
    ## 406 1981  December      24      53     196      27      46     246
    ## 407 1981  February      24      53     196      27      46     246
    ## 408 1981   January      24      53     196      27      46     246
    ## 409 1981      July      24      53     196      27      46     246
    ## 410 1981      June      24      53     196      27      46     246
    ## 411 1981     March      24      53     196      27      46     246
    ## 412 1981       May      24      53     196      27      46     246
    ## 413 1981  November      24      53     196      27      46     246
    ## 414 1981   October      24      53     196      27      46     246
    ## 415 1981 September      24      53     196      27      46     246
    ## 416 1982     April      24      54     196      27      47     246
    ## 417 1982    August      24      54     196      27      47     246
    ## 418 1982  December      24      54     196      27      47     246
    ## 419 1982  February      24      54     196      27      47     246
    ## 420 1982   January      24      54     196      27      47     246
    ## 421 1982      July      24      54     196      27      47     246
    ## 422 1982      June      24      54     196      27      47     246
    ## 423 1982     March      24      54     196      27      47     246
    ## 424 1982       May      24      54     196      27      47     246
    ## 425 1982  November      24      54     196      27      47     246
    ## 426 1982   October      24      54     196      27      47     246
    ## 427 1982 September      24      54     196      27      47     246
    ## 428 1983     April      16      55     168      34      46     271
    ## 429 1983    August      16      55     168      34      46     272
    ## 430 1983  December      16      55     168      34      46     272
    ## 431 1983  February      16      55     168      34      46     272
    ## 432 1983   January      16      55     168      34      46     272
    ## 433 1983      July      16      55     168      34      46     272
    ## 434 1983      June      16      55     168      34      46     271
    ## 435 1983     March      16      55     168      34      46     272
    ## 436 1983       May      16      55     168      34      46     271
    ## 437 1983  November      16      55     168      34      46     272
    ## 438 1983   October      16      55     168      34      46     272
    ## 439 1983 September      16      55     168      34      46     272
    ## 440 1984     April      15      55     168      34      45     271
    ## 441 1984    August      15      55     168      35      45     271
    ## 442 1984  December      15      55     168      35      45     271
    ## 443 1984  February      16      55     168      34      45     271
    ## 444 1984   January      16      55     168      34      45     271
    ## 445 1984      July      15      55     168      35      45     271
    ## 446 1984      June      15      55     168      35      45     271
    ## 447 1984     March      15      55     168      34      45     271
    ## 448 1984       May      15      55     168      35      45     271
    ## 449 1984  November      15      55     168      35      45     271
    ## 450 1984   October      15      55     168      35      45     271
    ## 451 1984 September      15      55     168      35      45     271
    ## 452 1985     April      16      53     183      34      47     257
    ## 453 1985    August      16      53     183      34      47     257
    ## 454 1985  December      16      53     183      34      47     257
    ## 455 1985  February      16      53     183      34      47     257
    ## 456 1985   January      16      53     183      34      47     257
    ## 457 1985      July      16      53     183      34      47     257
    ## 458 1985      June      16      53     183      34      47     257
    ## 459 1985     March      16      53     183      34      47     257
    ## 460 1985       May      16      53     183      34      47     257
    ## 461 1985  November      16      53     183      34      47     257
    ## 462 1985   October      16      53     183      34      47     257
    ## 463 1985 September      16      53     183      34      47     257
    ## 464 1986     April      16      53     183      34      48     257
    ## 465 1986    August      16      54     183      34      48     257
    ## 466 1986  December      16      54     183      34      48     257
    ## 467 1986  February      16      53     183      34      48     257
    ## 468 1986   January      16      53     183      34      48     257
    ## 469 1986      July      16      54     183      34      48     257
    ## 470 1986      June      16      53     183      34      48     257
    ## 471 1986     March      16      53     183      34      48     257
    ## 472 1986       May      16      53     183      34      48     257
    ## 473 1986  November      16      54     183      34      48     257
    ## 474 1986   October      16      54     183      34      48     257
    ## 475 1986 September      16      54     183      34      48     257
    ## 476 1987     April      24      46     179      27      55     262
    ## 477 1987    August      24      46     179      27      55     262
    ## 478 1987  December      24      46     179      27      55     262
    ## 479 1987  February      24      46     179      27      55     262
    ## 480 1987   January      24      46     179      27      55     262
    ## 481 1987      July      24      46     179      27      55     262
    ## 482 1987      June      24      46     179      27      55     262
    ## 483 1987     March      24      46     179      27      55     262
    ## 484 1987       May      24      46     179      27      55     262
    ## 485 1987  November      24      46     179      27      55     262
    ## 486 1987   October      24      46     179      27      55     262
    ## 487 1987 September      24      46     179      27      55     262
    ## 488 1988     April      24      46     179      28      54     262
    ## 489 1988    August      24      46     178      28      54     262
    ## 490 1988  December      24      46     179      28      54     262
    ## 491 1988  February      24      46     179      28      54     262
    ## 492 1988   January      24      46     179      28      54     262
    ## 493 1988      July      24      46     178      28      54     262
    ## 494 1988      June      24      46     179      28      54     262
    ## 495 1988     March      25      46     179      28      54     262
    ## 496 1988       May      24      46     179      28      54     262
    ## 497 1988  November      24      46     179      28      54     262
    ## 498 1988   October      24      46     178      28      54     262
    ## 499 1988 September      24      46     178      28      54     262
    ## 500 1989     April      23      46     178      29      55     264
    ## 501 1989    August      23      46     178      29      55     261
    ## 502 1989  December      23      46     178      29      55     261
    ## 503 1989  February      23      46     178      29      55     264
    ## 504 1989   January      23      46     178      29      55     264
    ## 505 1989      July      23      46     178      29      55     261
    ## 506 1989      June      23      46     178      29      55     262
    ## 507 1989     March      23      46     178      29      55     264
    ## 508 1989       May      23      46     178      29      55     264
    ## 509 1989  November      23      46     178      29      55     261
    ## 510 1989   October      23      46     178      29      55     261
    ## 511 1989 September      23      46     178      29      55     261
    ## 512 1990     April      22      46     176      30      55     258
    ## 513 1990    August      22      46     176      30      56     257
    ## 514 1990  December      22      46     176      30      56     259
    ## 515 1990  February      22      46     176      30      55     258
    ## 516 1990   January      22      46     176      30      55     258
    ## 517 1990      July      22      46     176      30      56     257
    ## 518 1990      June      22      46     176      30      56     257
    ## 519 1990     March      22      46     176      30      55     258
    ## 520 1990       May      22      46     176      30      55     257
    ## 521 1990  November      22      46     176      30      56     259
    ## 522 1990   October      22      46     176      30      56     258
    ## 523 1990 September      22      46     176      30      56     257
    ## 524 1991     April      21      45     166      28      57     269
    ## 525 1991    August      21      45     166      29      57     268
    ## 526 1991  December      21      45     167      29      57     269
    ## 527 1991  February      20      45     168      29      57     269
    ## 528 1991   January      20      45     168      29      57     269
    ## 529 1991      July      21      45     166      28      57     268
    ## 530 1991      June      21      45     166      28      57     268
    ## 531 1991     March      21      45     166      28      57     269
    ## 532 1991       May      21      45     166      28      57     268
    ## 533 1991  November      21      45     167      29      57     269
    ## 534 1991   October      21      45     166      29      57     268
    ## 535 1991 September      21      45     166      29      57     268
    ## 536 1992     April      20      43     166      28      58     268
    ## 537 1992    August      20      43     166      28      58     268
    ## 538 1992  December      20      43     166      28      60     270
    ## 539 1992  February      20      43     166      27      58     268
    ## 540 1992   January      20      43     166      27      58     268
    ## 541 1992      July      20      43     166      28      58     268
    ## 542 1992      June      20      43     166      28      58     268
    ## 543 1992     March      20      43     166      28      58     268
    ## 544 1992       May      20      43     166      28      58     268
    ## 545 1992  November      20      43     166      28      60     270
    ## 546 1992   October      20      43     166      28      59     268
    ## 547 1992 September      20      43     166      28      58     268
    ## 548 1993     April      18      46     175      30      57     256
    ## 549 1993    August      17      46     178      31      57     258
    ## 550 1993  December      17      46     178      31      57     258
    ## 551 1993  February      18      46     175      30      57     255
    ## 552 1993   January      18      46     175      30      57     255
    ## 553 1993      July      17      46     178      31      57     258
    ## 554 1993      June      17      46     178      31      57     258
    ## 555 1993     March      18      46     175      30      57     255
    ## 556 1993       May      17      46     177      31      57     257
    ## 557 1993  November      17      46     178      31      57     258
    ## 558 1993   October      17      46     178      31      57     258
    ## 559 1993 September      17      46     178      31      57     258
    ## 560 1994     April      19      48     178      28      54     256
    ## 561 1994    August      19      48     178      28      54     256
    ## 562 1994  December      19      48     178      28      54     256
    ## 563 1994  February      19      48     178      28      54     257
    ## 564 1994   January      19      48     178      28      54     257
    ## 565 1994      July      19      48     178      28      54     256
    ## 566 1994      June      19      48     178      28      54     256
    ## 567 1994     March      19      48     178      28      54     257
    ## 568 1994       May      19      48     178      28      54     256
    ## 569 1994  November      19      48     178      28      54     256
    ## 570 1994   October      19      48     178      28      54     256
    ## 571 1994 September      19      48     178      28      54     256
    ## 572 1995     April      30      54     235      20      46     199
    ## 573 1995    August      30      54     235      20      46     199
    ## 574 1995  December      30      54     235      20      46     199
    ## 575 1995  February      30      54     235      20      46     199
    ## 576 1995   January      30      54     235      20      46     199
    ## 577 1995      July      30      54     235      20      46     199
    ## 578 1995      June      30      54     235      20      46     199
    ## 579 1995     March      30      54     235      20      46     199
    ## 580 1995       May      30      54     235      20      46     199
    ## 581 1995  November      30      54     235      20      46     199
    ## 582 1995   October      30      54     235      20      46     199
    ## 583 1995 September      30      54     235      20      46     199
    ## 584 1996     April      31      54     236      19      47     196
    ## 585 1996    August      32      54     235      19      47     198
    ## 586 1996  December      32      55     235      19      47     198
    ## 587 1996  February      31      54     236      19      47     196
    ## 588 1996   January      31      54     236      19      47     196
    ## 589 1996      July      32      54     235      19      47     198
    ## 590 1996      June      31      54     236      19      47     198
    ## 591 1996     March      31      54     236      19      47     195
    ## 592 1996       May      31      54     236      19      47     197
    ## 593 1996  November      32      55     235      19      47     198
    ## 594 1996   October      32      54     235      19      47     198
    ## 595 1996 September      32      54     235      19      47     198
    ## 596 1997     April      32      55     227      18      45     207
    ## 597 1997    August      33      55     228      18      45     207
    ## 598 1997  December      34      55     229      18      45     207
    ## 599 1997  February      32      55     227      18      45     207
    ## 600 1997   January      32      55     227      18      45     207
    ## 601 1997      July      32      55     228      18      45     207
    ## 602 1997      June      32      55     228      18      45     207
    ## 603 1997     March      32      55     227      18      45     207
    ## 604 1997       May      32      55     228      18      45     207
    ## 605 1997  November      34      55     229      18      45     207
    ## 606 1997   October      34      55     228      18      45     207
    ## 607 1997 September      34      55     228      18      45     207
    ## 608 1998     April      32      55     227      18      45     205
    ## 609 1998    August      32      55     228      18      45     206
    ## 610 1998  December      32      55     228      18      45     206
    ## 611 1998  February      32      55     227      18      45     203
    ## 612 1998   January      32      55     227      18      45     203
    ## 613 1998      July      32      55     228      18      45     206
    ## 614 1998      June      32      55     227      18      45     206
    ## 615 1998     March      32      55     227      18      45     204
    ## 616 1998       May      32      55     227      18      45     205
    ## 617 1998  November      32      55     228      18      45     206
    ## 618 1998   October      32      55     228      18      45     206
    ## 619 1998 September      32      55     228      18      45     206
    ## 620 1999     April      31      55     222      17      45     210
    ## 621 1999    August      31      55     223      17      45     210
    ## 622 1999  December      31      56     223      17      45     211
    ## 623 1999  February      31      55     223      17      45     210
    ## 624 1999   January      31      55     223      17      45     210
    ## 625 1999      July      31      55     223      17      45     210
    ## 626 1999      June      31      55     223      17      45     210
    ## 627 1999     March      31      55     222      17      45     210
    ## 628 1999       May      31      55     222      17      45     210
    ## 629 1999  November      31      56     223      17      45     211
    ## 630 1999   October      31      55     223      17      45     210
    ## 631 1999 September      31      55     223      17      45     210
    ## 632 2000     April      30      55     223      18      45     210
    ## 633 2000    August      30      55     223      18      46     210
    ## 634 2000  December      30      55     223      19      46     210
    ## 635 2000  February      30      55     223      18      45     210
    ## 636 2000   January      30      55     223      18      45     210
    ## 637 2000      July      30      55     223      18      45     210
    ## 638 2000      June      30      55     223      18      45     210
    ## 639 2000     March      30      55     223      18      45     210
    ## 640 2000       May      30      55     223      18      45     210
    ## 641 2000  November      30      55     223      19      46     210
    ## 642 2000   October      30      55     223      18      46     210
    ## 643 2000 September      30      55     223      18      46     210
    ## 644 2001     April      29      49     220      19      50     210
    ## 645 2001    August      29      49     222      19      50     210
    ## 646 2001  December      30      49     223      19      50     211
    ## 647 2001  February      29      49     220      19      50     211
    ## 648 2001   January      29      49     220      19      50     211
    ## 649 2001      July      29      49     222      19      50     210
    ## 650 2001      June      29      49     221      19      50     210
    ## 651 2001     March      29      49     220      19      50     211
    ## 652 2001       May      29      49     220      19      50     210
    ## 653 2001  November      30      49     223      19      50     211
    ## 654 2001   October      30      49     222      19      50     210
    ## 655 2001 September      29      49     222      19      50     210
    ## 656 2002     April      27      50     222      21      50     211
    ## 657 2002    August      27      50     222      21      50     211
    ## 658 2002  December      27      50     222      21      50     211
    ## 659 2002  February      27      50     221      21      50     211
    ## 660 2002   January      27      50     221      21      50     211
    ## 661 2002      July      27      50     222      21      50     211
    ## 662 2002      June      27      50     222      21      50     211
    ## 663 2002     March      27      50     222      21      50     211
    ## 664 2002       May      27      50     222      21      50     211
    ## 665 2002  November      27      50     222      21      50     211
    ## 666 2002   October      27      50     222      21      50     211
    ## 667 2002 September      27      50     222      21      50     211
    ## 668 2003     April      26      51     231      24      48     203
    ## 669 2003    August      26      51     231      24      48     203
    ## 670 2003  December      27      51     231      25      48     203
    ## 671 2003  February      26      51     231      24      48     203
    ## 672 2003   January      26      51     231      24      48     203
    ## 673 2003      July      26      51     231      24      48     203
    ## 674 2003      June      26      51     231      24      48     203
    ## 675 2003     March      26      51     231      24      48     203
    ## 676 2003       May      26      51     231      24      48     203
    ## 677 2003  November      27      51     231      25      48     203
    ## 678 2003   October      26      51     231      25      48     203
    ## 679 2003 September      26      51     231      25      48     203
    ## 680 2004     April      28      51     229      22      48     204
    ## 681 2004    August      29      51     229      22      48     205
    ## 682 2004  December      29      51     229      23      48     205
    ## 683 2004  February      28      51     229      22      48     203
    ## 684 2004   January      28      51     229      22      48     203
    ## 685 2004      July      29      51     229      22      48     204
    ## 686 2004      June      28      51     229      22      48     204
    ## 687 2004     March      28      51     229      22      48     204
    ## 688 2004       May      28      51     229      22      48     204
    ## 689 2004  November      29      51     229      23      48     205
    ## 690 2004   October      29      51     229      22      48     205
    ## 691 2004 September      29      51     229      22      48     205
    ## 692 2005     April      28      54     232      22      45     202
    ## 693 2005    August      28      54     231      22      45     202
    ## 694 2005  December      28      54     232      22      45     202
    ## 695 2005  February      28      54     232      22      45     201
    ## 696 2005   January      28      54     232      22      45     201
    ## 697 2005      July      28      54     231      22      45     202
    ## 698 2005      June      28      54     231      22      45     202
    ## 699 2005     March      28      54     232      22      45     202
    ## 700 2005       May      28      54     231      22      45     202
    ## 701 2005  November      28      54     232      22      45     202
    ## 702 2005   October      28      54     232      22      45     202
    ## 703 2005 September      28      54     232      22      45     202
    ## 704 2006     April      28      54     231      22      45     201
    ## 705 2006    August      28      54     231      22      45     201
    ## 706 2006  December      28      54     232      22      45     202
    ## 707 2006  February      28      54     231      22      45     201
    ## 708 2006   January      28      54     231      22      45     201
    ## 709 2006      July      28      54     231      22      45     201
    ## 710 2006      June      28      54     231      22      45     201
    ## 711 2006     March      28      54     231      22      45     201
    ## 712 2006       May      28      54     231      22      45     201
    ## 713 2006  November      28      54     232      22      45     202
    ## 714 2006   October      28      54     231      22      45     201
    ## 715 2006 September      28      54     231      22      45     201
    ## 716 2007     April      22      48     201      28      50     233
    ## 717 2007    August      22      48     202      28      50     232
    ## 718 2007  December      22      48     202      28      50     234
    ## 719 2007  February      22      48     201      28      50     233
    ## 720 2007   January      22      48     201      28      50     233
    ## 721 2007      July      22      48     201      28      50     232
    ## 722 2007      June      22      47     201      28      50     232
    ## 723 2007     March      22      48     201      28      50     233
    ## 724 2007       May      22      48     201      28      50     232
    ## 725 2007  November      22      48     202      28      50     234
    ## 726 2007   October      22      48     202      28      50     233
    ## 727 2007 September      22      48     202      28      50     233
    ## 728 2008     April      22      48     198      28      50     234
    ## 729 2008    August      22      48     199      28      50     236
    ## 730 2008  December      22      48     199      28      50     236
    ## 731 2008  February      22      48     198      28      50     231
    ## 732 2008   January      22      48     198      28      50     231
    ## 733 2008      July      22      48     199      28      50     236
    ## 734 2008      June      22      48     199      28      50     235
    ## 735 2008     March      22      48     198      28      50     233
    ## 736 2008       May      22      48     199      28      50     235
    ## 737 2008  November      22      48     199      28      50     236
    ## 738 2008   October      22      48     199      28      50     236
    ## 739 2008 September      22      48     199      28      50     236
    ## 740 2009     April      22      40     179      28      57     254
    ## 741 2009    August      24      40     179      28      58     255
    ## 742 2009  December      24      41     179      28      59     257
    ## 743 2009  February      22      40     179      28      57     254
    ## 744 2009   January      22      40     179      28      57     254
    ## 745 2009      July      22      40     179      28      58     254
    ## 746 2009      June      22      40     179      28      57     255
    ## 747 2009     March      22      40     179      28      57     253
    ## 748 2009       May      22      40     179      28      57     255
    ## 749 2009  November      24      41     179      28      59     257
    ## 750 2009   October      24      41     179      28      59     255
    ## 751 2009 September      24      41     179      28      58     255
    ## 752 2010     April      24      41     177      26      57     254
    ## 753 2010    August      24      41     178      26      57     255
    ## 754 2010  December      24      41     178      27      59     255
    ## 755 2010  February      24      41     178      26      57     255
    ## 756 2010   January      24      41     178      26      57     255
    ## 757 2010      July      24      41     178      26      56     255
    ## 758 2010      June      24      41     178      26      57     255
    ## 759 2010     March      24      41     178      26      57     253
    ## 760 2010       May      24      41     177      26      57     254
    ## 761 2010  November      24      41     178      27      59     255
    ## 762 2010   October      24      41     178      26      57     255
    ## 763 2010 September      24      41     178      26      57     255
    ## 764 2011     April      29      47     241      21      51     192
    ## 765 2011    August      29      47     240      21      51     193
    ## 766 2011  December      29      47     242      21      51     193
    ## 767 2011  February      29      47     241      21      51     193
    ## 768 2011   January      29      47     241      21      51     193
    ## 769 2011      July      29      47     240      21      51     192
    ## 770 2011      June      29      47     240      21      51     193
    ## 771 2011     March      29      47     241      21      51     192
    ## 772 2011       May      29      47     240      21      51     192
    ## 773 2011  November      29      47     242      21      51     193
    ## 774 2011   October      29      47     242      21      51     193
    ## 775 2011 September      29      47     242      21      51     193
    ## 776 2012     April      29      47     242      21      51     190
    ## 777 2012    August      29      47     242      21      51     191
    ## 778 2012  December      29      47     243      21      51     194
    ## 779 2012  February      29      47     242      21      51     192
    ## 780 2012   January      29      47     242      21      51     192
    ## 781 2012      July      29      47     242      21      51     191
    ## 782 2012      June      29      47     242      21      51     191
    ## 783 2012     March      29      47     242      21      51     191
    ## 784 2012       May      29      47     242      21      51     190
    ## 785 2012  November      29      47     243      21      51     194
    ## 786 2012   October      29      47     242      21      51     191
    ## 787 2012 September      29      47     242      21      51     191
    ## 788 2013     April      30      45     232      20      53     201
    ## 789 2013    August      30      46     234      20      53     201
    ## 790 2013  December      30      46     234      20      54     201
    ## 791 2013  February      30      45     232      20      53     200
    ## 792 2013   January      30      45     232      20      53     200
    ## 793 2013      July      30      46     234      20      52     201
    ## 794 2013      June      30      46     234      20      52     201
    ## 795 2013     March      30      45     232      20      53     200
    ## 796 2013       May      30      45     233      20      53     201
    ## 797 2013  November      30      46     234      20      54     201
    ## 798 2013   October      30      46     234      20      53     201
    ## 799 2013 September      30      46     234      20      53     201
    ## 800 2014     April      29      45     233      21      53     199
    ## 801 2014    August      29      45     234      21      53     199
    ## 802 2014  December      29      45     235      21      53     201
    ## 803 2014  February      29      45     232      21      53     200
    ## 804 2014   January      29      45     232      21      53     200
    ## 805 2014      July      29      45     234      21      53     199
    ## 806 2014      June      29      45     233      21      53     199
    ## 807 2014     March      29      45     233      21      53     199
    ## 808 2014       May      29      45     233      21      53     199
    ## 809 2014  November      29      45     235      21      53     201
    ## 810 2014   October      29      45     234      21      53     199
    ## 811 2014 September      29      45     234      21      53     199
    ## 812 2015     April      31      54     244      18      44     188
    ## 813 2015  February      31      54     245      18      44     188
    ## 814 2015   January      31      54     245      18      44     188
    ## 815 2015      June      31      54     246      18      44     188
    ## 816 2015     March      31      54     245      18      44     188
    ## 817 2015       May      31      54     245      18      44     188
    ##     president
    ## 1         dem
    ## 2         dem
    ## 3         dem
    ## 4         dem
    ## 5         dem
    ## 6         dem
    ## 7         dem
    ## 8         dem
    ## 9         dem
    ## 10        dem
    ## 11        dem
    ## 12        dem
    ## 13        dem
    ## 14        dem
    ## 15        dem
    ## 16        dem
    ## 17        dem
    ## 18        dem
    ## 19        dem
    ## 20        dem
    ## 21        dem
    ## 22        dem
    ## 23        dem
    ## 24        dem
    ## 25        dem
    ## 26        dem
    ## 27        dem
    ## 28        dem
    ## 29        dem
    ## 30        dem
    ## 31        dem
    ## 32        dem
    ## 33        dem
    ## 34        dem
    ## 35        dem
    ## 36        dem
    ## 37        dem
    ## 38        dem
    ## 39        dem
    ## 40        dem
    ## 41        dem
    ## 42        dem
    ## 43        dem
    ## 44        dem
    ## 45        dem
    ## 46        dem
    ## 47        dem
    ## 48        dem
    ## 49        dem
    ## 50        dem
    ## 51        dem
    ## 52        dem
    ## 53        dem
    ## 54        dem
    ## 55        dem
    ## 56        dem
    ## 57        dem
    ## 58        dem
    ## 59        dem
    ## 60        dem
    ## 61        dem
    ## 62        dem
    ## 63        dem
    ## 64        dem
    ## 65        dem
    ## 66        dem
    ## 67        dem
    ## 68        dem
    ## 69        dem
    ## 70        dem
    ## 71        dem
    ## 72        dem
    ## 73        gop
    ## 74        gop
    ## 75        gop
    ## 76        gop
    ## 77        gop
    ## 78        gop
    ## 79        gop
    ## 80        gop
    ## 81        gop
    ## 82        gop
    ## 83        gop
    ## 84        gop
    ## 85        gop
    ## 86        gop
    ## 87        gop
    ## 88        gop
    ## 89        gop
    ## 90        gop
    ## 91        gop
    ## 92        gop
    ## 93        gop
    ## 94        gop
    ## 95        gop
    ## 96        gop
    ## 97        gop
    ## 98        gop
    ## 99        gop
    ## 100       gop
    ## 101       gop
    ## 102       gop
    ## 103       gop
    ## 104       gop
    ## 105       gop
    ## 106       gop
    ## 107       gop
    ## 108       gop
    ## 109       gop
    ## 110       gop
    ## 111       gop
    ## 112       gop
    ## 113       gop
    ## 114       gop
    ## 115       gop
    ## 116       gop
    ## 117       gop
    ## 118       gop
    ## 119       gop
    ## 120       gop
    ## 121       gop
    ## 122       gop
    ## 123       gop
    ## 124       gop
    ## 125       gop
    ## 126       gop
    ## 127       gop
    ## 128       gop
    ## 129       gop
    ## 130       gop
    ## 131       gop
    ## 132       gop
    ## 133       gop
    ## 134       gop
    ## 135       gop
    ## 136       gop
    ## 137       gop
    ## 138       gop
    ## 139       gop
    ## 140       gop
    ## 141       gop
    ## 142       gop
    ## 143       gop
    ## 144       gop
    ## 145       gop
    ## 146       gop
    ## 147       gop
    ## 148       gop
    ## 149       gop
    ## 150       gop
    ## 151       gop
    ## 152       gop
    ## 153       gop
    ## 154       gop
    ## 155       gop
    ## 156       gop
    ## 157       gop
    ## 158       gop
    ## 159       gop
    ## 160       gop
    ## 161       gop
    ## 162       gop
    ## 163       gop
    ## 164       gop
    ## 165       gop
    ## 166       gop
    ## 167       gop
    ## 168       gop
    ## 169       dem
    ## 170       dem
    ## 171       dem
    ## 172       dem
    ## 173       dem
    ## 174       dem
    ## 175       dem
    ## 176       dem
    ## 177       dem
    ## 178       dem
    ## 179       dem
    ## 180       dem
    ## 181       dem
    ## 182       dem
    ## 183       dem
    ## 184       dem
    ## 185       dem
    ## 186       dem
    ## 187       dem
    ## 188       dem
    ## 189       dem
    ## 190       dem
    ## 191       dem
    ## 192       dem
    ## 193       dem
    ## 194       dem
    ## 195       dem
    ## 196       dem
    ## 197       dem
    ## 198       dem
    ## 199       dem
    ## 200       dem
    ## 201       dem
    ## 202       dem
    ## 203       dem
    ## 204       dem
    ## 205       dem
    ## 206       dem
    ## 207       dem
    ## 208       dem
    ## 209       dem
    ## 210       dem
    ## 211       dem
    ## 212       dem
    ## 213       dem
    ## 214       dem
    ## 215       dem
    ## 216       dem
    ## 217       dem
    ## 218       dem
    ## 219       dem
    ## 220       dem
    ## 221       dem
    ## 222       dem
    ## 223       dem
    ## 224       dem
    ## 225       dem
    ## 226       dem
    ## 227       dem
    ## 228       dem
    ## 229       dem
    ## 230       dem
    ## 231       dem
    ## 232       dem
    ## 233       dem
    ## 234       dem
    ## 235       dem
    ## 236       dem
    ## 237       dem
    ## 238       dem
    ## 239       dem
    ## 240       dem
    ## 241       dem
    ## 242       dem
    ## 243       dem
    ## 244       dem
    ## 245       dem
    ## 246       dem
    ## 247       dem
    ## 248       dem
    ## 249       dem
    ## 250       dem
    ## 251       dem
    ## 252       dem
    ## 253       dem
    ## 254       dem
    ## 255       dem
    ## 256       dem
    ## 257       dem
    ## 258       dem
    ## 259       dem
    ## 260       dem
    ## 261       dem
    ## 262       dem
    ## 263       dem
    ## 264       dem
    ## 265       gop
    ## 266       gop
    ## 267       gop
    ## 268       gop
    ## 269       gop
    ## 270       gop
    ## 271       gop
    ## 272       gop
    ## 273       gop
    ## 274       gop
    ## 275       gop
    ## 276       gop
    ## 277       gop
    ## 278       gop
    ## 279       gop
    ## 280       gop
    ## 281       gop
    ## 282       gop
    ## 283       gop
    ## 284       gop
    ## 285       gop
    ## 286       gop
    ## 287       gop
    ## 288       gop
    ## 289       gop
    ## 290       gop
    ## 291       gop
    ## 292       gop
    ## 293       gop
    ## 294       gop
    ## 295       gop
    ## 296       gop
    ## 297       gop
    ## 298       gop
    ## 299       gop
    ## 300       gop
    ## 301       gop
    ## 302       gop
    ## 303       gop
    ## 304       gop
    ## 305       gop
    ## 306       gop
    ## 307       gop
    ## 308       gop
    ## 309       gop
    ## 310       gop
    ## 311       gop
    ## 312       gop
    ## 313       gop
    ## 314       gop
    ## 315       gop
    ## 316       gop
    ## 317       gop
    ## 318       gop
    ## 319       gop
    ## 320       gop
    ## 321       gop
    ## 322       gop
    ## 323       gop
    ## 324       gop
    ## 325       gop
    ## 326       gop
    ## 327       gop
    ## 328       gop
    ## 329       gop
    ## 330       gop
    ## 331       gop
    ## 332       gop
    ## 333       gop
    ## 334       gop
    ## 335       gop
    ## 336       gop
    ## 337       gop
    ## 338       gop
    ## 339       gop
    ## 340       gop
    ## 341       gop
    ## 342       gop
    ## 343       gop
    ## 344       gop
    ## 345       gop
    ## 346       gop
    ## 347       gop
    ## 348       gop
    ## 349       gop
    ## 350       gop
    ## 351       gop
    ## 352       gop
    ## 353       gop
    ## 354       gop
    ## 355       gop
    ## 356       dem
    ## 357       dem
    ## 358       dem
    ## 359       dem
    ## 360       dem
    ## 361       dem
    ## 362       dem
    ## 363       dem
    ## 364       dem
    ## 365       dem
    ## 366       dem
    ## 367       dem
    ## 368       dem
    ## 369       dem
    ## 370       dem
    ## 371       dem
    ## 372       dem
    ## 373       dem
    ## 374       dem
    ## 375       dem
    ## 376       dem
    ## 377       dem
    ## 378       dem
    ## 379       dem
    ## 380       dem
    ## 381       dem
    ## 382       dem
    ## 383       dem
    ## 384       dem
    ## 385       dem
    ## 386       dem
    ## 387       dem
    ## 388       dem
    ## 389       dem
    ## 390       dem
    ## 391       dem
    ## 392       dem
    ## 393       dem
    ## 394       dem
    ## 395       dem
    ## 396       dem
    ## 397       dem
    ## 398       dem
    ## 399       dem
    ## 400       dem
    ## 401       dem
    ## 402       dem
    ## 403       dem
    ## 404       gop
    ## 405       gop
    ## 406       gop
    ## 407       gop
    ## 408       gop
    ## 409       gop
    ## 410       gop
    ## 411       gop
    ## 412       gop
    ## 413       gop
    ## 414       gop
    ## 415       gop
    ## 416       gop
    ## 417       gop
    ## 418       gop
    ## 419       gop
    ## 420       gop
    ## 421       gop
    ## 422       gop
    ## 423       gop
    ## 424       gop
    ## 425       gop
    ## 426       gop
    ## 427       gop
    ## 428       gop
    ## 429       gop
    ## 430       gop
    ## 431       gop
    ## 432       gop
    ## 433       gop
    ## 434       gop
    ## 435       gop
    ## 436       gop
    ## 437       gop
    ## 438       gop
    ## 439       gop
    ## 440       gop
    ## 441       gop
    ## 442       gop
    ## 443       gop
    ## 444       gop
    ## 445       gop
    ## 446       gop
    ## 447       gop
    ## 448       gop
    ## 449       gop
    ## 450       gop
    ## 451       gop
    ## 452       gop
    ## 453       gop
    ## 454       gop
    ## 455       gop
    ## 456       gop
    ## 457       gop
    ## 458       gop
    ## 459       gop
    ## 460       gop
    ## 461       gop
    ## 462       gop
    ## 463       gop
    ## 464       gop
    ## 465       gop
    ## 466       gop
    ## 467       gop
    ## 468       gop
    ## 469       gop
    ## 470       gop
    ## 471       gop
    ## 472       gop
    ## 473       gop
    ## 474       gop
    ## 475       gop
    ## 476       gop
    ## 477       gop
    ## 478       gop
    ## 479       gop
    ## 480       gop
    ## 481       gop
    ## 482       gop
    ## 483       gop
    ## 484       gop
    ## 485       gop
    ## 486       gop
    ## 487       gop
    ## 488       gop
    ## 489       gop
    ## 490       gop
    ## 491       gop
    ## 492       gop
    ## 493       gop
    ## 494       gop
    ## 495       gop
    ## 496       gop
    ## 497       gop
    ## 498       gop
    ## 499       gop
    ## 500       gop
    ## 501       gop
    ## 502       gop
    ## 503       gop
    ## 504       gop
    ## 505       gop
    ## 506       gop
    ## 507       gop
    ## 508       gop
    ## 509       gop
    ## 510       gop
    ## 511       gop
    ## 512       gop
    ## 513       gop
    ## 514       gop
    ## 515       gop
    ## 516       gop
    ## 517       gop
    ## 518       gop
    ## 519       gop
    ## 520       gop
    ## 521       gop
    ## 522       gop
    ## 523       gop
    ## 524       gop
    ## 525       gop
    ## 526       gop
    ## 527       gop
    ## 528       gop
    ## 529       gop
    ## 530       gop
    ## 531       gop
    ## 532       gop
    ## 533       gop
    ## 534       gop
    ## 535       gop
    ## 536       gop
    ## 537       gop
    ## 538       gop
    ## 539       gop
    ## 540       gop
    ## 541       gop
    ## 542       gop
    ## 543       gop
    ## 544       gop
    ## 545       gop
    ## 546       gop
    ## 547       gop
    ## 548       dem
    ## 549       dem
    ## 550       dem
    ## 551       dem
    ## 552       dem
    ## 553       dem
    ## 554       dem
    ## 555       dem
    ## 556       dem
    ## 557       dem
    ## 558       dem
    ## 559       dem
    ## 560       dem
    ## 561       dem
    ## 562       dem
    ## 563       dem
    ## 564       dem
    ## 565       dem
    ## 566       dem
    ## 567       dem
    ## 568       dem
    ## 569       dem
    ## 570       dem
    ## 571       dem
    ## 572       dem
    ## 573       dem
    ## 574       dem
    ## 575       dem
    ## 576       dem
    ## 577       dem
    ## 578       dem
    ## 579       dem
    ## 580       dem
    ## 581       dem
    ## 582       dem
    ## 583       dem
    ## 584       dem
    ## 585       dem
    ## 586       dem
    ## 587       dem
    ## 588       dem
    ## 589       dem
    ## 590       dem
    ## 591       dem
    ## 592       dem
    ## 593       dem
    ## 594       dem
    ## 595       dem
    ## 596       dem
    ## 597       dem
    ## 598       dem
    ## 599       dem
    ## 600       dem
    ## 601       dem
    ## 602       dem
    ## 603       dem
    ## 604       dem
    ## 605       dem
    ## 606       dem
    ## 607       dem
    ## 608       dem
    ## 609       dem
    ## 610       dem
    ## 611       dem
    ## 612       dem
    ## 613       dem
    ## 614       dem
    ## 615       dem
    ## 616       dem
    ## 617       dem
    ## 618       dem
    ## 619       dem
    ## 620       dem
    ## 621       dem
    ## 622       dem
    ## 623       dem
    ## 624       dem
    ## 625       dem
    ## 626       dem
    ## 627       dem
    ## 628       dem
    ## 629       dem
    ## 630       dem
    ## 631       dem
    ## 632       dem
    ## 633       dem
    ## 634       dem
    ## 635       dem
    ## 636       dem
    ## 637       dem
    ## 638       dem
    ## 639       dem
    ## 640       dem
    ## 641       dem
    ## 642       dem
    ## 643       dem
    ## 644       gop
    ## 645       gop
    ## 646       gop
    ## 647       gop
    ## 648       gop
    ## 649       gop
    ## 650       gop
    ## 651       gop
    ## 652       gop
    ## 653       gop
    ## 654       gop
    ## 655       gop
    ## 656       gop
    ## 657       gop
    ## 658       gop
    ## 659       gop
    ## 660       gop
    ## 661       gop
    ## 662       gop
    ## 663       gop
    ## 664       gop
    ## 665       gop
    ## 666       gop
    ## 667       gop
    ## 668       gop
    ## 669       gop
    ## 670       gop
    ## 671       gop
    ## 672       gop
    ## 673       gop
    ## 674       gop
    ## 675       gop
    ## 676       gop
    ## 677       gop
    ## 678       gop
    ## 679       gop
    ## 680       gop
    ## 681       gop
    ## 682       gop
    ## 683       gop
    ## 684       gop
    ## 685       gop
    ## 686       gop
    ## 687       gop
    ## 688       gop
    ## 689       gop
    ## 690       gop
    ## 691       gop
    ## 692       gop
    ## 693       gop
    ## 694       gop
    ## 695       gop
    ## 696       gop
    ## 697       gop
    ## 698       gop
    ## 699       gop
    ## 700       gop
    ## 701       gop
    ## 702       gop
    ## 703       gop
    ## 704       gop
    ## 705       gop
    ## 706       gop
    ## 707       gop
    ## 708       gop
    ## 709       gop
    ## 710       gop
    ## 711       gop
    ## 712       gop
    ## 713       gop
    ## 714       gop
    ## 715       gop
    ## 716       gop
    ## 717       gop
    ## 718       gop
    ## 719       gop
    ## 720       gop
    ## 721       gop
    ## 722       gop
    ## 723       gop
    ## 724       gop
    ## 725       gop
    ## 726       gop
    ## 727       gop
    ## 728       gop
    ## 729       gop
    ## 730       gop
    ## 731       gop
    ## 732       gop
    ## 733       gop
    ## 734       gop
    ## 735       gop
    ## 736       gop
    ## 737       gop
    ## 738       gop
    ## 739       gop
    ## 740       dem
    ## 741       dem
    ## 742       dem
    ## 743       dem
    ## 744       dem
    ## 745       dem
    ## 746       dem
    ## 747       dem
    ## 748       dem
    ## 749       dem
    ## 750       dem
    ## 751       dem
    ## 752       dem
    ## 753       dem
    ## 754       dem
    ## 755       dem
    ## 756       dem
    ## 757       dem
    ## 758       dem
    ## 759       dem
    ## 760       dem
    ## 761       dem
    ## 762       dem
    ## 763       dem
    ## 764       dem
    ## 765       dem
    ## 766       dem
    ## 767       dem
    ## 768       dem
    ## 769       dem
    ## 770       dem
    ## 771       dem
    ## 772       dem
    ## 773       dem
    ## 774       dem
    ## 775       dem
    ## 776       dem
    ## 777       dem
    ## 778       dem
    ## 779       dem
    ## 780       dem
    ## 781       dem
    ## 782       dem
    ## 783       dem
    ## 784       dem
    ## 785       dem
    ## 786       dem
    ## 787       dem
    ## 788       dem
    ## 789       dem
    ## 790       dem
    ## 791       dem
    ## 792       dem
    ## 793       dem
    ## 794       dem
    ## 795       dem
    ## 796       dem
    ## 797       dem
    ## 798       dem
    ## 799       dem
    ## 800       dem
    ## 801       dem
    ## 802       dem
    ## 803       dem
    ## 804       dem
    ## 805       dem
    ## 806       dem
    ## 807       dem
    ## 808       dem
    ## 809       dem
    ## 810       dem
    ## 811       dem
    ## 812       dem
    ## 813       dem
    ## 814       dem
    ## 815       dem
    ## 816       dem
    ## 817       dem

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
  left_join(pols_month_data, snp_data, by = c("year", "month")) %>%
  janitor::clean_names()

fivethirtyeight_data = 
  left_join(fivethirtyeight_data, unemployment_data, by = c("year", "month")) %>%
  janitor::clean_names()

fivethirtyeight_data
```

    ##     year     month gov_gop sen_gop rep_gop gov_dem sen_dem rep_dem
    ## 1   1947     April      23      51     253      23      45     198
    ## 2   1947    August      23      51     253      23      45     198
    ## 3   1947  December      24      51     253      23      45     198
    ## 4   1947  February      23      51     253      23      45     198
    ## 5   1947   January      23      51     253      23      45     198
    ## 6   1947      July      23      51     253      23      45     198
    ## 7   1947      June      23      51     253      23      45     198
    ## 8   1947     March      23      51     253      23      45     198
    ## 9   1947       May      23      51     253      23      45     198
    ## 10  1947  November      24      51     253      23      45     198
    ## 11  1947   October      23      51     253      23      45     198
    ## 12  1947 September      23      51     253      23      45     198
    ## 13  1948     April      22      53     253      24      48     198
    ## 14  1948    August      22      53     253      24      48     198
    ## 15  1948  December      22      53     253      24      48     198
    ## 16  1948  February      22      53     253      24      48     198
    ## 17  1948   January      22      53     253      24      48     198
    ## 18  1948      July      22      53     253      24      48     198
    ## 19  1948      June      22      53     253      24      48     198
    ## 20  1948     March      22      53     253      24      48     198
    ## 21  1948       May      22      53     253      24      48     198
    ## 22  1948  November      22      53     253      24      48     198
    ## 23  1948   October      22      53     253      24      48     198
    ## 24  1948 September      22      53     253      24      48     198
    ## 25  1949     April      18      45     177      29      58     269
    ## 26  1949    August      18      45     177      30      58     269
    ## 27  1949  December      18      45     177      30      58     269
    ## 28  1949  February      18      45     177      29      58     269
    ## 29  1949   January      18      45     177      29      58     269
    ## 30  1949      July      18      45     177      30      58     269
    ## 31  1949      June      18      45     177      29      58     269
    ## 32  1949     March      18      45     177      29      58     269
    ## 33  1949       May      18      45     177      29      58     269
    ## 34  1949  November      18      45     177      30      58     269
    ## 35  1949   October      18      45     177      30      58     269
    ## 36  1949 September      18      45     177      30      58     269
    ## 37  1950     April      18      44     177      29      57     269
    ## 38  1950    August      18      44     177      29      57     269
    ## 39  1950  December      18      44     177      29      57     269
    ## 40  1950  February      18      44     177      29      57     269
    ## 41  1950   January      18      44     177      29      57     269
    ## 42  1950      July      18      44     177      29      57     269
    ## 43  1950      June      18      44     177      29      57     269
    ## 44  1950     March      18      44     177      29      57     269
    ## 45  1950       May      18      44     177      29      57     269
    ## 46  1950  November      18      44     177      29      57     269
    ## 47  1950   October      18      44     177      29      57     269
    ## 48  1950 September      18      44     177      29      57     269
    ## 49  1951     April      24      47     207      22      51     242
    ## 50  1951    August      24      47     207      22      51     242
    ## 51  1951  December      25      47     207      22      51     242
    ## 52  1951  February      24      47     207      22      51     242
    ## 53  1951   January      24      47     207      22      51     242
    ## 54  1951      July      24      47     207      22      51     242
    ## 55  1951      June      24      47     207      22      51     242
    ## 56  1951     March      24      47     207      22      51     242
    ## 57  1951       May      24      47     207      22      51     242
    ## 58  1951  November      25      47     207      22      51     242
    ## 59  1951   October      25      47     207      22      51     242
    ## 60  1951 September      24      47     207      22      51     242
    ## 61  1952     April      24      50     207      22      50     242
    ## 62  1952    August      24      50     207      22      50     242
    ## 63  1952  December      24      50     207      22      50     242
    ## 64  1952  February      24      50     207      22      50     242
    ## 65  1952   January      24      50     207      22      50     242
    ## 66  1952      July      24      50     207      22      50     242
    ## 67  1952      June      24      50     207      22      50     242
    ## 68  1952     March      24      50     207      22      50     242
    ## 69  1952       May      24      50     207      22      50     242
    ## 70  1952  November      24      50     207      22      50     242
    ## 71  1952   October      24      50     207      22      50     242
    ## 72  1952 September      24      50     207      22      50     242
    ## 73  1953     April      29      50     222      17      49     220
    ## 74  1953    August      29      50     222      17      49     220
    ## 75  1953  December      30      50     222      18      49     220
    ## 76  1953  February      29      50     222      17      49     220
    ## 77  1953   January      29      50     222      17      49     220
    ## 78  1953      July      29      50     222      17      49     220
    ## 79  1953      June      29      50     222      17      49     220
    ## 80  1953     March      29      50     222      17      49     220
    ## 81  1953       May      29      50     222      17      49     220
    ## 82  1953  November      30      50     222      18      49     220
    ## 83  1953   October      30      50     222      18      49     220
    ## 84  1953 September      29      50     222      17      49     220
    ## 85  1954     April      29      55     222      18      53     220
    ## 86  1954    August      29      55     222      18      53     220
    ## 87  1954  December      29      55     222      19      53     220
    ## 88  1954  February      29      55     222      18      53     220
    ## 89  1954   January      29      55     222      18      53     220
    ## 90  1954      July      29      55     222      18      53     220
    ## 91  1954      June      29      55     222      18      53     220
    ## 92  1954     March      29      55     222      18      53     220
    ## 93  1954       May      29      55     222      18      53     220
    ## 94  1954  November      29      55     222      19      53     220
    ## 95  1954   October      29      55     222      18      53     220
    ## 96  1954 September      29      55     222      18      53     220
    ## 97  1955     April      21      47     204      26      48     237
    ## 98  1955    August      21      47     204      26      48     237
    ## 99  1955  December      21      47     204      26      48     237
    ## 100 1955  February      21      47     204      26      48     237
    ## 101 1955   January      21      47     204      26      48     237
    ## 102 1955      July      21      47     204      26      48     237
    ## 103 1955      June      21      47     204      26      48     237
    ## 104 1955     March      21      47     204      26      48     237
    ## 105 1955       May      21      47     204      26      48     237
    ## 106 1955  November      21      47     204      26      48     237
    ## 107 1955   October      21      47     204      26      48     237
    ## 108 1955 September      21      47     204      26      48     237
    ## 109 1956     April      21      49     204      26      50     237
    ## 110 1956    August      21      49     204      26      50     237
    ## 111 1956  December      21      49     204      26      51     237
    ## 112 1956  February      21      49     204      26      51     237
    ## 113 1956   January      21      49     204      26      51     237
    ## 114 1956      July      21      49     204      26      50     237
    ## 115 1956      June      21      49     204      26      50     237
    ## 116 1956     March      21      49     204      26      51     237
    ## 117 1956       May      21      49     204      26      50     237
    ## 118 1956  November      21      49     204      26      51     237
    ## 119 1956   October      21      49     204      26      50     237
    ## 120 1956 September      21      49     204      26      50     237
    ## 121 1957     April      19      47     203      28      52     242
    ## 122 1957    August      19      47     203      28      52     242
    ## 123 1957  December      20      47     203      28      52     242
    ## 124 1957  February      19      47     203      28      52     242
    ## 125 1957   January      19      47     203      28      52     242
    ## 126 1957      July      19      47     203      28      52     242
    ## 127 1957      June      19      47     203      28      52     242
    ## 128 1957     March      19      47     203      28      52     242
    ## 129 1957       May      19      47     203      28      52     242
    ## 130 1957  November      20      47     203      28      52     242
    ## 131 1957   October      20      47     203      28      52     242
    ## 132 1957 September      20      47     203      28      52     242
    ## 133 1958     April      20      47     203      28      52     242
    ## 134 1958    August      20      47     203      28      52     242
    ## 135 1958  December      20      47     203      28      52     242
    ## 136 1958  February      20      47     203      28      52     242
    ## 137 1958   January      20      47     203      28      52     242
    ## 138 1958      July      20      47     203      28      52     242
    ## 139 1958      June      20      47     203      28      52     242
    ## 140 1958     March      20      47     203      28      52     242
    ## 141 1958       May      20      47     203      28      52     242
    ## 142 1958  November      20      47     203      28      52     242
    ## 143 1958   October      20      47     203      28      52     242
    ## 144 1958 September      20      47     203      28      52     242
    ## 145 1959     April      15      35     159      35      65     289
    ## 146 1959    August      15      35     159      35      65     289
    ## 147 1959  December      15      35     159      35      65     289
    ## 148 1959  February      15      35     159      35      65     289
    ## 149 1959   January      15      35     159      35      65     289
    ## 150 1959      July      15      35     159      35      65     289
    ## 151 1959      June      15      35     159      35      65     289
    ## 152 1959     March      15      35     159      35      65     289
    ## 153 1959       May      15      35     159      35      65     289
    ## 154 1959  November      15      35     159      35      65     289
    ## 155 1959   October      15      35     159      35      65     289
    ## 156 1959 September      15      35     159      35      65     289
    ## 157 1960     April      16      35     159      34      70     289
    ## 158 1960    August      16      35     159      34      70     289
    ## 159 1960  December      17      35     159      34      70     289
    ## 160 1960  February      16      35     159      34      70     289
    ## 161 1960   January      16      35     159      34      70     289
    ## 162 1960      July      16      35     159      34      70     289
    ## 163 1960      June      16      35     159      34      70     289
    ## 164 1960     March      16      35     159      34      70     289
    ## 165 1960       May      16      35     159      34      70     289
    ## 166 1960  November      17      35     159      34      70     289
    ## 167 1960   October      17      35     159      34      70     289
    ## 168 1960 September      17      35     159      34      70     289
    ## 169 1961     April      16      37     176      34      64     273
    ## 170 1961    August      16      37     176      34      64     273
    ## 171 1961  December      16      37     176      34      64     273
    ## 172 1961  February      16      37     176      34      64     273
    ## 173 1961   January      16      37     176      34      64     273
    ## 174 1961      July      16      37     176      34      64     273
    ## 175 1961      June      16      37     176      34      64     273
    ## 176 1961     March      16      37     176      34      64     273
    ## 177 1961       May      16      37     176      34      64     273
    ## 178 1961  November      16      37     176      34      64     273
    ## 179 1961   October      16      37     176      34      64     273
    ## 180 1961 September      16      37     176      34      64     273
    ## 181 1962     April      16      42     176      34      65     273
    ## 182 1962    August      16      42     176      34      65     273
    ## 183 1962  December      16      42     176      34      65     273
    ## 184 1962  February      16      42     176      34      65     273
    ## 185 1962   January      16      42     176      34      65     273
    ## 186 1962      July      16      42     176      34      65     273
    ## 187 1962      June      16      42     176      34      65     273
    ## 188 1962     March      16      42     176      34      65     273
    ## 189 1962       May      16      42     176      34      65     273
    ## 190 1962  November      16      42     176      34      65     273
    ## 191 1962   October      16      42     176      34      65     273
    ## 192 1962 September      16      42     176      34      65     273
    ## 193 1963     April      16      34     182      34      68     262
    ## 194 1963    August      16      34     182      34      68     262
    ## 195 1963  December      16      34     182      34      68     262
    ## 196 1963  February      17      34     182      33      68     262
    ## 197 1963   January      17      34     182      33      68     262
    ## 198 1963      July      16      34     182      34      68     262
    ## 199 1963      June      16      34     182      34      68     262
    ## 200 1963     March      16      34     182      34      68     262
    ## 201 1963       May      16      34     182      34      68     262
    ## 202 1963  November      16      34     182      34      68     262
    ## 203 1963   October      16      34     182      34      68     262
    ## 204 1963 September      16      34     182      34      68     262
    ## 205 1964     April      16      34     182      34      71     262
    ## 206 1964    August      16      34     182      34      71     262
    ## 207 1964  December      16      34     182      34      71     262
    ## 208 1964  February      16      34     182      34      71     262
    ## 209 1964   January      16      34     182      34      71     262
    ## 210 1964      July      16      34     182      34      71     262
    ## 211 1964      June      16      34     182      34      71     262
    ## 212 1964     March      16      34     182      34      71     262
    ## 213 1964       May      16      34     182      34      71     262
    ## 214 1964  November      16      34     182      34      71     262
    ## 215 1964   October      16      34     182      34      71     262
    ## 216 1964 September      16      34     182      34      71     262
    ## 217 1965     April      17      32     141      33      69     301
    ## 218 1965    August      17      32     141      33      69     301
    ## 219 1965  December      17      32     141      33      69     301
    ## 220 1965  February      17      32     141      33      69     301
    ## 221 1965   January      17      32     141      33      69     301
    ## 222 1965      July      17      32     141      33      69     301
    ## 223 1965      June      17      32     141      33      69     301
    ## 224 1965     March      17      32     141      33      69     301
    ## 225 1965       May      17      32     141      33      69     301
    ## 226 1965  November      17      32     141      33      69     301
    ## 227 1965   October      17      32     141      33      69     301
    ## 228 1965 September      17      32     141      33      69     301
    ## 229 1966     April      18      33     141      33      70     301
    ## 230 1966    August      18      33     141      33      70     301
    ## 231 1966  December      18      33     141      33      70     301
    ## 232 1966  February      18      33     141      33      70     301
    ## 233 1966   January      18      33     141      33      70     301
    ## 234 1966      July      18      33     141      33      70     301
    ## 235 1966      June      18      33     141      33      70     301
    ## 236 1966     March      18      33     141      33      70     301
    ## 237 1966       May      18      33     141      33      70     301
    ## 238 1966  November      18      33     141      33      70     301
    ## 239 1966   October      18      33     141      33      70     301
    ## 240 1966 September      18      33     141      33      70     301
    ## 241 1967     April      25      36     188      27      64     251
    ## 242 1967    August      25      36     188      27      64     251
    ## 243 1967  December      25      36     188      27      64     251
    ## 244 1967  February      25      36     188      27      64     251
    ## 245 1967   January      25      36     188      27      64     251
    ## 246 1967      July      25      36     188      27      64     251
    ## 247 1967      June      25      36     188      27      64     251
    ## 248 1967     March      25      36     188      27      64     251
    ## 249 1967       May      25      36     188      27      64     251
    ## 250 1967  November      25      36     188      27      64     251
    ## 251 1967   October      25      36     188      27      64     251
    ## 252 1967 September      25      36     188      27      64     251
    ## 253 1968     April      26      39     188      26      65     251
    ## 254 1968    August      26      39     188      26      65     251
    ## 255 1968  December      26      39     188      26      65     251
    ## 256 1968  February      26      39     188      26      65     251
    ## 257 1968   January      26      39     188      26      65     251
    ## 258 1968      July      26      39     188      26      65     251
    ## 259 1968      June      26      39     188      26      65     251
    ## 260 1968     March      26      39     188      26      65     251
    ## 261 1968       May      26      39     188      26      65     251
    ## 262 1968  November      26      39     188      26      65     251
    ## 263 1968   October      26      39     188      26      65     251
    ## 264 1968 September      26      39     188      26      65     251
    ## 265 1969     April      31      43     199      22      57     250
    ## 266 1969    August      31      43     199      22      57     250
    ## 267 1969  December      31      43     199      22      57     250
    ## 268 1969  February      31      43     199      22      57     250
    ## 269 1969   January      31      43     199      22      57     250
    ## 270 1969      July      31      43     199      22      57     250
    ## 271 1969      June      31      43     199      22      57     250
    ## 272 1969     March      31      43     199      22      57     250
    ## 273 1969       May      31      43     199      22      57     250
    ## 274 1969  November      31      43     199      22      57     250
    ## 275 1969   October      31      43     199      22      57     250
    ## 276 1969 September      31      43     199      22      57     250
    ## 277 1970     April      32      43     199      20      58     250
    ## 278 1970    August      32      43     199      20      58     250
    ## 279 1970  December      32      43     199      20      58     250
    ## 280 1970  February      32      43     199      20      58     250
    ## 281 1970   January      32      43     199      20      58     250
    ## 282 1970      July      32      43     199      20      58     250
    ## 283 1970      June      32      43     199      20      58     250
    ## 284 1970     March      32      43     199      20      58     250
    ## 285 1970       May      32      43     199      20      58     250
    ## 286 1970  November      32      43     199      20      58     250
    ## 287 1970   October      32      43     199      20      58     250
    ## 288 1970 September      32      43     199      20      58     250
    ## 289 1971     April      21      44     185      30      55     259
    ## 290 1971    August      21      44     185      30      55     259
    ## 291 1971  December      21      44     185      30      55     259
    ## 292 1971  February      21      44     185      30      55     259
    ## 293 1971   January      21      44     185      30      55     259
    ## 294 1971      July      21      44     185      30      55     259
    ## 295 1971      June      21      44     185      30      55     259
    ## 296 1971     March      21      44     185      30      55     259
    ## 297 1971       May      21      44     185      30      55     259
    ## 298 1971  November      21      44     185      30      55     259
    ## 299 1971   October      21      44     185      30      55     259
    ## 300 1971 September      21      44     185      30      55     259
    ## 301 1972     April      20      44     185      31      57     259
    ## 302 1972    August      20      44     185      31      57     259
    ## 303 1972  December      20      44     185      31      57     259
    ## 304 1972  February      20      44     185      31      57     259
    ## 305 1972   January      20      44     185      31      57     259
    ## 306 1972      July      20      44     185      31      57     259
    ## 307 1972      June      20      44     185      31      57     259
    ## 308 1972     March      20      44     185      31      57     259
    ## 309 1972       May      20      44     185      31      57     259
    ## 310 1972  November      20      44     185      31      57     259
    ## 311 1972   October      20      44     185      31      57     259
    ## 312 1972 September      20      44     185      31      57     259
    ## 313 1973     April      19      42     195      32      56     249
    ## 314 1973    August      20      42     195      32      56     249
    ## 315 1973  December      20      42     195      32      56     249
    ## 316 1973  February      19      42     195      32      56     249
    ## 317 1973   January      19      42     195      32      56     249
    ## 318 1973      July      19      42     195      32      56     249
    ## 319 1973      June      19      42     195      32      56     249
    ## 320 1973     March      19      42     195      32      56     249
    ## 321 1973       May      19      42     195      32      56     249
    ## 322 1973  November      20      42     195      32      56     249
    ## 323 1973   October      20      42     195      32      56     249
    ## 324 1973 September      20      42     195      32      56     249
    ## 325 1974     April      18      45     195      34      59     249
    ## 326 1974  February      18      45     195      34      59     249
    ## 327 1974   January      18      45     195      34      59     249
    ## 328 1974      July      18      45     195      34      59     249
    ## 329 1974      June      18      45     195      34      59     249
    ## 330 1974     March      18      45     195      34      59     249
    ## 331 1974       May      18      45     195      34      59     249
    ## 332 1975     April      13      38     148      37      61     295
    ## 333 1975    August      13      38     148      37      61     295
    ## 334 1975  December      13      38     148      37      61     295
    ## 335 1975  February      13      38     148      37      61     295
    ## 336 1975   January      13      38     148      37      61     295
    ## 337 1975      July      13      38     148      37      61     295
    ## 338 1975      June      13      38     148      37      61     295
    ## 339 1975     March      13      38     148      37      61     295
    ## 340 1975       May      13      38     148      37      61     295
    ## 341 1975  November      13      38     148      37      61     295
    ## 342 1975   October      13      38     148      37      61     295
    ## 343 1975 September      13      38     148      37      61     295
    ## 344 1976     April      13      39     148      37      65     295
    ## 345 1976    August      13      39     148      37      65     295
    ## 346 1976  December      13      39     148      37      65     295
    ## 347 1976  February      13      39     148      37      65     295
    ## 348 1976   January      13      39     148      37      65     295
    ## 349 1976      July      13      39     148      37      65     295
    ## 350 1976      June      13      39     148      37      65     295
    ## 351 1976     March      13      39     148      37      65     295
    ## 352 1976       May      13      39     148      37      65     295
    ## 353 1976  November      13      39     148      37      65     295
    ## 354 1976   October      13      39     148      37      65     295
    ## 355 1976 September      13      39     148      37      65     295
    ## 356 1977     April      12      38     147      38      62     294
    ## 357 1977    August      12      38     147      40      62     294
    ## 358 1977  December      12      38     147      41      62     294
    ## 359 1977  February      12      38     147      38      62     294
    ## 360 1977   January      12      38     147      38      62     294
    ## 361 1977      July      12      38     147      40      62     294
    ## 362 1977      June      12      38     147      39      62     294
    ## 363 1977     March      12      38     147      38      62     294
    ## 364 1977       May      12      38     147      38      62     294
    ## 365 1977  November      12      38     147      41      62     294
    ## 366 1977   October      12      38     147      40      62     294
    ## 367 1977 September      12      38     147      40      62     294
    ## 368 1978     April      12      41     147      38      66     294
    ## 369 1978    August      12      41     147      39      66     294
    ## 370 1978  December      12      41     147      39      66     294
    ## 371 1978  February      12      41     147      38      66     294
    ## 372 1978   January      12      41     147      38      66     294
    ## 373 1978      July      12      41     147      38      66     294
    ## 374 1978      June      12      41     147      38      66     294
    ## 375 1978     March      12      41     147      38      66     294
    ## 376 1978       May      12      41     147      38      66     294
    ## 377 1978  November      12      41     147      39      66     294
    ## 378 1978   October      12      41     147      39      66     294
    ## 379 1978 September      12      41     147      39      66     294
    ## 380 1979     April      19      41     160      32      58     280
    ## 381 1979    August      19      41     160      32      58     280
    ## 382 1979  December      19      41     160      32      58     280
    ## 383 1979  February      19      41     160      32      58     280
    ## 384 1979   January      19      41     160      32      58     280
    ## 385 1979      July      19      41     160      32      58     280
    ## 386 1979      June      19      41     160      32      58     280
    ## 387 1979     March      19      41     160      32      58     280
    ## 388 1979       May      19      41     160      32      58     280
    ## 389 1979  November      19      41     160      32      58     280
    ## 390 1979   October      19      41     160      32      58     280
    ## 391 1979 September      19      41     160      32      58     280
    ## 392 1980     April      20      42     160      31      59     280
    ## 393 1980    August      20      42     160      31      59     280
    ## 394 1980  December      20      42     160      31      59     280
    ## 395 1980  February      19      42     160      32      59     280
    ## 396 1980   January      19      42     160      32      59     280
    ## 397 1980      July      20      42     160      31      59     280
    ## 398 1980      June      20      42     160      31      59     280
    ## 399 1980     March      20      42     160      31      59     280
    ## 400 1980       May      20      42     160      31      59     280
    ## 401 1980  November      20      42     160      31      59     280
    ## 402 1980   October      20      42     160      31      59     280
    ## 403 1980 September      20      42     160      31      59     280
    ## 404 1981     April      24      53     196      27      46     246
    ## 405 1981    August      24      53     196      27      46     246
    ## 406 1981  December      24      53     196      27      46     246
    ## 407 1981  February      24      53     196      27      46     246
    ## 408 1981   January      24      53     196      27      46     246
    ## 409 1981      July      24      53     196      27      46     246
    ## 410 1981      June      24      53     196      27      46     246
    ## 411 1981     March      24      53     196      27      46     246
    ## 412 1981       May      24      53     196      27      46     246
    ## 413 1981  November      24      53     196      27      46     246
    ## 414 1981   October      24      53     196      27      46     246
    ## 415 1981 September      24      53     196      27      46     246
    ## 416 1982     April      24      54     196      27      47     246
    ## 417 1982    August      24      54     196      27      47     246
    ## 418 1982  December      24      54     196      27      47     246
    ## 419 1982  February      24      54     196      27      47     246
    ## 420 1982   January      24      54     196      27      47     246
    ## 421 1982      July      24      54     196      27      47     246
    ## 422 1982      June      24      54     196      27      47     246
    ## 423 1982     March      24      54     196      27      47     246
    ## 424 1982       May      24      54     196      27      47     246
    ## 425 1982  November      24      54     196      27      47     246
    ## 426 1982   October      24      54     196      27      47     246
    ## 427 1982 September      24      54     196      27      47     246
    ## 428 1983     April      16      55     168      34      46     271
    ## 429 1983    August      16      55     168      34      46     272
    ## 430 1983  December      16      55     168      34      46     272
    ## 431 1983  February      16      55     168      34      46     272
    ## 432 1983   January      16      55     168      34      46     272
    ## 433 1983      July      16      55     168      34      46     272
    ## 434 1983      June      16      55     168      34      46     271
    ## 435 1983     March      16      55     168      34      46     272
    ## 436 1983       May      16      55     168      34      46     271
    ## 437 1983  November      16      55     168      34      46     272
    ## 438 1983   October      16      55     168      34      46     272
    ## 439 1983 September      16      55     168      34      46     272
    ## 440 1984     April      15      55     168      34      45     271
    ## 441 1984    August      15      55     168      35      45     271
    ## 442 1984  December      15      55     168      35      45     271
    ## 443 1984  February      16      55     168      34      45     271
    ## 444 1984   January      16      55     168      34      45     271
    ## 445 1984      July      15      55     168      35      45     271
    ## 446 1984      June      15      55     168      35      45     271
    ## 447 1984     March      15      55     168      34      45     271
    ## 448 1984       May      15      55     168      35      45     271
    ## 449 1984  November      15      55     168      35      45     271
    ## 450 1984   October      15      55     168      35      45     271
    ## 451 1984 September      15      55     168      35      45     271
    ## 452 1985     April      16      53     183      34      47     257
    ## 453 1985    August      16      53     183      34      47     257
    ## 454 1985  December      16      53     183      34      47     257
    ## 455 1985  February      16      53     183      34      47     257
    ## 456 1985   January      16      53     183      34      47     257
    ## 457 1985      July      16      53     183      34      47     257
    ## 458 1985      June      16      53     183      34      47     257
    ## 459 1985     March      16      53     183      34      47     257
    ## 460 1985       May      16      53     183      34      47     257
    ## 461 1985  November      16      53     183      34      47     257
    ## 462 1985   October      16      53     183      34      47     257
    ## 463 1985 September      16      53     183      34      47     257
    ## 464 1986     April      16      53     183      34      48     257
    ## 465 1986    August      16      54     183      34      48     257
    ## 466 1986  December      16      54     183      34      48     257
    ## 467 1986  February      16      53     183      34      48     257
    ## 468 1986   January      16      53     183      34      48     257
    ## 469 1986      July      16      54     183      34      48     257
    ## 470 1986      June      16      53     183      34      48     257
    ## 471 1986     March      16      53     183      34      48     257
    ## 472 1986       May      16      53     183      34      48     257
    ## 473 1986  November      16      54     183      34      48     257
    ## 474 1986   October      16      54     183      34      48     257
    ## 475 1986 September      16      54     183      34      48     257
    ## 476 1987     April      24      46     179      27      55     262
    ## 477 1987    August      24      46     179      27      55     262
    ## 478 1987  December      24      46     179      27      55     262
    ## 479 1987  February      24      46     179      27      55     262
    ## 480 1987   January      24      46     179      27      55     262
    ## 481 1987      July      24      46     179      27      55     262
    ## 482 1987      June      24      46     179      27      55     262
    ## 483 1987     March      24      46     179      27      55     262
    ## 484 1987       May      24      46     179      27      55     262
    ## 485 1987  November      24      46     179      27      55     262
    ## 486 1987   October      24      46     179      27      55     262
    ## 487 1987 September      24      46     179      27      55     262
    ## 488 1988     April      24      46     179      28      54     262
    ## 489 1988    August      24      46     178      28      54     262
    ## 490 1988  December      24      46     179      28      54     262
    ## 491 1988  February      24      46     179      28      54     262
    ## 492 1988   January      24      46     179      28      54     262
    ## 493 1988      July      24      46     178      28      54     262
    ## 494 1988      June      24      46     179      28      54     262
    ## 495 1988     March      25      46     179      28      54     262
    ## 496 1988       May      24      46     179      28      54     262
    ## 497 1988  November      24      46     179      28      54     262
    ## 498 1988   October      24      46     178      28      54     262
    ## 499 1988 September      24      46     178      28      54     262
    ## 500 1989     April      23      46     178      29      55     264
    ## 501 1989    August      23      46     178      29      55     261
    ## 502 1989  December      23      46     178      29      55     261
    ## 503 1989  February      23      46     178      29      55     264
    ## 504 1989   January      23      46     178      29      55     264
    ## 505 1989      July      23      46     178      29      55     261
    ## 506 1989      June      23      46     178      29      55     262
    ## 507 1989     March      23      46     178      29      55     264
    ## 508 1989       May      23      46     178      29      55     264
    ## 509 1989  November      23      46     178      29      55     261
    ## 510 1989   October      23      46     178      29      55     261
    ## 511 1989 September      23      46     178      29      55     261
    ## 512 1990     April      22      46     176      30      55     258
    ## 513 1990    August      22      46     176      30      56     257
    ## 514 1990  December      22      46     176      30      56     259
    ## 515 1990  February      22      46     176      30      55     258
    ## 516 1990   January      22      46     176      30      55     258
    ## 517 1990      July      22      46     176      30      56     257
    ## 518 1990      June      22      46     176      30      56     257
    ## 519 1990     March      22      46     176      30      55     258
    ## 520 1990       May      22      46     176      30      55     257
    ## 521 1990  November      22      46     176      30      56     259
    ## 522 1990   October      22      46     176      30      56     258
    ## 523 1990 September      22      46     176      30      56     257
    ## 524 1991     April      21      45     166      28      57     269
    ## 525 1991    August      21      45     166      29      57     268
    ## 526 1991  December      21      45     167      29      57     269
    ## 527 1991  February      20      45     168      29      57     269
    ## 528 1991   January      20      45     168      29      57     269
    ## 529 1991      July      21      45     166      28      57     268
    ## 530 1991      June      21      45     166      28      57     268
    ## 531 1991     March      21      45     166      28      57     269
    ## 532 1991       May      21      45     166      28      57     268
    ## 533 1991  November      21      45     167      29      57     269
    ## 534 1991   October      21      45     166      29      57     268
    ## 535 1991 September      21      45     166      29      57     268
    ## 536 1992     April      20      43     166      28      58     268
    ## 537 1992    August      20      43     166      28      58     268
    ## 538 1992  December      20      43     166      28      60     270
    ## 539 1992  February      20      43     166      27      58     268
    ## 540 1992   January      20      43     166      27      58     268
    ## 541 1992      July      20      43     166      28      58     268
    ## 542 1992      June      20      43     166      28      58     268
    ## 543 1992     March      20      43     166      28      58     268
    ## 544 1992       May      20      43     166      28      58     268
    ## 545 1992  November      20      43     166      28      60     270
    ## 546 1992   October      20      43     166      28      59     268
    ## 547 1992 September      20      43     166      28      58     268
    ## 548 1993     April      18      46     175      30      57     256
    ## 549 1993    August      17      46     178      31      57     258
    ## 550 1993  December      17      46     178      31      57     258
    ## 551 1993  February      18      46     175      30      57     255
    ## 552 1993   January      18      46     175      30      57     255
    ## 553 1993      July      17      46     178      31      57     258
    ## 554 1993      June      17      46     178      31      57     258
    ## 555 1993     March      18      46     175      30      57     255
    ## 556 1993       May      17      46     177      31      57     257
    ## 557 1993  November      17      46     178      31      57     258
    ## 558 1993   October      17      46     178      31      57     258
    ## 559 1993 September      17      46     178      31      57     258
    ## 560 1994     April      19      48     178      28      54     256
    ## 561 1994    August      19      48     178      28      54     256
    ## 562 1994  December      19      48     178      28      54     256
    ## 563 1994  February      19      48     178      28      54     257
    ## 564 1994   January      19      48     178      28      54     257
    ## 565 1994      July      19      48     178      28      54     256
    ## 566 1994      June      19      48     178      28      54     256
    ## 567 1994     March      19      48     178      28      54     257
    ## 568 1994       May      19      48     178      28      54     256
    ## 569 1994  November      19      48     178      28      54     256
    ## 570 1994   October      19      48     178      28      54     256
    ## 571 1994 September      19      48     178      28      54     256
    ## 572 1995     April      30      54     235      20      46     199
    ## 573 1995    August      30      54     235      20      46     199
    ## 574 1995  December      30      54     235      20      46     199
    ## 575 1995  February      30      54     235      20      46     199
    ## 576 1995   January      30      54     235      20      46     199
    ## 577 1995      July      30      54     235      20      46     199
    ## 578 1995      June      30      54     235      20      46     199
    ## 579 1995     March      30      54     235      20      46     199
    ## 580 1995       May      30      54     235      20      46     199
    ## 581 1995  November      30      54     235      20      46     199
    ## 582 1995   October      30      54     235      20      46     199
    ## 583 1995 September      30      54     235      20      46     199
    ## 584 1996     April      31      54     236      19      47     196
    ## 585 1996    August      32      54     235      19      47     198
    ## 586 1996  December      32      55     235      19      47     198
    ## 587 1996  February      31      54     236      19      47     196
    ## 588 1996   January      31      54     236      19      47     196
    ## 589 1996      July      32      54     235      19      47     198
    ## 590 1996      June      31      54     236      19      47     198
    ## 591 1996     March      31      54     236      19      47     195
    ## 592 1996       May      31      54     236      19      47     197
    ## 593 1996  November      32      55     235      19      47     198
    ## 594 1996   October      32      54     235      19      47     198
    ## 595 1996 September      32      54     235      19      47     198
    ## 596 1997     April      32      55     227      18      45     207
    ## 597 1997    August      33      55     228      18      45     207
    ## 598 1997  December      34      55     229      18      45     207
    ## 599 1997  February      32      55     227      18      45     207
    ## 600 1997   January      32      55     227      18      45     207
    ## 601 1997      July      32      55     228      18      45     207
    ## 602 1997      June      32      55     228      18      45     207
    ## 603 1997     March      32      55     227      18      45     207
    ## 604 1997       May      32      55     228      18      45     207
    ## 605 1997  November      34      55     229      18      45     207
    ## 606 1997   October      34      55     228      18      45     207
    ## 607 1997 September      34      55     228      18      45     207
    ## 608 1998     April      32      55     227      18      45     205
    ## 609 1998    August      32      55     228      18      45     206
    ## 610 1998  December      32      55     228      18      45     206
    ## 611 1998  February      32      55     227      18      45     203
    ## 612 1998   January      32      55     227      18      45     203
    ## 613 1998      July      32      55     228      18      45     206
    ## 614 1998      June      32      55     227      18      45     206
    ## 615 1998     March      32      55     227      18      45     204
    ## 616 1998       May      32      55     227      18      45     205
    ## 617 1998  November      32      55     228      18      45     206
    ## 618 1998   October      32      55     228      18      45     206
    ## 619 1998 September      32      55     228      18      45     206
    ## 620 1999     April      31      55     222      17      45     210
    ## 621 1999    August      31      55     223      17      45     210
    ## 622 1999  December      31      56     223      17      45     211
    ## 623 1999  February      31      55     223      17      45     210
    ## 624 1999   January      31      55     223      17      45     210
    ## 625 1999      July      31      55     223      17      45     210
    ## 626 1999      June      31      55     223      17      45     210
    ## 627 1999     March      31      55     222      17      45     210
    ## 628 1999       May      31      55     222      17      45     210
    ## 629 1999  November      31      56     223      17      45     211
    ## 630 1999   October      31      55     223      17      45     210
    ## 631 1999 September      31      55     223      17      45     210
    ## 632 2000     April      30      55     223      18      45     210
    ## 633 2000    August      30      55     223      18      46     210
    ## 634 2000  December      30      55     223      19      46     210
    ## 635 2000  February      30      55     223      18      45     210
    ## 636 2000   January      30      55     223      18      45     210
    ## 637 2000      July      30      55     223      18      45     210
    ## 638 2000      June      30      55     223      18      45     210
    ## 639 2000     March      30      55     223      18      45     210
    ## 640 2000       May      30      55     223      18      45     210
    ## 641 2000  November      30      55     223      19      46     210
    ## 642 2000   October      30      55     223      18      46     210
    ## 643 2000 September      30      55     223      18      46     210
    ## 644 2001     April      29      49     220      19      50     210
    ## 645 2001    August      29      49     222      19      50     210
    ## 646 2001  December      30      49     223      19      50     211
    ## 647 2001  February      29      49     220      19      50     211
    ## 648 2001   January      29      49     220      19      50     211
    ## 649 2001      July      29      49     222      19      50     210
    ## 650 2001      June      29      49     221      19      50     210
    ## 651 2001     March      29      49     220      19      50     211
    ## 652 2001       May      29      49     220      19      50     210
    ## 653 2001  November      30      49     223      19      50     211
    ## 654 2001   October      30      49     222      19      50     210
    ## 655 2001 September      29      49     222      19      50     210
    ## 656 2002     April      27      50     222      21      50     211
    ## 657 2002    August      27      50     222      21      50     211
    ## 658 2002  December      27      50     222      21      50     211
    ## 659 2002  February      27      50     221      21      50     211
    ## 660 2002   January      27      50     221      21      50     211
    ## 661 2002      July      27      50     222      21      50     211
    ## 662 2002      June      27      50     222      21      50     211
    ## 663 2002     March      27      50     222      21      50     211
    ## 664 2002       May      27      50     222      21      50     211
    ## 665 2002  November      27      50     222      21      50     211
    ## 666 2002   October      27      50     222      21      50     211
    ## 667 2002 September      27      50     222      21      50     211
    ## 668 2003     April      26      51     231      24      48     203
    ## 669 2003    August      26      51     231      24      48     203
    ## 670 2003  December      27      51     231      25      48     203
    ## 671 2003  February      26      51     231      24      48     203
    ## 672 2003   January      26      51     231      24      48     203
    ## 673 2003      July      26      51     231      24      48     203
    ## 674 2003      June      26      51     231      24      48     203
    ## 675 2003     March      26      51     231      24      48     203
    ## 676 2003       May      26      51     231      24      48     203
    ## 677 2003  November      27      51     231      25      48     203
    ## 678 2003   October      26      51     231      25      48     203
    ## 679 2003 September      26      51     231      25      48     203
    ## 680 2004     April      28      51     229      22      48     204
    ## 681 2004    August      29      51     229      22      48     205
    ## 682 2004  December      29      51     229      23      48     205
    ## 683 2004  February      28      51     229      22      48     203
    ## 684 2004   January      28      51     229      22      48     203
    ## 685 2004      July      29      51     229      22      48     204
    ## 686 2004      June      28      51     229      22      48     204
    ## 687 2004     March      28      51     229      22      48     204
    ## 688 2004       May      28      51     229      22      48     204
    ## 689 2004  November      29      51     229      23      48     205
    ## 690 2004   October      29      51     229      22      48     205
    ## 691 2004 September      29      51     229      22      48     205
    ## 692 2005     April      28      54     232      22      45     202
    ## 693 2005    August      28      54     231      22      45     202
    ## 694 2005  December      28      54     232      22      45     202
    ## 695 2005  February      28      54     232      22      45     201
    ## 696 2005   January      28      54     232      22      45     201
    ## 697 2005      July      28      54     231      22      45     202
    ## 698 2005      June      28      54     231      22      45     202
    ## 699 2005     March      28      54     232      22      45     202
    ## 700 2005       May      28      54     231      22      45     202
    ## 701 2005  November      28      54     232      22      45     202
    ## 702 2005   October      28      54     232      22      45     202
    ## 703 2005 September      28      54     232      22      45     202
    ## 704 2006     April      28      54     231      22      45     201
    ## 705 2006    August      28      54     231      22      45     201
    ## 706 2006  December      28      54     232      22      45     202
    ## 707 2006  February      28      54     231      22      45     201
    ## 708 2006   January      28      54     231      22      45     201
    ## 709 2006      July      28      54     231      22      45     201
    ## 710 2006      June      28      54     231      22      45     201
    ## 711 2006     March      28      54     231      22      45     201
    ## 712 2006       May      28      54     231      22      45     201
    ## 713 2006  November      28      54     232      22      45     202
    ## 714 2006   October      28      54     231      22      45     201
    ## 715 2006 September      28      54     231      22      45     201
    ## 716 2007     April      22      48     201      28      50     233
    ## 717 2007    August      22      48     202      28      50     232
    ## 718 2007  December      22      48     202      28      50     234
    ## 719 2007  February      22      48     201      28      50     233
    ## 720 2007   January      22      48     201      28      50     233
    ## 721 2007      July      22      48     201      28      50     232
    ## 722 2007      June      22      47     201      28      50     232
    ## 723 2007     March      22      48     201      28      50     233
    ## 724 2007       May      22      48     201      28      50     232
    ## 725 2007  November      22      48     202      28      50     234
    ## 726 2007   October      22      48     202      28      50     233
    ## 727 2007 September      22      48     202      28      50     233
    ## 728 2008     April      22      48     198      28      50     234
    ## 729 2008    August      22      48     199      28      50     236
    ## 730 2008  December      22      48     199      28      50     236
    ## 731 2008  February      22      48     198      28      50     231
    ## 732 2008   January      22      48     198      28      50     231
    ## 733 2008      July      22      48     199      28      50     236
    ## 734 2008      June      22      48     199      28      50     235
    ## 735 2008     March      22      48     198      28      50     233
    ## 736 2008       May      22      48     199      28      50     235
    ## 737 2008  November      22      48     199      28      50     236
    ## 738 2008   October      22      48     199      28      50     236
    ## 739 2008 September      22      48     199      28      50     236
    ## 740 2009     April      22      40     179      28      57     254
    ## 741 2009    August      24      40     179      28      58     255
    ## 742 2009  December      24      41     179      28      59     257
    ## 743 2009  February      22      40     179      28      57     254
    ## 744 2009   January      22      40     179      28      57     254
    ## 745 2009      July      22      40     179      28      58     254
    ## 746 2009      June      22      40     179      28      57     255
    ## 747 2009     March      22      40     179      28      57     253
    ## 748 2009       May      22      40     179      28      57     255
    ## 749 2009  November      24      41     179      28      59     257
    ## 750 2009   October      24      41     179      28      59     255
    ## 751 2009 September      24      41     179      28      58     255
    ## 752 2010     April      24      41     177      26      57     254
    ## 753 2010    August      24      41     178      26      57     255
    ## 754 2010  December      24      41     178      27      59     255
    ## 755 2010  February      24      41     178      26      57     255
    ## 756 2010   January      24      41     178      26      57     255
    ## 757 2010      July      24      41     178      26      56     255
    ## 758 2010      June      24      41     178      26      57     255
    ## 759 2010     March      24      41     178      26      57     253
    ## 760 2010       May      24      41     177      26      57     254
    ## 761 2010  November      24      41     178      27      59     255
    ## 762 2010   October      24      41     178      26      57     255
    ## 763 2010 September      24      41     178      26      57     255
    ## 764 2011     April      29      47     241      21      51     192
    ## 765 2011    August      29      47     240      21      51     193
    ## 766 2011  December      29      47     242      21      51     193
    ## 767 2011  February      29      47     241      21      51     193
    ## 768 2011   January      29      47     241      21      51     193
    ## 769 2011      July      29      47     240      21      51     192
    ## 770 2011      June      29      47     240      21      51     193
    ## 771 2011     March      29      47     241      21      51     192
    ## 772 2011       May      29      47     240      21      51     192
    ## 773 2011  November      29      47     242      21      51     193
    ## 774 2011   October      29      47     242      21      51     193
    ## 775 2011 September      29      47     242      21      51     193
    ## 776 2012     April      29      47     242      21      51     190
    ## 777 2012    August      29      47     242      21      51     191
    ## 778 2012  December      29      47     243      21      51     194
    ## 779 2012  February      29      47     242      21      51     192
    ## 780 2012   January      29      47     242      21      51     192
    ## 781 2012      July      29      47     242      21      51     191
    ## 782 2012      June      29      47     242      21      51     191
    ## 783 2012     March      29      47     242      21      51     191
    ## 784 2012       May      29      47     242      21      51     190
    ## 785 2012  November      29      47     243      21      51     194
    ## 786 2012   October      29      47     242      21      51     191
    ## 787 2012 September      29      47     242      21      51     191
    ## 788 2013     April      30      45     232      20      53     201
    ## 789 2013    August      30      46     234      20      53     201
    ## 790 2013  December      30      46     234      20      54     201
    ## 791 2013  February      30      45     232      20      53     200
    ## 792 2013   January      30      45     232      20      53     200
    ## 793 2013      July      30      46     234      20      52     201
    ## 794 2013      June      30      46     234      20      52     201
    ## 795 2013     March      30      45     232      20      53     200
    ## 796 2013       May      30      45     233      20      53     201
    ## 797 2013  November      30      46     234      20      54     201
    ## 798 2013   October      30      46     234      20      53     201
    ## 799 2013 September      30      46     234      20      53     201
    ## 800 2014     April      29      45     233      21      53     199
    ## 801 2014    August      29      45     234      21      53     199
    ## 802 2014  December      29      45     235      21      53     201
    ## 803 2014  February      29      45     232      21      53     200
    ## 804 2014   January      29      45     232      21      53     200
    ## 805 2014      July      29      45     234      21      53     199
    ## 806 2014      June      29      45     233      21      53     199
    ## 807 2014     March      29      45     233      21      53     199
    ## 808 2014       May      29      45     233      21      53     199
    ## 809 2014  November      29      45     235      21      53     201
    ## 810 2014   October      29      45     234      21      53     199
    ## 811 2014 September      29      45     234      21      53     199
    ## 812 2015     April      31      54     244      18      44     188
    ## 813 2015  February      31      54     245      18      44     188
    ## 814 2015   January      31      54     245      18      44     188
    ## 815 2015      June      31      54     246      18      44     188
    ## 816 2015     March      31      54     245      18      44     188
    ## 817 2015       May      31      54     245      18      44     188
    ##     president   close rate
    ## 1         dem      NA   NA
    ## 2         dem      NA   NA
    ## 3         dem      NA   NA
    ## 4         dem      NA   NA
    ## 5         dem      NA   NA
    ## 6         dem      NA   NA
    ## 7         dem      NA   NA
    ## 8         dem      NA   NA
    ## 9         dem      NA   NA
    ## 10        dem      NA   NA
    ## 11        dem      NA   NA
    ## 12        dem      NA   NA
    ## 13        dem      NA  3.9
    ## 14        dem      NA  3.9
    ## 15        dem      NA  4.0
    ## 16        dem      NA  3.8
    ## 17        dem      NA  3.4
    ## 18        dem      NA  3.6
    ## 19        dem      NA  3.6
    ## 20        dem      NA  4.0
    ## 21        dem      NA  3.5
    ## 22        dem      NA  3.8
    ## 23        dem      NA  3.7
    ## 24        dem      NA  3.8
    ## 25        dem      NA  5.3
    ## 26        dem      NA  6.8
    ## 27        dem      NA  6.6
    ## 28        dem      NA  4.7
    ## 29        dem      NA  4.3
    ## 30        dem      NA  6.7
    ## 31        dem      NA  6.2
    ## 32        dem      NA  5.0
    ## 33        dem      NA  6.1
    ## 34        dem      NA  6.4
    ## 35        dem      NA  7.9
    ## 36        dem      NA  6.6
    ## 37        dem   17.96  5.8
    ## 38        dem   18.42  4.5
    ## 39        dem   20.43  4.3
    ## 40        dem   17.22  6.4
    ## 41        dem   17.05  6.5
    ## 42        dem   17.84  5.0
    ## 43        dem   17.69  5.4
    ## 44        dem   17.29  6.3
    ## 45        dem   18.78  5.5
    ## 46        dem   19.51  4.2
    ## 47        dem   19.53  4.2
    ## 48        dem   19.45  4.4
    ## 49        dem   22.43  3.1
    ## 50        dem   23.28  3.1
    ## 51        dem   23.77  3.1
    ## 52        dem   21.80  3.4
    ## 53        dem   21.66  3.7
    ## 54        dem   22.40  3.1
    ## 55        dem   20.96  3.2
    ## 56        dem   21.48  3.4
    ## 57        dem   21.52  3.0
    ## 58        dem   22.88  3.5
    ## 59        dem   22.94  3.5
    ## 60        dem   23.26  3.3
    ## 61        dem   23.32  2.9
    ## 62        dem   25.03  3.4
    ## 63        dem   26.57  2.7
    ## 64        dem   23.26  3.1
    ## 65        dem   24.14  3.2
    ## 66        dem   25.40  3.2
    ## 67        dem   24.96  3.0
    ## 68        dem   24.37  2.9
    ## 69        dem   23.86  3.0
    ## 70        dem   25.66  2.8
    ## 71        dem   24.52  3.0
    ## 72        dem   24.54  3.1
    ## 73        gop   24.62  2.7
    ## 74        gop   23.32  2.7
    ## 75        gop   24.81  4.5
    ## 76        gop   25.90  2.6
    ## 77        gop   26.38  2.9
    ## 78        gop   24.75  2.6
    ## 79        gop   24.14  2.5
    ## 80        gop   25.29  2.6
    ## 81        gop   24.54  2.5
    ## 82        gop   24.76  3.5
    ## 83        gop   24.54  3.1
    ## 84        gop   23.35  2.9
    ## 85        gop   28.26  5.9
    ## 86        gop   29.83  6.0
    ## 87        gop   35.98  5.0
    ## 88        gop   26.15  5.2
    ## 89        gop   26.08  4.9
    ## 90        gop   30.88  5.8
    ## 91        gop   29.21  5.6
    ## 92        gop   26.94  5.7
    ## 93        gop   29.19  5.9
    ## 94        gop   34.24  5.3
    ## 95        gop   31.68  5.7
    ## 96        gop   32.31  6.1
    ## 97        gop   37.96  4.7
    ## 98        gop   43.18  4.2
    ## 99        gop   45.48  4.2
    ## 100       gop   36.76  4.7
    ## 101       gop   36.63  4.9
    ## 102       gop   43.52  4.0
    ## 103       gop   41.03  4.2
    ## 104       gop   36.58  4.6
    ## 105       gop   37.91  4.3
    ## 106       gop   45.51  4.2
    ## 107       gop   42.34  4.3
    ## 108       gop   43.67  4.1
    ## 109       gop   48.38  4.0
    ## 110       gop   47.51  4.1
    ## 111       gop   46.67  4.2
    ## 112       gop   45.34  3.9
    ## 113       gop   43.82  4.0
    ## 114       gop   49.39  4.4
    ## 115       gop   46.97  4.3
    ## 116       gop   48.48  4.2
    ## 117       gop   45.20  4.3
    ## 118       gop   45.08  4.3
    ## 119       gop   45.58  3.9
    ## 120       gop   45.35  3.9
    ## 121       gop   45.74  3.9
    ## 122       gop   45.22  4.1
    ## 123       gop   39.99  5.2
    ## 124       gop   43.26  3.9
    ## 125       gop   44.72  4.2
    ## 126       gop   47.91  4.2
    ## 127       gop   47.37  4.3
    ## 128       gop   44.11  3.7
    ## 129       gop   47.43  4.1
    ## 130       gop   41.72  5.1
    ## 131       gop   41.06  4.5
    ## 132       gop   42.42  4.4
    ## 133       gop   43.44  7.4
    ## 134       gop   47.75  7.4
    ## 135       gop   55.21  6.2
    ## 136       gop   40.84  6.4
    ## 137       gop   41.70  5.8
    ## 138       gop   47.19  7.5
    ## 139       gop   45.24  7.3
    ## 140       gop   42.10  6.7
    ## 141       gop   44.09  7.4
    ## 142       gop   52.48  6.2
    ## 143       gop   51.33  6.7
    ## 144       gop   50.06  7.1
    ## 145       gop   57.59  5.2
    ## 146       gop   59.60  5.2
    ## 147       gop   59.89  5.3
    ## 148       gop   55.41  5.9
    ## 149       gop   55.45  6.0
    ## 150       gop   60.51  5.1
    ## 151       gop   58.47  5.0
    ## 152       gop   55.44  5.6
    ## 153       gop   58.68  5.1
    ## 154       gop   58.28  5.8
    ## 155       gop   57.52  5.7
    ## 156       gop   56.88  5.5
    ## 157       gop   54.37  5.2
    ## 158       gop   56.96  5.6
    ## 159       gop   58.11  6.6
    ## 160       gop   56.12  4.8
    ## 161       gop   55.61  5.2
    ## 162       gop   55.51  5.5
    ## 163       gop   56.92  5.4
    ## 164       gop   55.34  5.4
    ## 165       gop   55.83  5.1
    ## 166       gop   55.54  6.1
    ## 167       gop   53.39  6.1
    ## 168       gop   53.52  5.5
    ## 169       dem   65.31  7.0
    ## 170       dem   68.07  6.6
    ## 171       dem   71.55  6.0
    ## 172       dem   63.44  6.9
    ## 173       dem   61.78  6.6
    ## 174       dem   66.76  7.0
    ## 175       dem   64.64  6.9
    ## 176       dem   65.06  6.9
    ## 177       dem   66.56  7.1
    ## 178       dem   71.32  6.1
    ## 179       dem   68.62  6.5
    ## 180       dem   66.73  6.7
    ## 181       dem   65.24  5.6
    ## 182       dem   59.12  5.7
    ## 183       dem   63.10  5.5
    ## 184       dem   69.96  5.5
    ## 185       dem   68.84  5.8
    ## 186       dem   58.23  5.4
    ## 187       dem   54.75  5.5
    ## 188       dem   69.55  5.6
    ## 189       dem   59.63  5.5
    ## 190       dem   62.26  5.7
    ## 191       dem   56.52  5.4
    ## 192       dem   56.27  5.6
    ## 193       dem   69.80  5.7
    ## 194       dem   72.50  5.4
    ## 195       dem   75.02  5.5
    ## 196       dem   64.29  5.9
    ## 197       dem   66.20  5.7
    ## 198       dem   69.13  5.6
    ## 199       dem   69.37  5.6
    ## 200       dem   66.57  5.7
    ## 201       dem   70.80  5.9
    ## 202       dem   73.23  5.7
    ## 203       dem   74.01  5.5
    ## 204       dem   71.70  5.5
    ## 205       dem   79.46  5.3
    ## 206       dem   81.83  5.0
    ## 207       dem   84.75  5.0
    ## 208       dem   77.80  5.4
    ## 209       dem   77.04  5.6
    ## 210       dem   83.18  4.9
    ## 211       dem   81.69  5.2
    ## 212       dem   78.98  5.4
    ## 213       dem   80.37  5.1
    ## 214       dem   84.42  4.8
    ## 215       dem   84.86  5.1
    ## 216       dem   84.18  5.1
    ## 217       dem   89.11  4.8
    ## 218       dem   87.17  4.4
    ## 219       dem   92.43  4.0
    ## 220       dem   87.43  5.1
    ## 221       dem   87.56  4.9
    ## 222       dem   85.25  4.4
    ## 223       dem   84.12  4.6
    ## 224       dem   86.16  4.7
    ## 225       dem   88.42  4.6
    ## 226       dem   91.61  4.1
    ## 227       dem   92.42  4.2
    ## 228       dem   89.96  4.3
    ## 229       dem   91.06  3.8
    ## 230       dem   77.10  3.8
    ## 231       dem   80.33  3.8
    ## 232       dem   91.22  3.8
    ## 233       dem   92.88  4.0
    ## 234       dem   83.60  3.8
    ## 235       dem   84.74  3.8
    ## 236       dem   89.23  3.8
    ## 237       dem   86.13  3.9
    ## 238       dem   80.45  3.6
    ## 239       dem   80.20  3.7
    ## 240       dem   76.56  3.7
    ## 241       dem   94.01  3.8
    ## 242       dem   93.64  3.8
    ## 243       dem   96.47  3.8
    ## 244       dem   86.78  3.8
    ## 245       dem   86.61  3.9
    ## 246       dem   94.75  3.8
    ## 247       dem   90.64  3.9
    ## 248       dem   90.20  3.8
    ## 249       dem   89.08  3.8
    ## 250       dem   94.00  3.9
    ## 251       dem   93.30  4.0
    ## 252       dem   96.71  3.8
    ## 253       dem   97.46  3.5
    ## 254       dem   98.86  3.5
    ## 255       dem  103.86  3.4
    ## 256       dem   89.36  3.8
    ## 257       dem   92.24  3.7
    ## 258       dem   97.74  3.7
    ## 259       dem   99.58  3.7
    ## 260       dem   90.20  3.7
    ## 261       dem   98.68  3.5
    ## 262       dem  108.37  3.4
    ## 263       dem  103.41  3.4
    ## 264       dem  102.67  3.4
    ## 265       gop  103.69  3.4
    ## 266       gop   95.51  3.5
    ## 267       gop   92.06  3.5
    ## 268       gop   98.13  3.4
    ## 269       gop  103.01  3.4
    ## 270       gop   91.83  3.5
    ## 271       gop   97.71  3.5
    ## 272       gop  101.51  3.4
    ## 273       gop  103.46  3.4
    ## 274       gop   93.81  3.5
    ## 275       gop   97.12  3.7
    ## 276       gop   93.12  3.7
    ## 277       gop   81.52  4.6
    ## 278       gop   81.52  5.1
    ## 279       gop   92.15  6.1
    ## 280       gop   89.50  4.2
    ## 281       gop   85.02  3.9
    ## 282       gop   78.05  5.0
    ## 283       gop   72.72  4.9
    ## 284       gop   89.63  4.4
    ## 285       gop   76.55  4.8
    ## 286       gop   87.20  5.9
    ## 287       gop   83.25  5.5
    ## 288       gop   84.30  5.4
    ## 289       gop  103.95  5.9
    ## 290       gop   99.03  6.1
    ## 291       gop  102.09  6.0
    ## 292       gop   96.75  5.9
    ## 293       gop   95.88  5.9
    ## 294       gop   95.58  6.0
    ## 295       gop   98.70  5.9
    ## 296       gop  100.31  6.0
    ## 297       gop   99.63  5.9
    ## 298       gop   93.99  6.0
    ## 299       gop   94.23  5.8
    ## 300       gop   98.34  6.0
    ## 301       gop  107.67  5.7
    ## 302       gop  111.09  5.6
    ## 303       gop  118.05  5.2
    ## 304       gop  106.57  5.7
    ## 305       gop  103.94  5.8
    ## 306       gop  107.39  5.6
    ## 307       gop  107.14  5.7
    ## 308       gop  107.20  5.8
    ## 309       gop  109.53  5.7
    ## 310       gop  116.67  5.3
    ## 311       gop  111.58  5.6
    ## 312       gop  110.55  5.5
    ## 313       gop  106.97  5.0
    ## 314       gop  104.25  4.8
    ## 315       gop   97.55  4.9
    ## 316       gop  111.68  5.0
    ## 317       gop  116.03  4.9
    ## 318       gop  108.22  4.8
    ## 319       gop  104.26  4.9
    ## 320       gop  111.52  4.9
    ## 321       gop  104.95  4.9
    ## 322       gop   95.96  4.8
    ## 323       gop  108.29  4.6
    ## 324       gop  108.43  4.8
    ## 325       gop   90.31  5.1
    ## 326       gop   96.22  5.2
    ## 327       gop   96.57  5.1
    ## 328       gop   79.31  5.5
    ## 329       gop   86.00  5.4
    ## 330       gop   93.98  5.1
    ## 331       gop   87.28  5.1
    ## 332       gop   87.30  8.8
    ## 333       gop   86.88  8.4
    ## 334       gop   90.19  8.2
    ## 335       gop   81.59  8.1
    ## 336       gop   76.98  8.1
    ## 337       gop   88.75  8.6
    ## 338       gop   95.19  8.8
    ## 339       gop   83.36  8.6
    ## 340       gop   91.15  9.0
    ## 341       gop   91.24  8.3
    ## 342       gop   89.04  8.4
    ## 343       gop   83.87  8.4
    ## 344       gop  101.64  7.7
    ## 345       gop  102.91  7.8
    ## 346       gop  107.46  7.8
    ## 347       gop   99.71  7.7
    ## 348       gop  100.86  7.9
    ## 349       gop  103.44  7.8
    ## 350       gop  104.28  7.6
    ## 351       gop  102.77  7.6
    ## 352       gop  100.18  7.4
    ## 353       gop  102.10  7.8
    ## 354       gop  102.90  7.7
    ## 355       gop  105.24  7.6
    ## 356       dem   98.44  7.2
    ## 357       dem   96.77  7.0
    ## 358       dem   95.10  6.4
    ## 359       dem   99.82  7.6
    ## 360       dem  102.03  7.5
    ## 361       dem   98.85  6.9
    ## 362       dem  100.48  7.2
    ## 363       dem   98.42  7.4
    ## 364       dem   96.12  7.0
    ## 365       dem   94.83  6.8
    ## 366       dem   92.34  6.8
    ## 367       dem   96.53  6.8
    ## 368       dem   96.83  6.1
    ## 369       dem  103.29  5.9
    ## 370       dem   96.11  6.0
    ## 371       dem   87.04  6.3
    ## 372       dem   89.25  6.4
    ## 373       dem  100.68  6.2
    ## 374       dem   95.53  5.9
    ## 375       dem   89.21  6.3
    ## 376       dem   97.24  6.0
    ## 377       dem   94.70  5.9
    ## 378       dem   93.15  5.8
    ## 379       dem  102.54  6.0
    ## 380       dem  101.76  5.8
    ## 381       dem  109.32  6.0
    ## 382       dem  107.94  6.0
    ## 383       dem   96.28  5.9
    ## 384       dem   99.93  5.9
    ## 385       dem  103.81  5.7
    ## 386       dem  102.91  5.7
    ## 387       dem  101.59  5.8
    ## 388       dem   99.08  5.6
    ## 389       dem  106.16  5.9
    ## 390       dem  101.82  6.0
    ## 391       dem  109.32  5.9
    ## 392       dem  106.29  6.9
    ## 393       dem  122.38  7.7
    ## 394       dem  135.76  7.2
    ## 395       dem  113.66  6.3
    ## 396       dem  114.16  6.3
    ## 397       dem  121.67  7.8
    ## 398       dem  114.24  7.6
    ## 399       dem  102.09  6.3
    ## 400       dem  111.24  7.5
    ## 401       dem  140.52  7.5
    ## 402       dem  127.47  7.5
    ## 403       dem  125.46  7.5
    ## 404       gop  132.81  7.2
    ## 405       gop  122.79  7.4
    ## 406       gop  122.55  8.5
    ## 407       gop  131.27  7.4
    ## 408       gop  129.55  7.5
    ## 409       gop  130.92  7.2
    ## 410       gop  131.21  7.5
    ## 411       gop  136.00  7.4
    ## 412       gop  132.59  7.5
    ## 413       gop  126.35  8.3
    ## 414       gop  121.89  7.9
    ## 415       gop  116.18  7.6
    ## 416       gop  116.44  9.3
    ## 417       gop  119.51  9.8
    ## 418       gop  140.64 10.8
    ## 419       gop  113.11  8.9
    ## 420       gop  120.40  8.6
    ## 421       gop  107.09  9.8
    ## 422       gop  109.61  9.6
    ## 423       gop  111.96  9.0
    ## 424       gop  111.88  9.4
    ## 425       gop  138.53 10.8
    ## 426       gop  133.72 10.4
    ## 427       gop  120.42 10.1
    ## 428       gop  164.43 10.2
    ## 429       gop  164.40  9.5
    ## 430       gop  164.93  8.3
    ## 431       gop  148.06 10.4
    ## 432       gop  145.30 10.4
    ## 433       gop  162.56  9.4
    ## 434       gop  167.64 10.1
    ## 435       gop  152.96 10.3
    ## 436       gop  162.39 10.1
    ## 437       gop  166.40  8.5
    ## 438       gop  163.55  8.8
    ## 439       gop  166.07  9.2
    ## 440       gop  160.05  7.7
    ## 441       gop  166.68  7.5
    ## 442       gop  167.24  7.3
    ## 443       gop  157.06  7.8
    ## 444       gop  163.41  8.0
    ## 445       gop  150.66  7.5
    ## 446       gop  153.18  7.2
    ## 447       gop  159.18  7.8
    ## 448       gop  150.55  7.4
    ## 449       gop  163.58  7.2
    ## 450       gop  166.09  7.4
    ## 451       gop  166.10  7.3
    ## 452       gop  179.83  7.3
    ## 453       gop  188.63  7.1
    ## 454       gop  211.28  7.0
    ## 455       gop  181.18  7.2
    ## 456       gop  179.63  7.3
    ## 457       gop  190.92  7.4
    ## 458       gop  191.85  7.4
    ## 459       gop  180.66  7.2
    ## 460       gop  189.55  7.2
    ## 461       gop  202.17  7.0
    ## 462       gop  189.82  7.1
    ## 463       gop  182.08  7.1
    ## 464       gop  235.52  7.1
    ## 465       gop  252.93  6.9
    ## 466       gop  242.17  6.6
    ## 467       gop  226.92  7.2
    ## 468       gop  211.78  6.7
    ## 469       gop  236.12  7.0
    ## 470       gop  250.84  7.2
    ## 471       gop  238.90  7.2
    ## 472       gop  247.35  7.2
    ## 473       gop  249.22  6.9
    ## 474       gop  243.98  7.0
    ## 475       gop  231.32  7.0
    ## 476       gop  288.36  6.3
    ## 477       gop  329.80  6.0
    ## 478       gop  247.08  5.7
    ## 479       gop  284.20  6.6
    ## 480       gop  274.08  6.6
    ## 481       gop  318.66  6.1
    ## 482       gop  304.00  6.2
    ## 483       gop  291.70  6.6
    ## 484       gop  290.10  6.3
    ## 485       gop  230.30  5.8
    ## 486       gop  251.79  6.0
    ## 487       gop  321.83  5.9
    ## 488       gop  261.33  5.4
    ## 489       gop  261.52  5.6
    ## 490       gop  277.72  5.3
    ## 491       gop  267.82  5.7
    ## 492       gop  257.07  5.7
    ## 493       gop  272.02  5.4
    ## 494       gop  273.50  5.4
    ## 495       gop  258.89  5.7
    ## 496       gop  262.16  5.6
    ## 497       gop  273.70  5.3
    ## 498       gop  278.97  5.4
    ## 499       gop  271.91  5.4
    ## 500       gop  309.64  5.2
    ## 501       gop  351.45  5.2
    ## 502       gop  353.40  5.4
    ## 503       gop  288.86  5.2
    ## 504       gop  297.47  5.4
    ## 505       gop  346.08  5.2
    ## 506       gop  317.98  5.3
    ## 507       gop  294.87  5.0
    ## 508       gop  320.52  5.2
    ## 509       gop  345.99  5.4
    ## 510       gop  340.36  5.3
    ## 511       gop  349.15  5.3
    ## 512       gop  330.80  5.4
    ## 513       gop  322.56  5.7
    ## 514       gop  330.22  6.3
    ## 515       gop  331.89  5.3
    ## 516       gop  329.08  5.4
    ## 517       gop  356.15  5.5
    ## 518       gop  358.02  5.2
    ## 519       gop  339.94  5.2
    ## 520       gop  361.23  5.4
    ## 521       gop  322.22  6.2
    ## 522       gop  304.00  5.9
    ## 523       gop  306.05  5.9
    ## 524       gop  375.34  6.7
    ## 525       gop  395.43  6.9
    ## 526       gop  417.09  7.3
    ## 527       gop  367.07  6.6
    ## 528       gop  343.93  6.4
    ## 529       gop  387.81  6.8
    ## 530       gop  371.16  6.9
    ## 531       gop  375.22  6.8
    ## 532       gop  389.83  6.9
    ## 533       gop  375.22  7.0
    ## 534       gop  392.45  7.0
    ## 535       gop  387.86  6.9
    ## 536       gop  414.95  7.4
    ## 537       gop  414.03  7.6
    ## 538       gop  435.71  7.4
    ## 539       gop  412.70  7.4
    ## 540       gop  408.78  7.3
    ## 541       gop  424.21  7.7
    ## 542       gop  408.14  7.8
    ## 543       gop  403.69  7.4
    ## 544       gop  415.35  7.6
    ## 545       gop  431.35  7.4
    ## 546       gop  418.68  7.3
    ## 547       gop  417.80  7.6
    ## 548       dem  440.19  7.1
    ## 549       dem  463.56  6.8
    ## 550       dem  466.45  6.5
    ## 551       dem  443.38  7.1
    ## 552       dem  438.78  7.3
    ## 553       dem  448.13  6.9
    ## 554       dem  450.53  7.0
    ## 555       dem  451.67  7.0
    ## 556       dem  450.19  7.1
    ## 557       dem  461.79  6.6
    ## 558       dem  467.83  6.8
    ## 559       dem  458.93  6.7
    ## 560       dem  450.91  6.4
    ## 561       dem  475.49  6.0
    ## 562       dem  459.27  5.5
    ## 563       dem  467.14  6.6
    ## 564       dem  481.61  6.6
    ## 565       dem  458.26  6.1
    ## 566       dem  444.27  6.1
    ## 567       dem  445.77  6.5
    ## 568       dem  456.50  6.1
    ## 569       dem  453.69  5.6
    ## 570       dem  472.35  5.8
    ## 571       dem  462.71  5.9
    ## 572       dem  514.71  5.8
    ## 573       dem  561.88  5.7
    ## 574       dem  615.93  5.6
    ## 575       dem  487.39  5.4
    ## 576       dem  470.42  5.6
    ## 577       dem  562.06  5.7
    ## 578       dem  544.75  5.6
    ## 579       dem  500.71  5.4
    ## 580       dem  533.40  5.6
    ## 581       dem  605.37  5.6
    ## 582       dem  581.50  5.5
    ## 583       dem  584.41  5.6
    ## 584       dem  654.17  5.6
    ## 585       dem  651.99  5.1
    ## 586       dem  740.74  5.4
    ## 587       dem  640.43  5.5
    ## 588       dem  636.02  5.6
    ## 589       dem  639.95  5.5
    ## 590       dem  670.63  5.3
    ## 591       dem  645.50  5.5
    ## 592       dem  669.12  5.6
    ## 593       dem  757.02  5.4
    ## 594       dem  705.27  5.2
    ## 595       dem  687.33  5.2
    ## 596       dem  801.34  5.1
    ## 597       dem  899.47  4.8
    ## 598       dem  970.43  4.7
    ## 599       dem  790.82  5.2
    ## 600       dem  786.16  5.3
    ## 601       dem  954.31  4.9
    ## 602       dem  885.14  5.0
    ## 603       dem  757.12  5.2
    ## 604       dem  848.28  4.9
    ## 605       dem  955.40  4.6
    ## 606       dem  914.62  4.7
    ## 607       dem  947.28  4.9
    ## 608       dem 1111.75  4.3
    ## 609       dem  957.28  4.5
    ## 610       dem 1229.23  4.4
    ## 611       dem 1049.34  4.6
    ## 612       dem  980.28  4.6
    ## 613       dem 1120.67  4.5
    ## 614       dem 1133.84  4.5
    ## 615       dem 1101.75  4.7
    ## 616       dem 1090.82  4.4
    ## 617       dem 1163.63  4.4
    ## 618       dem 1098.67  4.5
    ## 619       dem 1017.01  4.6
    ## 620       dem 1335.18  4.3
    ## 621       dem 1320.41  4.2
    ## 622       dem 1469.25  4.0
    ## 623       dem 1238.33  4.4
    ## 624       dem 1279.64  4.3
    ## 625       dem 1328.72  4.3
    ## 626       dem 1372.71  4.3
    ## 627       dem 1286.37  4.2
    ## 628       dem 1301.84  4.2
    ## 629       dem 1388.91  4.1
    ## 630       dem 1362.93  4.1
    ## 631       dem 1282.71  4.2
    ## 632       dem 1452.43  3.8
    ## 633       dem 1517.68  4.1
    ## 634       dem 1320.28  3.9
    ## 635       dem 1366.42  4.1
    ## 636       dem 1394.46  4.0
    ## 637       dem 1430.83  4.0
    ## 638       dem 1454.60  4.0
    ## 639       dem 1498.58  4.0
    ## 640       dem 1420.60  4.0
    ## 641       dem 1314.95  3.9
    ## 642       dem 1429.40  3.9
    ## 643       dem 1436.51  3.9
    ## 644       gop 1249.46  4.4
    ## 645       gop 1133.58  4.9
    ## 646       gop 1148.08  5.7
    ## 647       gop 1239.94  4.2
    ## 648       gop 1366.01  4.2
    ## 649       gop 1211.23  4.6
    ## 650       gop 1224.38  4.5
    ## 651       gop 1160.33  4.3
    ## 652       gop 1255.82  4.3
    ## 653       gop 1139.45  5.5
    ## 654       gop 1059.78  5.3
    ## 655       gop 1040.94  5.0
    ## 656       gop 1076.92  5.9
    ## 657       gop  916.07  5.7
    ## 658       gop  879.82  6.0
    ## 659       gop 1106.73  5.7
    ## 660       gop 1130.20  5.7
    ## 661       gop  911.62  5.8
    ## 662       gop  989.82  5.8
    ## 663       gop 1147.39  5.7
    ## 664       gop 1067.14  5.8
    ## 665       gop  936.31  5.9
    ## 666       gop  885.76  5.7
    ## 667       gop  815.28  5.7
    ## 668       gop  916.92  6.0
    ## 669       gop 1008.01  6.1
    ## 670       gop 1111.92  5.7
    ## 671       gop  841.15  5.9
    ## 672       gop  855.70  5.8
    ## 673       gop  990.31  6.2
    ## 674       gop  974.50  6.3
    ## 675       gop  848.18  5.9
    ## 676       gop  963.59  6.1
    ## 677       gop 1058.20  5.8
    ## 678       gop 1050.71  6.0
    ## 679       gop  995.97  6.1
    ## 680       gop 1107.30  5.6
    ## 681       gop 1104.24  5.4
    ## 682       gop 1211.92  5.4
    ## 683       gop 1144.94  5.6
    ## 684       gop 1131.13  5.7
    ## 685       gop 1101.72  5.5
    ## 686       gop 1140.84  5.6
    ## 687       gop 1126.21  5.8
    ## 688       gop 1120.68  5.6
    ## 689       gop 1173.82  5.4
    ## 690       gop 1130.20  5.5
    ## 691       gop 1114.58  5.4
    ## 692       gop 1156.85  5.2
    ## 693       gop 1220.33  4.9
    ## 694       gop 1248.29  4.9
    ## 695       gop 1203.60  5.4
    ## 696       gop 1181.27  5.3
    ## 697       gop 1234.18  5.0
    ## 698       gop 1191.33  5.0
    ## 699       gop 1180.59  5.2
    ## 700       gop 1191.50  5.1
    ## 701       gop 1249.48  5.0
    ## 702       gop 1207.01  5.0
    ## 703       gop 1228.81  5.0
    ## 704       gop 1310.61  4.7
    ## 705       gop 1303.82  4.7
    ## 706       gop 1418.30  4.4
    ## 707       gop 1280.66  4.8
    ## 708       gop 1280.08  4.7
    ## 709       gop 1276.66  4.7
    ## 710       gop 1270.20  4.6
    ## 711       gop 1294.87  4.7
    ## 712       gop 1270.09  4.6
    ## 713       gop 1400.63  4.5
    ## 714       gop 1377.94  4.4
    ## 715       gop 1335.85  4.5
    ## 716       gop 1482.37  4.5
    ## 717       gop 1473.99  4.6
    ## 718       gop 1468.36  5.0
    ## 719       gop 1406.82  4.5
    ## 720       gop 1438.24  4.6
    ## 721       gop 1455.27  4.7
    ## 722       gop 1503.35  4.6
    ## 723       gop 1420.86  4.4
    ## 724       gop 1530.62  4.4
    ## 725       gop 1481.14  4.7
    ## 726       gop 1549.38  4.7
    ## 727       gop 1526.75  4.7
    ## 728       gop 1385.59  5.0
    ## 729       gop 1282.83  6.1
    ## 730       gop  903.25  7.3
    ## 731       gop 1330.63  4.9
    ## 732       gop 1378.55  5.0
    ## 733       gop 1267.38  5.8
    ## 734       gop 1280.00  5.6
    ## 735       gop 1322.70  5.1
    ## 736       gop 1400.38  5.4
    ## 737       gop  896.24  6.8
    ## 738       gop  968.75  6.5
    ## 739       gop 1166.36  6.1
    ## 740       dem  872.81  9.0
    ## 741       dem 1020.62  9.6
    ## 742       dem 1115.10  9.9
    ## 743       dem  735.09  8.3
    ## 744       dem  825.88  7.8
    ## 745       dem  987.48  9.5
    ## 746       dem  919.32  9.5
    ## 747       dem  797.87  8.7
    ## 748       dem  919.14  9.4
    ## 749       dem 1095.63  9.9
    ## 750       dem 1036.19 10.0
    ## 751       dem 1057.08  9.8
    ## 752       dem 1186.69  9.9
    ## 753       dem 1049.33  9.5
    ## 754       dem 1257.64  9.3
    ## 755       dem 1104.49  9.8
    ## 756       dem 1073.87  9.8
    ## 757       dem 1101.60  9.4
    ## 758       dem 1030.71  9.4
    ## 759       dem 1169.43  9.9
    ## 760       dem 1089.41  9.6
    ## 761       dem 1180.55  9.8
    ## 762       dem 1183.26  9.4
    ## 763       dem 1141.20  9.5
    ## 764       dem 1363.61  9.1
    ## 765       dem 1218.89  9.0
    ## 766       dem 1257.60  8.5
    ## 767       dem 1327.22  9.0
    ## 768       dem 1286.12  9.2
    ## 769       dem 1292.28  9.0
    ## 770       dem 1320.64  9.1
    ## 771       dem 1325.83  9.0
    ## 772       dem 1345.20  9.0
    ## 773       dem 1246.96  8.6
    ## 774       dem 1253.30  8.8
    ## 775       dem 1131.42  9.0
    ## 776       dem 1397.91  8.2
    ## 777       dem 1406.58  8.0
    ## 778       dem 1426.19  7.9
    ## 779       dem 1365.68  8.3
    ## 780       dem 1312.41  8.3
    ## 781       dem 1379.32  8.2
    ## 782       dem 1362.16  8.2
    ## 783       dem 1408.47  8.2
    ## 784       dem 1310.33  8.2
    ## 785       dem 1416.18  7.7
    ## 786       dem 1412.16  7.8
    ## 787       dem 1440.67  7.8
    ## 788       dem 1597.57  7.6
    ## 789       dem 1632.97  7.2
    ## 790       dem 1848.36  6.7
    ## 791       dem 1514.68  7.7
    ## 792       dem 1498.11  8.0
    ## 793       dem 1685.73  7.3
    ## 794       dem 1606.28  7.5
    ## 795       dem 1569.19  7.5
    ## 796       dem 1630.74  7.5
    ## 797       dem 1805.81  7.0
    ## 798       dem 1756.54  7.2
    ## 799       dem 1681.55  7.2
    ## 800       dem 1883.95  6.2
    ## 801       dem 2003.37  6.1
    ## 802       dem 2058.90  5.6
    ## 803       dem 1859.45  6.7
    ## 804       dem 1782.59  6.6
    ## 805       dem 1930.67  6.2
    ## 806       dem 1960.23  6.1
    ## 807       dem 1872.34  6.6
    ## 808       dem 1923.57  6.3
    ## 809       dem 2067.56  5.8
    ## 810       dem 2018.05  5.7
    ## 811       dem 1972.29  5.9
    ## 812       dem 2085.51  5.4
    ## 813       dem 2104.50  5.5
    ## 814       dem 1994.99  5.7
    ## 815       dem 2063.11  5.3
    ## 816       dem 2067.89  5.5
    ## 817       dem 2107.39  5.5

These data show various election results from months and years along
with certain SNP500 close values and unemployment rates. The
pols\_month\_data data show whether or not the governor, senators,
representatives, and president were democratic or republican in that
month and year from April 1947 to March 1988. The pols\_month\_data has
817 rows and 9 columns. The snp\_data shows the month and year and the
SNP close value of how the S\&P500 stock market index did from April
1950 to May 2015. The average closing point of the S\&P over the 65
years is 474.8887404. The snp\_data has 787 rows and 3 columns.The
unemployment\_data dataset shows the unemployment rate for that month
and year from January 1948 to December 2015. The average unemployment
rate is NA. The unemployment\_data has 816 rows and 3columns. It’s
important to note that all three of these datasets have different
starting and ending months and years.The final combined dataset,
fivethirtyeight\_data has 817 rows and 11 columns.

# Problem 3

``` r
popular_baby = 
  read_csv(file = "./data_hw2/Popular_Baby_Names.csv") %>%
  janitor::clean_names() %>%
  mutate(ethnicity = recode(ethnicity, 'ASIAN AND PACIFIC ISLANDER' = "asian and pacific islander",'BLACK NON HISPANIC' = "black", 'HISPANIC' = "hispanic", 'WHITE NON HISPANIC' = "white", 'ASIAN AND PACI' = "asian and pacific islander", 'BLACK NON HISP' = "black", 'WHITE NON HISP' = "white")) %>%
  mutate(childs_first_name = str_to_lower(childs_first_name)) %>%
  group_by(childs_first_name, rank, gender) %>%
  mutate("count" = row_number()) %>%
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

    ## # A tibble: 12,181 x 5
    ## # Groups:   childs_first_name, rank, gender [10,383]
    ##    year_of_birth gender ethnicity                  childs_first_name  rank
    ##            <dbl> <chr>  <chr>                      <chr>             <dbl>
    ##  1          2016 FEMALE asian and pacific islander olivia                1
    ##  2          2016 FEMALE asian and pacific islander chloe                 2
    ##  3          2016 FEMALE asian and pacific islander sophia                3
    ##  4          2016 FEMALE asian and pacific islander emily                 4
    ##  5          2016 FEMALE asian and pacific islander emma                  4
    ##  6          2016 FEMALE asian and pacific islander mia                   5
    ##  7          2016 FEMALE asian and pacific islander charlotte             6
    ##  8          2016 FEMALE asian and pacific islander sarah                 7
    ##  9          2016 FEMALE asian and pacific islander isabella              8
    ## 10          2016 FEMALE asian and pacific islander hannah                8
    ## # … with 12,171 more rows

\*Create a table of the popularity of the name Olivia

``` r
female_name = 
  arrange(popular_baby, year_of_birth, ethnicity, childs_first_name) %>%
  filter(childs_first_name == "olivia")
female_name
```

    ## # A tibble: 24 x 5
    ## # Groups:   childs_first_name, rank, gender [11]
    ##    year_of_birth gender ethnicity                  childs_first_name  rank
    ##            <dbl> <chr>  <chr>                      <chr>             <dbl>
    ##  1          2011 FEMALE asian and pacific islander olivia                4
    ##  2          2011 FEMALE black                      olivia               10
    ##  3          2011 FEMALE hispanic                   olivia               18
    ##  4          2011 FEMALE white                      olivia                2
    ##  5          2012 FEMALE asian and pacific islander olivia                3
    ##  6          2012 FEMALE black                      olivia                8
    ##  7          2012 FEMALE hispanic                   olivia               22
    ##  8          2012 FEMALE white                      olivia                4
    ##  9          2013 FEMALE asian and pacific islander olivia                3
    ## 10          2013 FEMALE black                      olivia                6
    ## # … with 14 more rows

  - Create a table of the most popular male baby names

<!-- end list -->

``` r
male_name = 
  arrange(popular_baby, year_of_birth, ethnicity, childs_first_name) %>%
  filter(gender == "MALE", rank == 1)
male_name
```

    ## # A tibble: 24 x 5
    ## # Groups:   childs_first_name, rank, gender [8]
    ##    year_of_birth gender ethnicity                  childs_first_name  rank
    ##            <dbl> <chr>  <chr>                      <chr>             <dbl>
    ##  1          2011 MALE   asian and pacific islander ethan                 1
    ##  2          2011 MALE   black                      jayden                1
    ##  3          2011 MALE   hispanic                   jayden                1
    ##  4          2011 MALE   white                      michael               1
    ##  5          2012 MALE   asian and pacific islander ryan                  1
    ##  6          2012 MALE   black                      jayden                1
    ##  7          2012 MALE   hispanic                   jayden                1
    ##  8          2012 MALE   white                      joseph                1
    ##  9          2013 MALE   asian and pacific islander jayden                1
    ## 10          2013 MALE   black                      ethan                 1
    ## # … with 14 more rows

  - Create a ggplot of male, white, non-Hispanic children in 2016
