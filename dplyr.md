dplyr
================
Saurabh Mishra
2/7/2018

Introduction to dplyr
---------------------

### R Markdown

Subsetting data should be the LAST resort. Use proper data aggregation techniques or facetting in ggplot2. Or, more realistic, only subset the data as a temporary measure while you develop your elegant code for computing on or visualizing these data subsets.

Copies and excerpts of your data clutter your workspace, invite mistakes, and sow general confusion. Avoid whenever possible. Reality can also lie somewhere in between. A subset is not self-documenting and is fragile (in case of data manipulation)

### Filter

filter() takes logical expressions and returns the rows for which all are TRUE.

filter() subsets data row-wise:

``` r
library(gapminder)
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.1     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.7.2     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ───────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
gapminder
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779
    ##  2 Afghanistan Asia       1957    30.3  9240934       821
    ##  3 Afghanistan Asia       1962    32.0 10267083       853
    ##  4 Afghanistan Asia       1967    34.0 11537966       836
    ##  5 Afghanistan Asia       1972    36.1 13079460       740
    ##  6 Afghanistan Asia       1977    38.4 14880372       786
    ##  7 Afghanistan Asia       1982    39.9 12881816       978
    ##  8 Afghanistan Asia       1987    40.8 13867957       852
    ##  9 Afghanistan Asia       1992    41.7 16317921       649
    ## 10 Afghanistan Asia       1997    41.8 22227415       635
    ## # ... with 1,694 more rows

``` r
filter(gapminder, lifeExp < 39)
```

    ## # A tibble: 95 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779
    ##  2 Afghanistan Asia       1957    30.3  9240934       821
    ##  3 Afghanistan Asia       1962    32.0 10267083       853
    ##  4 Afghanistan Asia       1967    34.0 11537966       836
    ##  5 Afghanistan Asia       1972    36.1 13079460       740
    ##  6 Afghanistan Asia       1977    38.4 14880372       786
    ##  7 Angola      Africa     1952    30.0  4232095      3521
    ##  8 Angola      Africa     1957    32.0  4561361      3828
    ##  9 Angola      Africa     1962    34.0  4826015      4269
    ## 10 Angola      Africa     1967    36.0  5247469      5523
    ## # ... with 85 more rows

``` r
filter(gapminder, country == "Canada")
```

    ## # A tibble: 12 x 6
    ##    country continent  year lifeExp      pop gdpPercap
    ##    <fctr>  <fctr>    <int>   <dbl>    <int>     <dbl>
    ##  1 Canada  Americas   1952    68.8 14785584     11367
    ##  2 Canada  Americas   1957    70.0 17010154     12490
    ##  3 Canada  Americas   1962    71.3 18985849     13462
    ##  4 Canada  Americas   1967    72.1 20819767     16077
    ##  5 Canada  Americas   1972    72.9 22284500     18971
    ##  6 Canada  Americas   1977    74.2 23796400     22091
    ##  7 Canada  Americas   1982    75.8 25201900     22899
    ##  8 Canada  Americas   1987    76.9 26549700     26627
    ##  9 Canada  Americas   1992    78.0 28523502     26343
    ## 10 Canada  Americas   1997    78.6 30305843     28955
    ## 11 Canada  Americas   2002    79.8 31902268     33329
    ## 12 Canada  Americas   2007    80.7 33390141     36319

``` r
filter(gapminder, country == "Rwanda", year > 1979)
```

    ## # A tibble: 6 x 6
    ##   country continent  year lifeExp     pop gdpPercap
    ##   <fctr>  <fctr>    <int>   <dbl>   <int>     <dbl>
    ## 1 Rwanda  Africa     1982    46.2 5507565       882
    ## 2 Rwanda  Africa     1987    44.0 6349365       848
    ## 3 Rwanda  Africa     1992    23.6 7290203       737
    ## 4 Rwanda  Africa     1997    36.1 7212583       590
    ## 5 Rwanda  Africa     2002    43.4 7852401       786
    ## 6 Rwanda  Africa     2007    46.2 8860588       863

``` r
filter(gapminder, country %in% c("Rwanda", "Afghanistan"))
```

    ## # A tibble: 24 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779
    ##  2 Afghanistan Asia       1957    30.3  9240934       821
    ##  3 Afghanistan Asia       1962    32.0 10267083       853
    ##  4 Afghanistan Asia       1967    34.0 11537966       836
    ##  5 Afghanistan Asia       1972    36.1 13079460       740
    ##  6 Afghanistan Asia       1977    38.4 14880372       786
    ##  7 Afghanistan Asia       1982    39.9 12881816       978
    ##  8 Afghanistan Asia       1987    40.8 13867957       852
    ##  9 Afghanistan Asia       1992    41.7 16317921       649
    ## 10 Afghanistan Asia       1997    41.8 22227415       635
    ## # ... with 14 more rows

### Pipe

The pipe operator takes the thing on the left-hand-side and pipes it into the function call on the right-hand-side – literally, drops it in as the first argument.

    ## # A tibble: 10 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779
    ##  2 Afghanistan Asia       1957    30.3  9240934       821
    ##  3 Afghanistan Asia       1962    32.0 10267083       853
    ##  4 Afghanistan Asia       1967    34.0 11537966       836
    ##  5 Afghanistan Asia       1972    36.1 13079460       740
    ##  6 Afghanistan Asia       1977    38.4 14880372       786
    ##  7 Afghanistan Asia       1982    39.9 12881816       978
    ##  8 Afghanistan Asia       1987    40.8 13867957       852
    ##  9 Afghanistan Asia       1992    41.7 16317921       649
    ## 10 Afghanistan Asia       1997    41.8 22227415       635

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

### Select

select() takes logical expressions and returns the columns for which all are TRUE.

select() is used to subset the data on variables or columns:

``` r
select(gapminder, year, lifeExp)
```

    ## # A tibble: 1,704 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    28.8
    ##  2  1957    30.3
    ##  3  1962    32.0
    ##  4  1967    34.0
    ##  5  1972    36.1
    ##  6  1977    38.4
    ##  7  1982    39.9
    ##  8  1987    40.8
    ##  9  1992    41.7
    ## 10  1997    41.8
    ## # ... with 1,694 more rows

``` r
gapminder %>% select(year, lifeExp) %>% head(4)
```

    ## # A tibble: 4 x 2
    ##    year lifeExp
    ##   <int>   <dbl>
    ## 1  1952    28.8
    ## 2  1957    30.3
    ## 3  1962    32.0
    ## 4  1967    34.0

``` r
gapminder %>%
  filter(country == "Cambodia") %>%
  select(year, lifeExp)
