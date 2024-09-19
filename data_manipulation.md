Data Manipulation
================
Chhiring Lama
2024-09-19

This document will sow to “manipulate” data.

Import the two datasets that we’re going to manipulate

``` r
litters_df <- read_csv("./Data/FAS_litters.csv", na = c("NA", ".", ""))

litters_df <- janitor::clean_names(litters_df)

pups_df <- read_csv("./Data/FAS_pups.csv", na = c("NA", ".", ""))

pups_df <- janitor::clean_names(pups_df)
```

# `select`

Use `select()` to select variables

``` r
select(litters_df, group, litter_number, gd18_weight)
```

    ## # A tibble: 49 × 3
    ##    group litter_number   gd18_weight
    ##    <chr> <chr>                 <dbl>
    ##  1 Con7  #85                    34.7
    ##  2 Con7  #1/2/95/2              42  
    ##  3 Con7  #5/5/3/83/3-3          41.4
    ##  4 Con7  #5/4/2/95/2            44.1
    ##  5 Con7  #4/2/95/3-3            NA  
    ##  6 Con7  #2/2/95/3-2            NA  
    ##  7 Con7  #1/5/3/83/3-3/2        NA  
    ##  8 Con8  #3/83/3-3              NA  
    ##  9 Con8  #2/95/3                NA  
    ## 10 Con8  #3/5/2/2/95            NA  
    ## # ℹ 39 more rows

``` r
select(litters_df, group:gd18_weight)
```

    ## # A tibble: 49 × 4
    ##    group litter_number   gd0_weight gd18_weight
    ##    <chr> <chr>                <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7
    ##  2 Con7  #1/2/95/2             27          42  
    ##  3 Con7  #5/5/3/83/3-3         26          41.4
    ##  4 Con7  #5/4/2/95/2           28.5        44.1
    ##  5 Con7  #4/2/95/3-3           NA          NA  
    ##  6 Con7  #2/2/95/3-2           NA          NA  
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA  
    ##  8 Con8  #3/83/3-3             NA          NA  
    ##  9 Con8  #2/95/3               NA          NA  
    ## 10 Con8  #3/5/2/2/95           28.5        NA  
    ## # ℹ 39 more rows

``` r
select(litters_df, -c(gd0_weight, gd18_weight)) ##deselects these columns
```

    ## # A tibble: 49 × 6
    ##    group litter_number  gd_of_birth pups_born_alive pups_dead_birth pups_survive
    ##    <chr> <chr>                <dbl>           <dbl>           <dbl>        <dbl>
    ##  1 Con7  #85                     20               3               4            3
    ##  2 Con7  #1/2/95/2               19               8               0            7
    ##  3 Con7  #5/5/3/83/3-3           19               6               0            5
    ##  4 Con7  #5/4/2/95/2             19               5               1            4
    ##  5 Con7  #4/2/95/3-3             20               6               0            6
    ##  6 Con7  #2/2/95/3-2             20               6               0            4
    ##  7 Con7  #1/5/3/83/3-3…          20               9               0            9
    ##  8 Con8  #3/83/3-3               20               9               1            8
    ##  9 Con8  #2/95/3                 20               8               0            8
    ## 10 Con8  #3/5/2/2/95             20               8               0            8
    ## # ℹ 39 more rows

``` r
select(litters_df, starts_with("gd")) ##selects columns whose name start with gd_
```

    ## # A tibble: 49 × 3
    ##    gd0_weight gd18_weight gd_of_birth
    ##         <dbl>       <dbl>       <dbl>
    ##  1       19.7        34.7          20
    ##  2       27          42            19
    ##  3       26          41.4          19
    ##  4       28.5        44.1          19
    ##  5       NA          NA            20
    ##  6       NA          NA            20
    ##  7       NA          NA            20
    ##  8       NA          NA            20
    ##  9       NA          NA            20
    ## 10       28.5        NA            20
    ## # ℹ 39 more rows

``` r
select(litters_df, GROUP = group) ##selects group and renames them
```

    ## # A tibble: 49 × 1
    ##    GROUP
    ##    <chr>
    ##  1 Con7 
    ##  2 Con7 
    ##  3 Con7 
    ##  4 Con7 
    ##  5 Con7 
    ##  6 Con7 
    ##  7 Con7 
    ##  8 Con8 
    ##  9 Con8 
    ## 10 Con8 
    ## # ℹ 39 more rows

