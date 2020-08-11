---
layout: page
title: eiCompare
---


# eiCompare

`eiCompare` is an R package built to help practitioners and academics quantify racially polarized voting with ease and confidence. eiCompare was built with several types of users in mind:

-   Expert witnesses in voting rights litigation who need to accurately quantify vote dilution in an area and present the results convincingly to a judge or jury.
-   National voting rights advocacy organizations trying to identify elections across the country where vote dilution might be at play.
-   Grassroots organizations looking for data-driven tools to fuel their fight against vote dilution at the local level.
-   Academics who study the causes and consequences of vote dilution and racially polarized voting.

`eiCompare` wraps around existing R packages such as [`ei`]([https://gking.harvard.edu/eiR](https://gking.harvard.edu/eiR)), [`eiPack`](https://www.rdocumentation.org/packages/eiPack/versions/0.1-7), and [`WRU`](https://github.com/kosukeimai/wru), adding a layer of polish, usability and statistical robustness to their foundational tools. It contains a suite of data cleaning tools to help users work with voter files, election results, and census data while preventing common mistakes.  It also contains a number of functions that enable users to understand the uncertainty in their estimates. Finally, `eiCompare` adds elegant visualizations that can be produced with just a few lines of code.

## What can `eiCompare` do?

`eiCompare` has tools that help at every step of the pipeline for quantifying racially polarized voting:

![eiCompare data science pipeline](images/eiCompare_diagram.png)
Users can measure racially polarized voting in any election, provided they have access to election results and the corresponding voter file (see below for information about the different types of data needed to measure racially polarized voting). The analysis proceeds via the following pipeline.

1. **Geocoding:** To effectively estimate the race of voters, we need to know the Census block in which they reside. To figure this out, first geocode the addresses of the voters to get latitude and longitude values corresponding to their exact locations on a map. Then, use those locations to figure out the Census block in which they reside.

2. **Bayesian Improved Surname Geocoding (BISG):** We use BISG to estimate the race of voters on the basis of their surnames and the census block in which they reside. Next, use these estimates count how many voters of each race turned out to vote in each election precinct.

3. **Ecological Inference (EI):** We merge our estimates of racial turnout with the actual results of elections in each precinct. Using the estimates of racial turnout and the election results, we can apply EI to estimate the proportion of voters from each racial group voted for each political candidate.

4. **Performance Analysis:** Alternatively, we can use the race estimates from BISG to compare different electoral map proposals and predict how they might affect racial turnout in future elections. 

This process involves a lot of data cleaning, merging different datasets, and doing spatial joins to move between the different steps. `eiCompare` has functions to help with all of these little steps too!


### Data

[] enter data here

## Our contribution:

  Matt Baretto, Loren Collingwood, and their co-authors developed v1.0 of `eiCompare` as a  'minimum viable product' while conducting analyses for the East Ramapo court case. This summer we worked to revamp the package and demonstrate it's capabilities through a different applications.

### Package improvements

- We added new functionality, including:
	- Performance analysis
	- Tools for geocoding voter file addresses
	- Functions for processing data throughout the pipeline
- We modernized the style, structure, and documentation of the package, preparing it for open-sourced deployment on [CRAN](https://cran.r-project.org/), the software distribution system for the R programming language.
- We added support for parallel processing, resulting in 5X improvements in efficiency.
-  We improved handling of statistical uncertainty throughout the pipeline by enabling the propagation of errors from start to finish
- We added new and revamped data visualization functions for presenting results at every step.

### Applications

Beyond upgrading the package, we also applied these tools in additional research work:

- We used `eiCompare`'s performance analysis tools to compare newly proposed maps by the plaintiffs and defendants in the East Ramapo case. As of August 2020, the courts were considering these analyses in deciding with which map to proceed.

- We applied quantified racially polarized voting using data from the 2018 Georgia Gubernatorial election. We benchmarked `eiCompare`'s race estimation against voters' self reported race, and benchmarked our estimates of racially polarized voting against  measures from exit polls. Moving forward, we plan to develop these benchmarks into an academic publication that can inform the courts' decisions about the validity of `eiCompare`'s tools.