---
layout: page
title: A through Z Example
---

This series of example code snippets walks you through a
pipeline for `eiCompare` from start to finish. Feel free to
use this as a guide to start your own work flow.

## Packages

``` r
suppressMessages({
  library(eiCompare)
  library(sf)
  library(tidyverse)
})
```

## Geocoding

``` r
# Import the voter file (as ram_vf)
load("~/shared/east_ramapo/data/ram_geocode.RData")
```

``` r
# Drop duplicated Records #
ram_vf <- ram_vf[!duplicated(ram_vf$Voter_ID),]
dim(ram)
```

```{r}
ram_vf <- run_geocoder(voter_file= ram_vf,
                       geocoder = "census",
                       parallel = TRUE,
                       voter_id = "VOTER_ID",
                       street = "Address",
                       city = "precinct_name",
                       state = "state",
                       zipcode = NULL,
                       country = NULL,
                       census_return = NULL,
                       census_benchmark = "Public_AR_Current",
                       census_vintage = 4,
                       census_output = "single",
                       census_class = "sf",
                       opencage_key = NULL)
```

## BISG

``` r
# Import census data
base_path <- "~/shared/east_ramapo/data/"
census_shape_path <- file.path(base_path, "tl_2014_36_tabblock10.shp")
# Census shape file
census_shape <- read_sf(census_shape_path)
# Census racial demographic data
data(rockland_census)
```

``` r
# Merge voter file with Census shape file
ramapo_w_census <- eiCompare::merge_voter_file_to_shape(
  voter_file = ram_vf,
  shape_file = census_shape,
  coords = c("lon", "lat"),
  voter_id = "Voter_ID"
)
```

``` r
# Filter out voters that didn't match on the block
ramapo_w_census <- dplyr::filter(
  ramapo_w_census,
  !is.na(.data[["BLOCKCE10"]])
)
```

``` r
# Apply BISG to the voter file to get race predictions
ramapo_bisg <- eiCompare::wru_predict_race_wrapper(
  voter_file = as.data.frame(ramapo_w_census),
  census_data = rockland_census,
  voter_id = "Voter_ID",
  surname = "Last_Name",
  state = "NY",
  county = "COUNTYFP10",
  tract = "TRACTCE10",
  block = "BLOCKCE10",
  census_geo = "block",
  use_surname = TRUE,
  surname_only = FALSE,
  surname_year = 2010,
  use_age = FALSE,
  use_sex = FALSE,
  return_surname_flag = TRUE,
  return_geocode_flag = TRUE,
  verbose = TRUE
)
```

``` r
# Aggregate BISG race predictions to precinct level
bisg_agg <- precinct_agg_combine(
  voter_file = ramapo_bisg,
  group_col = c("precinct","precinct_name"),
  race_cols = c("pred.whi", "pred.bla", "pred.his", "pred.asi", "pred.oth"),
  include_total = FALSE
)

# Standardize columns to be lower case
bisg_agg$precinct_name <- tolower(bisg_agg$precinct_name)
```

## Prepare election data

``` r
# Import election data
election_results <- read.csv(paste(base_path, "elec_results.csv", sep=""), header=TRUE)

# Remove CVAP race data
election_results <- election_results %>% select(-c(name,cvap,pct_latino,pct_black,pct_white))

# Standardize variable names
names(election_results) <- tolower(names(election_results))
# Remove periods from variable anmes
names(election_results) <- gsub("\\.","",names(election_results))
```

## Combine demographic and election results

``` r
# Merge election results and bisg race predictions
east_ramapo <- merge(election_results, bisg_agg, by = c('precinct','precinct'))

# Standardize race variables
names(east_ramapo) <- gsub("pred.", "pct_",names(east_ramapo))
names(east_ramapo) <- gsub("_prop", "", names(east_ramapo))
names(east_ramapo)

# Combine minority groups 
east_ramapo$pct_minority <- rowSums(east_ramapo[,c('pct_bla','pct_his','pct_asi','pct_oth')])
```

``` r
# Set up for ei functions
races <- c('pct_whi','pct_minority')
# There are three contests for this year, separate them 
cands1 <- c('charlespierre','lefowitz','jones','write')
cands2 <- c('rothman', 'morales', 'write2')
cands3 <- c('white', 'eisenbach', 'ramirez', 'write3')

# Separate out contests
contest1 <- east_ramapo[,c('precinct','precinct_name',
                      races, 'tot_1', cands1)]

contest2 <- east_ramapo[,c('precinct','precinct_name',
                      races, 'tot_2', cands2)]

contest3 <- east_ramapo[,c('precinct','precinct_name',
                      races, 'tot_3', cands3)]
```

``` r
## We'll focus on contest 1 from here
# Drop missing values
contest1  <- resolve_missing_vals(
  data = contest1,
  cand_cols = cands1,
  race_cols = races,
  totals_col = 'tot_1',
  na_action = "DROP"
)

# Dedpulicate precincts
contest1 <- dedupe_precincts(
  data = contest1,
  id_cols = 'precinct'
)

# Standardize candidates and make them proportions
contest1[,cands1] <- contest1[,cands1] / rowSums(contest1[,cands1])

# Make candidate columns with pct_ prefix
contest1 <- contest1 %>% 
            rename_at(vars(all_of(cands1)), ~paste0('pct_',.))

cands1 <- paste0('pct_',cands1)
```

## Plot bivariate and get coefficients

``` r
# Plot candidate against race proportions
plot_bivariate(
  data = contest1,
  cand_cols = cands1,
  race_cols = races
)
```

``` r
# Get correlation coefficients
race_cand_cors(
  data = contest1,
  cand_cols = cands1,
  race_cols = races
)
```

## Run iterative EI and RxC

``` r
# Run ei and save out plots to designated path
ei_contest1 <- ei_iter(
    data = contest1,
    cand_cols = cands1,
    race_cols = races,
    totals_col = 'tot_1',
    verbose= TRUE,
    plots = TRUE,
    plot_path = "/home/jovyan/shared/east_ramapo/output/plots",
    name = "Iter"
)
```

``` r
# Tune RxC parameters (Set plot path so you can look at the diagnostic plots)
rxc_diag <- ei_rxc(
    contest1, cands1, races, 'tot_1', 
    diagnostic = TRUE, par_compute = TRUE, verbose = TRUE,
    plot_path = "/home/jovyan/shared/east_ramapo/output/plots"
)
# You can change some of the RxC parmeters after taking a look at the plot outputs for the diagnostic
```

``` r
# Run RxC  
# In this example we've increased the total draws after our diagnostic
rxc_contest1  <- ei_rxc(
    contest1, cands1, races, 'tot_1', 
    totaldraws = 20000,
    verbose = TRUE, name = "RxC")
```

``` r
# Compare iterative EI and RxC results
plot(ei_contest1, rxc_contest1)
```