``` r
rename(litters_df, GROUP = group) ##renames but keeps all columns as they are 
```

    ## # A tibble: 49 × 8
    ##    GROUP litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
select(litters_df, litter_number, gd0_weight, everything()) ##first gives two columns and then everything else
```

    ## # A tibble: 49 × 8
    ##    litter_number   gd0_weight group gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl> <chr>       <dbl>       <dbl>           <dbl>
    ##  1 #85                   19.7 Con7         34.7          20               3
    ##  2 #1/2/95/2             27   Con7         42            19               8
    ##  3 #5/5/3/83/3-3         26   Con7         41.4          19               6
    ##  4 #5/4/2/95/2           28.5 Con7         44.1          19               5
    ##  5 #4/2/95/3-3           NA   Con7         NA            20               6
    ##  6 #2/2/95/3-2           NA   Con7         NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA   Con7         NA            20               9
    ##  8 #3/83/3-3             NA   Con8         NA            20               9
    ##  9 #2/95/3               NA   Con8         NA            20               8
    ## 10 #3/5/2/2/95           28.5 Con8         NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
relocate(litters_df, litter_number, gd0_weight) ##first gives two columns and then everything else
```

    ## # A tibble: 49 × 8
    ##    litter_number   gd0_weight group gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl> <chr>       <dbl>       <dbl>           <dbl>
    ##  1 #85                   19.7 Con7         34.7          20               3
    ##  2 #1/2/95/2             27   Con7         42            19               8
    ##  3 #5/5/3/83/3-3         26   Con7         41.4          19               6
    ##  4 #5/4/2/95/2           28.5 Con7         44.1          19               5
    ##  5 #4/2/95/3-3           NA   Con7         NA            20               6
    ##  6 #2/2/95/3-2           NA   Con7         NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA   Con7         NA            20               9
    ##  8 #3/83/3-3             NA   Con8         NA            20               9
    ##  9 #2/95/3               NA   Con8         NA            20               8
    ## 10 #3/5/2/2/95           28.5 Con8         NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## Learning Assessment:

In the pups data, select the columns containing litter number, sex, and
PD ears.

``` r
select(pups_df, litter_number, sex, pd_ears) 
```

    ## # A tibble: 313 × 3
    ##    litter_number   sex pd_ears
    ##    <chr>         <dbl>   <dbl>
    ##  1 #85               1       4
    ##  2 #85               1       4
    ##  3 #1/2/95/2         1       5
    ##  4 #1/2/95/2         1       5
    ##  5 #5/5/3/83/3-3     1       5
    ##  6 #5/5/3/83/3-3     1       5
    ##  7 #5/4/2/95/2       1      NA
    ##  8 #4/2/95/3-3       1       4
    ##  9 #4/2/95/3-3       1       4
    ## 10 #2/2/95/3-2       1       4
    ## # ℹ 303 more rows

``` r
select(pups_df, litter_number:pd_ears) ##as they are consecutive columns
```

    ## # A tibble: 313 × 3
    ##    litter_number   sex pd_ears
    ##    <chr>         <dbl>   <dbl>
    ##  1 #85               1       4
    ##  2 #85               1       4
    ##  3 #1/2/95/2         1       5
    ##  4 #1/2/95/2         1       5
    ##  5 #5/5/3/83/3-3     1       5
    ##  6 #5/5/3/83/3-3     1       5
    ##  7 #5/4/2/95/2       1      NA
    ##  8 #4/2/95/3-3       1       4
    ##  9 #4/2/95/3-3       1       4
    ## 10 #2/2/95/3-2       1       4
    ## # ℹ 303 more rows

# `filter`

``` r
filter(litters_df, gd_of_birth == 20) ##runs a Boolean function and returns rows that match this criterion
```

    ## # A tibble: 32 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  3 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  4 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  5 Con8  #3/83/3-3             NA          NA            20               9
    ##  6 Con8  #2/95/3               NA          NA            20               8
    ##  7 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  8 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ##  9 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 10 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 22 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive >= 8) 