```

    ## # A tibble: 12 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    39.4
    ##  2  1957    41.4
    ##  3  1962    43.4
    ##  4  1967    45.4
    ##  5  1972    40.3
    ##  6  1977    31.2
    ##  7  1982    51.0
    ##  8  1987    53.9
    ##  9  1992    55.8
    ## 10  1997    56.5
    ## 11  2002    56.8
    ## 12  2007    59.7

``` r
# R native:-
# gapminder[gapminder$country == "Cambodia", c("year", "lifeExp")]
```

select() can: 1. rename the variables you request to keep. 2. be used with everything() to hoist a variable up to the front of the tibble. everything() is a helper for variable selection

``` r
library(tidyverse)

filter(gapminder, country == "Burundi", year > 1996) %>% 
select(yr = year, lifeExp, gdpPercap) %>% 
select(gdpPercap, everything())
```

    ## # A tibble: 3 x 3
    ##   gdpPercap    yr lifeExp
    ##       <dbl> <int>   <dbl>
    ## 1       463  1997    45.3
    ## 2       446  2002    47.4
    ## 3       430  2007    49.6

Intermediate dplyr
------------------

### Mutate

mutate() is a function that defines and inserts new variables (columns) into a tibble.

Imagine we wanted to recover each country’s GDP. After all, the Gapminder data has a variable for population and GDP per capita.

``` r
# create an explicit copy of gapminder for experimenting
(my_gap <- gapminder)
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779
    ##  2 Afghanistan Asia       1957    30.3  9240934       821
    ##  3 Afghanistan Asia       1962    32.0 10267083       853
    ##  4 Afghanistan Asia       1967    34.0 11537966       836
    ##  5 Afghanistan Asia       1972    36.1 13079460       740
    ##  6 Afghanistan Asia       1977    38.4 14880372       786
    ##  7 Afghanistan Asia       1982    39.9 12881816       978
    ##  8 Afghanistan Asia       1987    40.8 13867957       852
    ##  9 Afghanistan Asia       1992    41.7 16317921       649
    ## 10 Afghanistan Asia       1997    41.8 22227415       635
    ## # ... with 1,694 more rows

