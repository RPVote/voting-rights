---
layout: default
---

<img src="{{ site.url }}{{ site.baseurl }}/assets/img/eScience.png">


# Detection of Vote Dilution: New tools and methods for protecting voting rights

In this project, we developed tools to aid the detection of racial
gerrymandering and vote dilution. These tools are necessary in current and
future court cases to provide sufficient evidence of vote dilution in an
effort to require fairer redistricting. We engaged with stakeholders throughout 
development to ensure usability and accuracy, as well as to ensure that these 
analyses are available and accessible to many.


## The Team

**Project Leads:** Matt Barreto (UCLA) and Loren Collingwood (University of New Mexico)

**Data Science Leads:** Scott Henderson (University of Washington) and Spencer Wood (University of Washington)

**DSSG Fellows:** Juandalyn Burke (University of Washington), Ari Decter-Frain (Cornell University), Hikari Murayama (UC Berkeley), Pratik Sachdeva (UC Berkeley)

# Abstract

A more representative democracy benefits everyone. The Voting Rights Act (VRA) of 1965 was established to provide every citizen the right to vote in a fair electoral process. Specifically, the VRA prohibits discriminatory voting practices such as voting dilution from being enacted on racial and language minorities. Since the United States election process does not require collecting information about the race or ethnicity of the voting population in an election, it is hard to prove two of the criteria needed to conclude that voting dilution is occurring. The first criteria is Racially Polarized Voting (RPV). RPV occurrs when groups of different race or ethnicities have divergent candidate preferences. The second criteria is that the racial majority is able to outvote and bloc the racial minority from electing their preferred candidate. [eiCompare](https://github.com/RPVote/eiCompare) is an R software package that proves these criteria. This package uses aggregate data to infer individual racial and ethnic identifications and compares three methods of ecological inference (Goodman’s ecological regression, King’s ecological inference, and RxC) to detect RPV and determine if voting dilution is occurring. We added several features to the tool including: geocoding capabilities, improved functionality for estimating the racial identities of voters, improved visualization of ecological inference results, parallel processing, and the ability to conduct a performance analysis with historical voting data. With the new improvements of the eiCompare, we believe that the package may be more accessible to future voting rights cases and readily used to identify areas were racially polarized voting and voter dilution occur.

For more program-related information, visit the [UW Data Science for Social Good (DSSG) program](https://escience.washington.edu/dssg/).