```

    ## # A tibble: 28 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2             27          42            19               8
    ##  2 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  3 Con8  #3/83/3-3             NA          NA            20               9
    ##  4 Con8  #2/95/3               NA          NA            20               8
    ##  5 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3           28          NA            19               9
    ##  7 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ##  8 Mod7  #59                   17          33.4          19               8
    ##  9 Mod7  #103                  21.4        42.1          19               9
    ## 10 Mod7  #3/83/3-2             NA          NA            19               8
    ## # ℹ 18 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive != 18) 
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group == "Low8") 
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #53                 21.8        37.2          20               8
    ## 2 Low8  #79                 25.4        43.8          19               8
    ## 3 Low8  #100                20          39.2          20               8
    ## 4 Low8  #4/84               21.8        35.2          20               4
    ## 5 Low8  #108                25.6        47.5          20               8
    ## 6 Low8  #99                 23.5        39            20               6
    ## 7 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group %in% c("Low8", "low7")) # either
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #53                 21.8        37.2          20               8
    ## 2 Low8  #79                 25.4        43.8          19               8
    ## 3 Low8  #100                20          39.2          20               8
    ## 4 Low8  #4/84               21.8        35.2          20               4
    ## 5 Low8  #108                25.6        47.5          20               8
    ## 6 Low8  #99                 23.5        39            20               6
    ## 7 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group %in% c("Low8", "low7"), pups_born_alive == 8) ##true for group and for pups born alive
```

    ## # A tibble: 4 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #53                 21.8        37.2          20               8
    ## 2 Low8  #79                 25.4        43.8          19               8
    ## 3 Low8  #100                20          39.2          20               8
    ## 4 Low8  #108                25.6        47.5          20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