``` r
my_precious <- my_gap %>% filter(country == "Canada")

my_gap %>%
    mutate(gdp = pop * gdpPercap)
```

    ## # A tibble: 1,704 x 7
    ##    country     continent  year lifeExp      pop gdpPercap         gdp
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>       <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779  6567086330
    ##  2 Afghanistan Asia       1957    30.3  9240934       821  7585448670
    ##  3 Afghanistan Asia       1962    32.0 10267083       853  8758855797
    ##  4 Afghanistan Asia       1967    34.0 11537966       836  9648014150
    ##  5 Afghanistan Asia       1972    36.1 13079460       740  9678553274
    ##  6 Afghanistan Asia       1977    38.4 14880372       786 11697659231
    ##  7 Afghanistan Asia       1982    39.9 12881816       978 12598563401
    ##  8 Afghanistan Asia       1987    40.8 13867957       852 11820990309
    ##  9 Afghanistan Asia       1992    41.7 16317921       649 10595901589
    ## 10 Afghanistan Asia       1997    41.8 22227415       635 14121995875
    ## # ... with 1,694 more rows

``` r
# GDP numbers are almost uselessly large and abstract. Randall Munroe of xkcd:

# "One thing that bothers me is large numbers presented without context… ‘If I added a zero to this number, would the sentence containing it mean something different to me?’ If the answer is ‘no,’ maybe the number has no business being in the sentence in the first place."


# It would be better to report GDP per capita, relative to some benchmark country. Taking Canada:

ctib <- my_gap %>%
  filter(country == "Canada")

# joining year would be a better option
my_gap <- my_gap %>%
    mutate(tmp = rep(ctib$gdpPercap, nlevels(country)),
           gdpPercapRel = gdpPercap / tmp,
           tmp = NULL) # get rid of a variable by setting it to NULL

# Canadian values for gdpPercapRel better all be 1!
my_gap %>% 
  filter(country == "Canada") %>% 
  select(country, year, gdpPercapRel)
```

    ## # A tibble: 12 x 3
    ##    country  year gdpPercapRel
    ##    <fctr>  <int>        <dbl>
    ##  1 Canada   1952         1.00
    ##  2 Canada   1957         1.00
    ##  3 Canada   1962         1.00
    ##  4 Canada   1967         1.00
    ##  5 Canada   1972         1.00
    ##  6 Canada   1977         1.00
    ##  7 Canada   1982         1.00
    ##  8 Canada   1987         1.00
    ##  9 Canada   1992         1.00
    ## 10 Canada   1997         1.00
    ## 11 Canada   2002         1.00
    ## 12 Canada   2007         1.00

``` r
summary(my_gap$gdpPercapRel)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## 0.007236 0.061648 0.171521 0.326659 0.446564 9.534690

``` r
# The relative GDP per capita numbers are, in general, well below 1. We see that most of the countries covered by this dataset have substantially lower GDP per capita, relative to Canada, across the entire time period.
```

### Arrange

arrange() reorders the rows in a data frame in a principled way.

``` r
# data ordered by year then country
my_gap %>%
  arrange(year, country)
```

    ## # A tibble: 1,704 x 7
    ##    country     continent  year lifeExp      pop gdpPercap gdpPercapRel
    ##    <fctr>      <fctr>    <int>   <dbl>    <int>     <dbl>        <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333       779       0.0686
    ##  2 Albania     Europe     1952    55.2  1282697      1601       0.141 
    ##  3 Algeria     Africa     1952    43.1  9279525      2449       0.215 
    ##  4 Angola      Africa     1952    30.0  4232095      3521       0.310 
    ##  5 Argentina   Americas   1952    62.5 17876956      5911       0.520 
    ##  6 Australia   Oceania    1952    69.1  8691212     10040       0.883 
    ##  7 Austria     Europe     1952    66.8  6927772      6137       0.540 
    ##  8 Bahrain     Asia       1952    50.9   120447      9867       0.868 
    ##  9 Bangladesh  Asia       1952    37.5 46886859       684       0.0602
    ## 10 Belgium     Europe     1952    68.0  8730405      8343       0.734 
    ## # ... with 1,694 more rows

``` r
# data from 2007, sorted on life expectancy in ascending order
my_gap %>%
  filter(year == 2007) %>%
  arrange(lifeExp)
