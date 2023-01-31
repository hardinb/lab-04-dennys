Lab 04 - La Quinta is Spanish for next to Denny’s, Pt. 1
================
Ben Hardin
1/31/2023

### Load packages and data

``` r
library(tidyverse) 
library(dsbox) 
```

``` r
states <- read_csv("data/states.csv")
dennys <- dennys
laquinta <- laquinta
```

### Exercise 1

The Denny’s dataset has 6 columns and 1643 rows. In this datafile, rows
are individual Denny’s. There are 6 variables (columns), representing
each Denny’s address, city, state, zip code, and longitude and latitude
coordinates.

``` r
#getting rows and columns of dennys
nrow(dennys)
```

    ## [1] 1643

``` r
ncol(dennys)
```

    ## [1] 6

``` r
summary(dennys)
```

    ##    address              city              state               zip           
    ##  Length:1643        Length:1643        Length:1643        Length:1643       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    longitude          latitude    
    ##  Min.   :-158.09   Min.   :19.65  
    ##  1st Qu.:-117.33   1st Qu.:33.00  
    ##  Median : -96.84   Median :36.05  
    ##  Mean   : -99.60   Mean   :36.36  
    ##  3rd Qu.: -82.65   3rd Qu.:40.09  
    ##  Max.   : -68.42   Max.   :64.84

### Exercise 2

The Laquinta dataset has 6 columns and 909 rows. Again, the rows
represent individual LaQuinta Inn locations. The 6 variables are the
same ones we saw in the Denny’s dataset, telling us the location of each
Laquinta location.

``` r
nrow(laquinta)
```

    ## [1] 909

``` r
ncol(laquinta)
```

    ## [1] 6

``` r
summary(laquinta)
```

    ##    address              city              state               zip           
    ##  Length:909         Length:909         Length:909         Length:909        
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    longitude          latitude     
    ##  Min.   :-149.91   Min.   : 6.225  
    ##  1st Qu.:-100.48   1st Qu.:30.475  
    ##  Median : -95.27   Median :34.082  
    ##  Mean   : -94.79   Mean   :35.038  
    ##  3rd Qu.: -84.35   3rd Qu.:39.199  
    ##  Max.   : -70.28   Max.   :64.824

### Exercise 3

From looking at the website, I can see that Denny’s has no locations
outside the US. LaQuinta on the other hand has several locations in
other countries. These include countries like Canada, Chile, China,
Columbia, New Zealand, Turkey, and the UAE.

### Exercise 4

We could also figure out whether these establishments have locations in
other countries using just the datasets we have. Given the information
we have, it seems like some options would be getting either the states
or the zip codes for each location, and filtering for whether the state
or zip code for each location is a place in the US or in a different
country.

### Exercise 5

There are 0 Denny’s locations left after filtering for states that are
not in the US. This confirms what we saw earlier on the website, that
there are no Denny’s outside the US.

``` r
dennys %>%
  filter(!(state %in% states$abbreviation))
```

    ## # A tibble: 0 × 6
    ## # … with 6 variables: address <chr>, city <chr>, state <chr>, zip <chr>,
    ## #   longitude <dbl>, latitude <dbl>

### Exercise 6

The code chunk below makes a new variable for whether each Denny’s is
located in the US or another country. We now have a new dataset with 7
variables, one of which is our new country variable! Confirming our
earlier findings, all 1643 observations in the dataset are Denny’s
located in the US.

``` r
dennys <- dennys %>%
  mutate(country_us = if_else(state %in% states$abbreviation, "United States", "other"))

#did it work?
summary(dennys)
```

    ##    address              city              state               zip           
    ##  Length:1643        Length:1643        Length:1643        Length:1643       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    longitude          latitude      country_us       
    ##  Min.   :-158.09   Min.   :19.65   Length:1643       
    ##  1st Qu.:-117.33   1st Qu.:33.00   Class :character  
    ##  Median : -96.84   Median :36.05   Mode  :character  
    ##  Mean   : -99.60   Mean   :36.36                     
    ##  3rd Qu.: -82.65   3rd Qu.:40.09                     
    ##  Max.   : -68.42   Max.   :64.84

``` r
dennys %>%
  count(country_us)
```

    ## # A tibble: 1 × 2
    ##   country_us        n
    ##   <chr>         <int>
    ## 1 United States  1643