drop_na(litters_df) ## drops rows with na in any column
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Mod7  #59                 17          33.4          19               8
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
drop_na(litters_df, gd0_weight) ##drops rows with na in the column gd0_weight
```

    ## # A tibble: 34 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3         28          NA            19               9
    ##  7 Mod7  #59                 17          33.4          19               8
    ##  8 Mod7  #103                21.4        42.1          19               9
    ##  9 Mod7  #3/82/3-2           28          45.9          20               5
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## # ℹ 24 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## Learning Assessment

In the pups data:

Filter to include only pups with sex 1 Filter to include only pups with
PD walk less than 11 and sex 2

### Solution

``` r
filter(pups_df, sex == 1) 
```

    ## # A tibble: 155 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # ℹ 145 more rows

``` r
filter(pups_df, pd_walk < 11, sex == 2) 
```

    ## # A tibble: 127 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #1/2/95/2         2       4      13        7       9
    ##  2 #1/2/95/2         2       4      13        7      10
    ##  3 #1/2/95/2         2       5      13        8      10
    ##  4 #1/2/95/2         2       5      13        8      10
    ##  5 #1/2/95/2         2       5      13        6      10
    ##  6 #5/5/3/83/3-3     2       5      13        8      10
    ##  7 #5/5/3/83/3-3     2       5      14        7      10
    ##  8 #5/5/3/83/3-3     2       5      14        8      10
    ##  9 #5/4/2/95/2       2      NA      14        7      10
    ## 10 #5/4/2/95/2       2      NA      14        7      10
    ## # ℹ 117 more rows

# `mutate`

``` r
mutate(litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

``` r
mutate(litters_df, sq_pups = pups_born_alive^2)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, sq_pups <dbl>

## Learning Assessment:

In the pups data:

Create a variable that subtracts 7 from PD pivot Create a variable that
is the sum of all the PD variables

### Solution

``` r
mutate(pups_df, 
       subtracted_pd_pivot = pd_pivot - 7, 
       sum_pd_vars = sum(select(pups_df,starts_with("pd_"))))
```

    ## # A tibble: 313 × 8
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk subtracted_pd_pivot
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>               <dbl>
    ##  1 #85               1       4      13        7      11                   0
    ##  2 #85               1       4      13        7      12                   0
    ##  3 #1/2/95/2         1       5      13        7       9                   0
    ##  4 #1/2/95/2         1       5      13        8      10                   1
    ##  5 #5/5/3/83/3-3     1       5      13        8      10                   1
    ##  6 #5/5/3/83/3-3     1       5      14        6       9                  -1
    ##  7 #5/4/2/95/2       1      NA      14        5       9                  -2
    ##  8 #4/2/95/3-3       1       4      13        6       8                  -1
    ##  9 #4/2/95/3-3       1       4      13        7       9                   0
    ## 10 #2/2/95/3-2       1       4      NA        8      10                   1
    ## # ℹ 303 more rows
    ## # ℹ 1 more variable: sum_pd_vars <dbl>

# `arrange`

``` r
arrange(litters_df, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod7  #59                 17          33.4          19               8
    ##  2 Mod7  #62                 19.5        35.9          19               7
    ##  3 Con7  #85                 19.7        34.7          20               3
    ##  4 Low8  #100                20          39.2          20               8
    ##  5 Mod7  #103                21.4        42.1          19               9
    ##  6 Mod7  #106                21.7        37.8          20               5
    ##  7 Low8  #53                 21.8        37.2          20               8
    ##  8 Low8  #4/84               21.8        35.2          20               4
    ##  9 Low7  #85/2               22.2        38.5          20               8
    ## 10 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, desc(gd0_weight))
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod8  #82/4               33.4        52.7          20               8
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  4 Mod8  #2/95/2             28.5        44.5          20               9
    ##  5 Con8  #5/4/3/83/3         28          NA            19               9
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Mod8  #7/110/3-2          27.5        46            19               8
    ##  8 Con7  #1/2/95/2           27          42            19               8
    ##  9 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 10 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, pups_born_alive, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Low7  #111                25.5        44.6          20               3
    ##  3 Low8  #4/84               21.8        35.2          20               4
    ##  4 Mod7  #106                21.7        37.8          20               5
    ##  5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  8 Con8  #2/2/95/2           NA          NA            19               5
    ##  9 Low8  #99                 23.5        39            20               6
    ## 10 Low7  #112                23.9        40.5          19               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

# PIPING

If we use individual functions separately that is a lot of repetition of
assigning each new function to the variable.

Option 1 (not to use):

``` r
litters_df_raw = 
    read_csv("./data/FAS_litters.csv", na = c("NA", ".", ""))

litters_df_clean_names = janitor::clean_names(litters_df_raw)
litters_df_selected_cols = select(litters_df_clean_names, -pups_survive)
litters_df_with_vars = 
  mutate(
    litters_df_selected_cols, 
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))
litters_df_with_vars_without_missing = 
  drop_na(litters_df_with_vars, wt_gain)
litters_df_with_vars_without_missing
```

Option 2(not to use): can nest each code, however it is very difficult
to read and troubleshoot when needed

``` r
litters_df_clean = 
  drop_na(
    mutate(
      select(
        janitor::clean_names(
          read_csv("./data/FAS_litters.csv", na = c("NA", ".", ""))
          ), 
      -pups_survive
      ),
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)
    ),
  wt_gain
  )
```

This is when piping comes in

``` r
litters_df_clean <- read_csv("./Data/FAS_litters.csv", na = c("NA", ".", "")) |>  
  janitor::clean_names() |>  
  select(-pups_born_alive) |> 
  filter(gd_of_birth == 20) |> 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) |> 
  drop_na()
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

If we want to run a linear regression on the weight gain:

``` r
read_csv("./Data/FAS_litters.csv", na = c("NA", ".", "")) |>  
  janitor::clean_names() |>  
  mutate(
    wt_gain = gd18_weight - gd0_weight) |> 
  lm(wt_gain ~ pups_born_alive, data = _) |>
  broom::tidy() ##shows in a cleaner dataframe format
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## # A tibble: 2 × 5
    ##   term            estimate std.error statistic  p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)       13.1       1.27      10.3  3.39e-11
    ## 2 pups_born_alive    0.605     0.173      3.49 1.55e- 3

## Learning Assessment:

Write a chain of commands that:

loads the pups data cleans the variable names filters the data to
include only pups with sex 1 removes the PD ears variable creates a
variable that indicates whether PD pivot is 7 or more days

### Solution

``` r
pups_df_clean <- read_csv("./Data/FAS_pups.csv", na = c("NA", ".", "")) |>  
  janitor::clean_names() |>  
  filter(sex == 1) |> 
  select(-pd_ears) |> 
  mutate(
    pd_pivot_greater_7 = (pd_pivot >= 7))
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
