---
title: "Bayesian Improved Surname Geocoding (BISG)"
author: "null"
date: "8/5/2020"
output:
  pdf_document: default
  html_document: default
---
This vignette demonstrates how to perform Bayesian Improved Surname Geocoding when the race/ethncity of individuals are unknown within a dataset.

## *What is Bayesian Improved Surname Geocoding?*

Bayesian Improved Surname Geocoding (BISG) is a method that applies the Bayes Rule/Theorem to predict the race/ethnicity of an individual using the individual's surname and geocoded location [Elliott et. al 2008, Elliot et al. 2009, Imai and Khanna 2016]. 

Specifically, BISG first calculates the prior probability of individual *i* being of a ceratin racial group *r* given their surname *s* or $$Pr(R_i=r|S_i=s)$$. The prior probability created from the surname is then updated with the probability of the individual *i* living in a geographic location *g* belonging to a racial group *r*, or $$Pr(G_i=g|R_i=r)$$). The following equation describes how BISG calculates race/ethnicity of individuals using Bayes Theorem, given the surname and geographic location, and specifically when race/ethncicty is unknown :


$$Pr(R_i=r|S_i=s, G_i=g)=\frac{Pr(G_i= g|R_i =r)Pr(R_i =r |S_i= s)}{\sum_{i=1}^n Pr(G_i= g|R_i =r)Pr(R_i =r |S_i= s)}$$

  
In R, the package that performs BISG is called, WRU: Who Are You `wru` [https://cran.r-project.org/web/packages/wru/index.html]. This vignette will walk you through how to prepare your geocoded voter file for performing BISG by stepping you throught the processing of cleaning your voter file, prepping voter data for running the BISG, and finally, performing BISG to obtain racial/ethnic probailities of individuals in a voter file.

## *Performing BISG on your data*
We will perform BISG using the previous Gwinnett and Fulton county voter registration data called `ga_geo.csv` that was geocoded in the **eiCompare: Geocoding vignette**. 

The first step in performing BISG is to geocode your voter file addresses. For information on geocoding, visit the Geocoding Vignette. 

Let's begin by loading your geocoded voter data into R/RStudio.

### Step 1: Load R libraries/packages, voter file, and census data
Load the R packages needed to perform BISG. If you have not already downloaded the following packages, please install these packages.


```r
# Install eiCompare
suppressPackageStartupMessages({
  devtools::install_github("RPVote/eiCompare@master")
})
```

```
## Using github PAT from envvar GITHUB_PAT
```

```
## Skipping install of 'eiCompare' from a github remote, the SHA1 (a2c1b348) has not changed since last install.
##   Use `force = TRUE` to force installation
```


```r
# Load libraries
suppressPackageStartupMessages({
  library(devtools)
  library(tidyverse)
  library(stringr)
  library(tigris)
  library(leaflet)
  library(sf)
  library(eiCompare)
  library(wru)
  library(readr)
})
```


```r
# source files
source("~/github/eiCompare/R/wru_predict_race_wrapper.R")
source("~/github/eiCompare/R/voter_file_utils.R")
```

Load in census data, the shape_file and geocoded voter registation data with latitude and longitude coordinates Gwinnett and Fulton .

Make sure to load your census data that details certain geographies (i.e. counties, cities, tracts, blocks, etc.)

```r
# Load Georgia census data
path_census <- "~/shared/georgia/"
census_data <- readRDS(paste(path_census, "georgia_census.rds", sep = ""))
```

Next, load the state shape file using the sf::st_read() function.

```
## Using FIPS code '13' for state 'GA'
```

```
## Warning in CPL_read_ogr(dsn, layer, query, as.character(options), quiet, : GDAL Error 1: PROJ: proj_identify: Open of /srv/
## conda/envs/notebook/share/proj failed
```

```
## Using FIPS code '135' for 'Gwinnett County'
```

```
## Using FIPS code '121' for 'Fulton County'
```

Load geocoded voter file.

```r
# Load geocoded voter registration file
voter_file_geocoded <- read_csv("~/github/eiCompare/data/ga_geo.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   county_code = col_double(),
##   voter_id = col_double(),
##   street_number = col_double(),
##   zipcode = col_double(),
##   cxy_tiger_line_id = col_double(),
##   STATEFP10 = col_double(),
##   COUNTYFP10 = col_double(),
##   TRACTCE10 = col_double(),
##   BLOCKCE10 = col_double()
## )
```

```
## See spec(...) for full column specifications.
```

Obtain the first six rows of the voter file to check that the file has downloaded properly.

```r
# Check the first six rows of the voter file
head(voter_file_geocoded, 6)
```

```
## # A tibble: 6 x 25
##   county_code county_name voter_id voter_status last_name first_name street_number street_name street_suffix city  state
##         <dbl> <chr>          <dbl> <chr>        <chr>     <chr>              <dbl> <chr>       <chr>         <chr> <chr>
## 1          60 Fulton             1 A            LOCKLER   GABRIELLA           1084 Howell Mil… NW            Atla… GA   
## 2          60 Fulton             2 A            RADLEY    OLIVIA              7305 Village Ce… <NA>          Fair… GA   
## 3          60 Fulton             3 A            BOORSE    KEISHA              6200 Bakers Fer… SW            Atla… GA   
## 4          67 Gwinnett          12 A            MAZ       SAVANNAH            1359 Beaver Rui… <NA>          Norc… GA   
## 5          67 Gwinnett          13 A            GAULE     NATASHIA            2961 Lenora Chu… <NA>          Snel… GA   
## 6          67 Gwinnett          15 A            MCMELLEN  ISMAEL              4305 Paxton Ln   <NA>          Lilb… GA   
## # … with 14 more variables: zipcode <dbl>, street_address <chr>, final_address <chr>, cxy_address <chr>, cxy_status <chr>,
## #   cxy_quality <chr>, cxy_matched_address <chr>, cxy_tiger_line_id <dbl>, cxy_tiger_side <chr>, STATEFP10 <dbl>,
## #   COUNTYFP10 <dbl>, TRACTCE10 <dbl>, BLOCKCE10 <dbl>, geometry <chr>
```

View the column names of the voter file. Some of these columns will be used along the journey to performing BISG.

```r
# Find out names of columns in voter file
names(voter_file_geocoded)
```

```
##  [1] "county_code"         "county_name"         "voter_id"            "voter_status"        "last_name"          
##  [6] "first_name"          "street_number"       "street_name"         "street_suffix"       "city"               
## [11] "state"               "zipcode"             "street_address"      "final_address"       "cxy_address"        
## [16] "cxy_status"          "cxy_quality"         "cxy_matched_address" "cxy_tiger_line_id"   "cxy_tiger_side"     
## [21] "STATEFP10"           "COUNTYFP10"          "TRACTCE10"           "BLOCKCE10"           "geometry"
```

Check the dimensions (the number of rows and columns) of the voter file.

```r
# Get the dimensions of the voter file
dim(voter_file_geocoded)
```

```
## [1] 12 25
```
There are 12 voters (or observations) and 25 columns in the voter file.

Convert geometry column name into two columns for latitude and longitude points.

```r
voter_file_geocoded <- voter_file_geocoded %>%
  extract(geometry, c("lon", "lat"), "\\((.*), (.*)\\)", convert = TRUE)
```

### Step 2: De-duplicate the voter file.

The next step involves removing duplicate voter IDs from the voter file, using the `dedupe_voter_file` function. 

```r
# Rename registration_id to voter_id
names(voter_file_geocoded)[names(voter_file_geocoded) == "registration_number"] <- "voter_id"
```


```r
# Remove duplicate voter IDs (the unique identifier for each voter)
voter_file_dedupe <- dedupe_voter_file(voter_file = voter_file_geocoded, voter_id = "voter_id")
```

There are no duplicate voter IDs in the dataset.

### Step 3: Perform BISG and obtain the predicted race/ethnicity of each voter.

```r
# Convert the voter_shaped_merged file into a data frame for performing BISG.
voter_file_complete <- as.data.frame(voter_file_dedupe)
class(voter_file_complete)
```

```
## [1] "data.frame"
```


```r
# Perform BISG
bisg_file <- eiCompare::wru_predict_race_wrapper(
  voter_file = voter_file_complete,
  census_data = census_data,
  voter_id = "voter_id",
  surname = "last_name",
  state = "GA",
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

```
## Matching surnames.
```

```
## Performing BISG to obtain race probabilities.
```

```
## Some voters failed geocode matching. Matching at a higher level.
```

```
## Some surnames did not match at higher geographic level. Using surname only for these cases.
```

```
## BISG complete.
```


```r
# Check BISG output
head(bisg_file$bisg)
```

```
##    voterid state  surname county tract block matched_surname pred.whi pred.bla pred.his pred.asi pred.oth
## 3        1    GA  lockler    121   600  1064            TRUE   0.8067  0.02000  0.01670  0.00000  0.15670
## 2        2    GA   radley    121 10514  3038            TRUE   0.8752  0.07680  0.02480  0.00455  0.01865
## 1        3    GA   boorse    121 10303  2019            TRUE   0.9921  0.00000  0.00395  0.00395  0.00000
## 7       12    GA      maz    135 50424  1011            TRUE   0.4254  0.03730  0.41040  0.11940  0.00750
## 12      13    GA    gaule    135 50719  1008            TRUE   0.9329  0.01675  0.03360  0.01675  0.00000
## 10      15    GA mcmellen    135 50714  2027            TRUE   0.8960  0.01600  0.05600  0.01600  0.01600
```

```r
# Assign BISG list data frame, bisg$bisg, to bisg_tbl
bisg_tbl <- bisg_file$bisg
```


## Summarizing BISG output

```r
summary(bisg_tbl)
```

```
##     voterid         state             surname             county             tract              block          
##  Min.   : 1.00   Length:12          Length:12          Length:12          Length:12          Length:12         
##  1st Qu.: 9.00   Class :character   Class :character   Class :character   Class :character   Class :character  
##  Median :14.00   Mode  :character   Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :12.25                                                                                                 
##  3rd Qu.:17.25                                                                                                 
##  Max.   :20.00                                                                                                 
##  matched_surname    pred.whi         pred.bla           pred.his          pred.asi          pred.oth       
##  Mode:logical    Min.   :0.0229   Min.   :0.000000   Min.   :0.00000   Min.   :0.00000   Min.   :0.000000  
##  TRUE:12         1st Qu.:0.3320   1st Qu.:0.003112   1st Qu.:0.01495   1st Qu.:0.00440   1st Qu.:0.005788  
##                  Median :0.8027   Median :0.018375   Median :0.02360   Median :0.00780   Median :0.012800  
##                  Mean   :0.6304   Mean   :0.113550   Mean   :0.13104   Mean   :0.09735   Mean   :0.027700  
##                  3rd Qu.:0.9052   3rd Qu.:0.087850   3rd Qu.:0.03920   3rd Qu.:0.01876   3rd Qu.:0.033775  
##                  Max.   :0.9921   Max.   :0.875300   Max.   :0.94890   Max.   :0.95920   Max.   :0.156700
```

### Look at BISG race predictions by county

```r
# Obtain aggregate values for the BISG results by county
bisg_agg <- precinct_agg_combine(
  voter_file = bisg_tbl,
  group_col = "county",
  race_cols = c("pre.whi", "pred.bla", "pred.his", "pred.asi", "pred.oth"),
  race_keys = c("White", "Black", "Hispanic", "Asian", "Other"),
  include_total = FALSE
)

# Assign values to county names for barplot
bisg_agg$county[bisg_agg$county == "121"] <- "Fulton"
bisg_agg$county[bisg_agg$county == "135"] <- "Gwinnett"

# Table with BISG race predictions by county
head(bisg_agg)
```

```
## # A tibble: 2 x 6
##   county   pred.whi_prop pred.bla_prop pred.his_prop pred.asi_prop pred.oth_prop
##   <chr>            <dbl>         <dbl>         <dbl>         <dbl>         <dbl>
## 1 Fulton           0.891        0.0323        0.0152       0.00283        0.0584
## 2 Gwinnett         0.543        0.141         0.170        0.129          0.0175
```

### Barplot of BISG results

```r
bisg_bar <- bisg_agg %>%
  gather("Type", "Value", -county) %>%
  ggplot(aes(county, Value, fill = Type)) +
  geom_bar(position = "dodge", stat = "identity") +
  labs(title = "BISG Predictions for Fulton and Gwinnett Counties", y = "Proportion", x = "Counties") +
  theme_bw()

bisg_bar + scale_color_discrete(name = "Race/Ethnicity Proportions")
```

![plot of chunk unnamed-chunk-18](figure/unnamed-chunk-18-1.png)

### Choropleth Map
Finally, we will map the BISG data onto choropleth maps.


```r
bisg_df <- bisg_tbl %>%
  select(block, pred.whi, pred.bla, pred.his, pred.asi, pred.oth)

bisg_df
```

```
##    block pred.whi pred.bla pred.his pred.asi pred.oth
## 3   1064   0.8067  0.02000  0.01670  0.00000  0.15670
## 2   3038   0.8752  0.07680  0.02480  0.00455  0.01865
## 1   2019   0.9921  0.00000  0.00395  0.00395  0.00000
## 7   1011   0.4254  0.03730  0.41040  0.11940  0.00750
## 12  1008   0.9329  0.01675  0.03360  0.01675  0.00000
## 10  2027   0.8960  0.01600  0.05600  0.01600  0.01600
## 11  3010   0.7439  0.19420  0.02060  0.00500  0.03640
## 8   1011   0.0517  0.87530  0.02540  0.00300  0.04460
## 4   4002   0.9808  0.00000  0.00000  0.00960  0.00960
## 5   2002   0.0379  0.00360  0.94890  0.00600  0.00350
## 6   3000   0.7988  0.12100  0.02240  0.02480  0.03290
## 9   3003   0.0229  0.00165  0.00970  0.95920  0.00655
```


```r
# Rename the by= variable to fips code name
names(bisg_df)[names(bisg_df) == "block"] <- "BLOCKCE10"

# Join bisg and shape file
bisg_sf <- left_join(shape_file, bisg_df, by = "BLOCKCE10")
```

#### Plot Map of Proportion of Black Voters

```r
# Plot choropleth map of race/ethnicity predictions for Fulton and Gwinnett counties
plot(bisg_sf["pred.bla"], main = "Proportion of Black Voters identified by BISG")
```

![plot of chunk unnamed-chunk-21](figure/unnamed-chunk-21-1.png)

#### Plot Map of Proportion of White Voters

```r
plot(bisg_sf["pred.whi"], main = "Proportion of White Voters identified by BISG")
```

![plot of chunk unnamed-chunk-22](figure/unnamed-chunk-22-1.png)

#### Plot Map of Proportion of Hispanic Voters

```r
plot(bisg_sf["pred.his"], main = "Proportion of Hispanic Voters identified by BISG")
```

![plot of chunk unnamed-chunk-23](figure/unnamed-chunk-23-1.png)

#### Plot Map of Proportion of Asian Voters

```r
plot(bisg_sf["pred.asi"], main = "Proportion of Asian Voters identified by BISG")
```

![plot of chunk unnamed-chunk-24](figure/unnamed-chunk-24-1.png)

#### Plot Map of Proportion of Other Voters

```r
plot(bisg_sf["pred.oth"], main = "Proportion of 'Other' Voters identified by BISG")
```

![plot of chunk unnamed-chunk-25](figure/unnamed-chunk-25-1.png)