```

    ## # A tibble: 142 x 7
    ##    country                  continent  year lifeExp      pop gdpPe… gdpPe…
    ##    <fctr>                   <fctr>    <int>   <dbl>    <int>  <dbl>  <dbl>
    ##  1 Swaziland                Africa     2007    39.6  1133066   4513 0.124 
    ##  2 Mozambique               Africa     2007    42.1 19951656    824 0.0227
    ##  3 Zambia                   Africa     2007    42.4 11746035   1271 0.0350
    ##  4 Sierra Leone             Africa     2007    42.6  6144562    863 0.0237
    ##  5 Lesotho                  Africa     2007    42.6  2012649   1569 0.0432
    ##  6 Angola                   Africa     2007    42.7 12420476   4797 0.132 
    ##  7 Zimbabwe                 Africa     2007    43.5 12311143    470 0.0129
    ##  8 Afghanistan              Asia       2007    43.8 31889923    975 0.0268
    ##  9 Central African Republic Africa     2007    44.7  4369038    706 0.0194
    ## 10 Liberia                  Africa     2007    45.7  3193942    415 0.0114
    ## # ... with 132 more rows

``` r
# descending order
my_gap %>%
  filter(year == 2007) %>%
  arrange(desc(lifeExp))
```

    ## # A tibble: 142 x 7
    ##    country          continent  year lifeExp       pop gdpPercap gdpPercap…
    ##    <fctr>           <fctr>    <int>   <dbl>     <int>     <dbl>      <dbl>
    ##  1 Japan            Asia       2007    82.6 127467972     31656      0.872
    ##  2 Hong Kong, China Asia       2007    82.2   6980412     39725      1.09 
    ##  3 Iceland          Europe     2007    81.8    301931     36181      0.996
    ##  4 Switzerland      Europe     2007    81.7   7554661     37506      1.03 
    ##  5 Australia        Oceania    2007    81.2  20434176     34435      0.948
    ##  6 Spain            Europe     2007    80.9  40448191     28821      0.794
    ##  7 Sweden           Europe     2007    80.9   9031088     33860      0.932
    ##  8 Israel           Asia       2007    80.7   6426679     25523      0.703
    ##  9 France           Europe     2007    80.7  61083916     30470      0.839
    ## 10 Canada           Americas   2007    80.7  33390141     36319      1.00 
    ## # ... with 132 more rows

### Rename

Used to rename vairables (columns)

``` r
my_gap %>%
    rename(life_exp = lifeExp,
           gdp_percap = gdpPercap,
           gdp_percap_rel = gdpPercapRel)
```

    ## # A tibble: 1,704 x 7
    ##    country     continent  year life_exp      pop gdp_percap gdp_percap_rel
    ##    <fctr>      <fctr>    <int>    <dbl>    <int>      <dbl>          <dbl>
    ##  1 Afghanistan Asia       1952     28.8  8425333        779         0.0686
    ##  2 Afghanistan Asia       1957     30.3  9240934        821         0.0657
    ##  3 Afghanistan Asia       1962     32.0 10267083        853         0.0634
    ##  4 Afghanistan Asia       1967     34.0 11537966        836         0.0520
    ##  5 Afghanistan Asia       1972     36.1 13079460        740         0.0390
    ##  6 Afghanistan Asia       1977     38.4 14880372        786         0.0356
    ##  7 Afghanistan Asia       1982     39.9 12881816        978         0.0427
    ##  8 Afghanistan Asia       1987     40.8 13867957        852         0.0320
    ##  9 Afghanistan Asia       1992     41.7 16317921        649         0.0246
    ## 10 Afghanistan Asia       1997     41.8 22227415        635         0.0219
    ## # ... with 1,694 more rows

**Select can also be used to rename variables**

### Group By and Summarize

group\_by() adds extra structure to the dataset – grouping information – which lays the groundwork for computations within the groups.

summarize() takes a dataset with n observations, computes requested summaries, and returns a dataset with 1 observation.

``` r
my_gap %>%
  group_by(continent) %>%
  summarize(continent_obsv = n())
