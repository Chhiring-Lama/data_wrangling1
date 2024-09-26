Tidy Data
================

I’m an R Markdown document!

This document will show how to tidy data

## pivot longer

``` r
pulse_df  <- read_sas("Data/public_pulse_data.sas7bdat") |> 
  janitor::clean_names()
```

This needs to go from wide to long format.

``` r
pulse_tidy_df  <- read_sas("Data/public_pulse_data.sas7bdat") |> 
  janitor::clean_names() |> 
  pivot_longer(
    cols = bdi_score_bl:bdi_score_12m, 
    names_to = "visit", 
    values_to = "bdi_score",
    names_prefix = "bdi_score_"
  ) |> 
  mutate(visit = replace(visit, visit == "bl", "00m")) |> 
  relocate(id, visit)
```

One more example.

``` r
litters_df <- read_csv("Data/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() 
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

This needs to go in the long format where we combine weight in gds.

``` r
litters_tidy_df <- read_csv("Data/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  pivot_longer(
    cols = gd0_weight:gd18_weight, 
    names_to = "gestational_day", 
    values_to = "weight"
  ) |> 
  mutate(
    gestational_day = case_match(
      gestational_day, 
      "gd0_weight" ~ 0, 
      "gd18_weight" ~ 18))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## pivot wider

Let’s make up an analysis result table

``` r
analysis_df <-
  tibble(
    group = c("treatment", "treatment", "control", "control"), 
    time = c("pre", "post", "pre", "post"), 
    mean = c(4, 10, 4.5, 5)
  )
```

The data is tidy but the visualization of 2-way association between
group and time is better in wide format

``` r
tibble(
    group = c("treatment", "treatment", "control", "control"), 
    time = c("pre", "post", "pre", "post"), 
    mean = c(4, 10, 4.5, 5)
  ) |> 
  pivot_wider(
    names_from = time, 
    values_from = mean
  ) |> 
  knitr::kable()
```

| group     | pre | post |
|:----------|----:|-----:|
| treatment | 4.0 |   10 |
| control   | 4.5 |    5 |
