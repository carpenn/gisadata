
<!-- README.md is generated from README.Rmd. Please edit that file -->

# gisadata

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/kasaai/gisadata.svg?branch=master)](https://travis-ci.org/kasaai/gisadata)
[![Codecov test
coverage](https://codecov.io/gh/kasaai/gisadata/branch/master/graph/badge.svg)](https://codecov.io/gh/kasaai/gisadata?branch=master)
<!-- badges: end -->

Workflow functions for tidying up claims data from the General Insurance
Statistical Agency (GISA) of Canada.

## Installation

``` r
remotes::install_github("kasaai/gisadata")
```

## Example

Attach necessary packages:

``` r
library(gisadata)
library(tidyverse)
library(fs)
```

Suppose the data archives are in the `gisa-data` directory:

``` r
dir_ls("gisa-data")
#> gisa-data/Auto Cat Report.zip       gisa-data/Auto Intro.zip            
#> gisa-data/Auto Loss Development.zip gisa-data/CLSP.zip
```

We can extract the CSV files by calling `gisa_unzip()`:

``` r
# By default, files are extracted to a temp directory
extract_dir <- gisa_unzip("gisa-data")
dir_ls(extract_dir)
#> /var/folders/lm/wwpd13g55cz3wf0gn594b59w0000gn/T/RtmpgVDXCb/Auto Cat Report
#> /var/folders/lm/wwpd13g55cz3wf0gn594b59w0000gn/T/RtmpgVDXCb/Auto Intro
#> /var/folders/lm/wwpd13g55cz3wf0gn594b59w0000gn/T/RtmpgVDXCb/Auto Loss Development
#> /var/folders/lm/wwpd13g55cz3wf0gn594b59w0000gn/T/RtmpgVDXCb/CLSP
```

Read and process tables:

``` r
data_auto <- extract_dir %>% 
  path("Auto Loss Development") %>% 
  gisa_process_auto_dev()

data_auto$`Loss development exhibit - Private Passenger - All Perils - AB` %>% 
  glimpse()
#> Observations: 16,869
#> Variables: 32
#> $ written_vehicles           <dbl> 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.…
#> $ earned_vehicles            <dbl> 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.…
#> $ written_premium            <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, -7386, 0,…
#> $ earned_premium             <dbl> 0.0000, 0.0000, 0.0000, 0.0000, 0.0000, 0.…
#> $ claim_count                <dbl> 1, 15, 3, 21, 2, 19, 8, 0, 1, 0, 0, 0, 66,…
#> $ claim_count_original       <dbl> 1, 15, 3, 21, 2, 19, 8, 0, 1, 0, 0, 0, 66,…
#> $ claim_count_ompp           <dbl> 1, 15, 3, 21, 2, 19, 8, 0, 1, 0, 0, 0, 66,…
#> $ loss_amount                <dbl> 3497, 11725, 6380, 89815, 16872, 77393, 26…
#> $ expense_amount             <dbl> 1243, 0, 374, 4268, 135, 4177, 260, 0, 0, …
#> $ loss_and_expense_amount    <dbl> 4740, 11725, 6754, 94083, 17007, 81570, 26…
#> $ section_number             <chr> "5", "5", "5", "5", "5", "5", "5", "5", "5…
#> $ valuation_year             <chr> "201812", "201812", "201812", "201812", "2…
#> $ company_identification     <chr> "000", "000", "000", "000", "000", "000", …
#> $ major_vehicle_class        <chr> "PPV", "PPV", "PPV", "PPV", "PPV", "PPV", …
#> $ minor_vehicle_class        <chr> "PPV-IR excluding Farmers", "PPV-IR exclud…
#> $ excluded_driver_code       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
#> $ trailer_indicator          <chr> "N", "N", "N", "N", "Y", "N", "N", "Y", "N…
#> $ grid_indicator             <chr> NA, NA, "Y", "N", NA, NA, "N", NA, "N", NA…
#> $ first_chance_indicator     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
#> $ region                     <chr> "1 - Alberta", "1 - Alberta", "1 - Alberta…
#> $ province                   <chr> "AB", "AB", "AB", "AB", "AB", "AB", "AB", …
#> $ deductible_amount          <chr> "All Perils ($250 Deductible) / Tous Risqu…
#> $ limit_amount               <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
#> $ kind_of_loss_code          <chr> "21", "26", "20", "29", "22", "20", "20", …
#> $ entry_half_year            <chr> "200301", "201301", "200602", "201302", "2…
#> $ accident_half_year         <chr> "200202", "201301", "200602", "201302", "2…
#> $ factor_flag                <chr> "No", "No", "No", "No", "No", "No", "No", …
#> $ fleet_flag                 <chr> "Fleet rated", "Fleet rated", "No", "No", …
#> $ major_coverage_type        <chr> "All Perils", "All Perils", "All Perils", …
#> $ minor_coverage_type        <chr> "All Perils", "All Perils", "All Perils", …
#> $ loss_transfer_flag         <chr> "No", "No", "No", "No", "No", "No", "No", …
#> $ paid_outstanding_indicator <chr> "Paid", "Paid", "Outstanding", "Paid", "Pa…
```

## Contributing

  - Do not commit any (real) data files.
  - Follow [Tidyverse style guide](https://style.tidyverse.org/).