```

    ## # A tibble: 5 x 2
    ##   continent continent_obsv
    ##   <fctr>             <int>
    ## 1 Africa               624
    ## 2 Americas             300
    ## 3 Asia                 396
    ## 4 Europe               360
    ## 5 Oceania               24

In native R, the object of class table that is returned makes downstream computation a bit fiddlier than you’d like. In this case, it’s too bad the continent levels come back only as names and not as a proper factor, with the original set of levels. This is an example of how the tidyverse smooths transitions where you want the output of step i to become the input of step i + 1.

``` r
# tally() is a convenience function that counts rows and honors groups.
my_gap %>%
  group_by(continent) %>%
  tally()
```

    ## # A tibble: 5 x 2
    ##   continent     n
    ##   <fctr>    <int>
    ## 1 Africa      624
    ## 2 Americas    300
    ## 3 Asia        396
    ## 4 Europe      360
    ## 5 Oceania      24

``` r
# count() is an even more convenient function that does both grouping and counting.
my_gap %>% 
  count(continent)
```

    ## # A tibble: 5 x 2
    ##   continent     n
    ##   <fctr>    <int>
    ## 1 Africa      624
    ## 2 Americas    300
    ## 3 Asia        396
    ## 4 Europe      360
    ## 5 Oceania      24

``` r
# number of unique countries in each continent
my_gap %>%
  group_by(continent) %>%
  summarize(n = n(),
            n_countries = n_distinct(country))
```

    ## # A tibble: 5 x 3
    ##   continent     n n_countries
    ##   <fctr>    <int>       <int>
    ## 1 Africa      624          52
    ## 2 Americas    300          25
    ## 3 Asia        396          33
    ## 4 Europe      360          30
    ## 5 Oceania      24           2

``` r
# average life expectancy by continent
my_gap %>%
  group_by(continent) %>%
  summarize(avg_lifeExp = mean(lifeExp))
```

    ## # A tibble: 5 x 2
    ##   continent avg_lifeExp
    ##   <fctr>          <dbl>
    ## 1 Africa           48.9
    ## 2 Americas         64.7
    ## 3 Asia             60.1
    ## 4 Europe           71.9
    ## 5 Oceania          74.3

``` r
#summarize_at() applies the same summary function(s) to multiple variables. Let’s compute average and median life expectancy and GDP per capita by continent by year … but only for 1952 and 2007.
my_gap %>%
  filter(year %in% c(1952, 2007)) %>%
  group_by(continent, year) %>%
  summarize_at(vars(lifeExp, gdpPercap), funs(mean, median))
```

    ## # A tibble: 10 x 6
    ## # Groups: continent [?]
    ##    continent  year lifeExp_mean gdpPercap_mean lifeExp_median gdpPercap_m…
    ##    <fctr>    <int>        <dbl>          <dbl>          <dbl>        <dbl>
    ##  1 Africa     1952         39.1           1253           38.8          987
    ##  2 Africa     2007         54.8           3089           52.9         1452
    ##  3 Americas   1952         53.3           4079           54.7         3048
    ##  4 Americas   2007         73.6          11003           72.9         8948
    ##  5 Asia       1952         46.3           5195           44.9         1207
    ##  6 Asia       2007         70.7          12473           72.4         4471
    ##  7 Europe     1952         64.4           5661           65.9         5142
    ##  8 Europe     2007         77.6          25054           78.6        28054
    ##  9 Oceania    1952         69.3          10298           69.3        10298
    ## 10 Oceania    2007         80.7          29810           80.7        29810

``` r
# What are the minimum and maximum life expectancies seen by year in Asia
my_gap %>%
  filter(continent == "Asia") %>%
  group_by(year) %>%
  summarize(min_lifeExp = min(lifeExp), max_lifeExp = max(lifeExp))
```

    ## # A tibble: 12 x 3
    ##     year min_lifeExp max_lifeExp
    ##    <int>       <dbl>       <dbl>
    ##  1  1952        28.8        65.4
    ##  2  1957        30.3        67.8
    ##  3  1962        32.0        69.4
    ##  4  1967        34.0        71.4
    ##  5  1972        36.1        73.4
    ##  6  1977        31.2        75.4
    ##  7  1982        39.9        77.1
    ##  8  1987        40.8        78.7
    ##  9  1992        41.7        79.4
    ## 10  1997        41.8        80.7
    ## 11  2002        42.1        82.0
    ## 12  2007        43.8        82.6
