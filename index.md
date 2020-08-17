---
layout: default
---

<img src="{{ site.url }}{{ site.baseurl }}/assets/img/eScience.png">


# Detection of Vote Dilution: New tools and methods for protecting voting rights

In this project, we developed tools to aid the detection of racial
gerrymandering and vote dilution. These tools are necessary in current and
future court cases to provide sufficient evidence of vote dilution in an
effort to require fairer redistricting. Thus, these tools must be properly
vetted, providing accurate results, and usable beyond technical specialists, so
that their utilization can expand.

## The Team

**Project Leads:** Matt Barreto (UCLA) and Loren Collingwood (University of New Mexico).

**Data Science Leads:** Scott Henderson and Spencer Wood

**DSSG Fellows:** Juandalyn Burke, Ari Decter-Frain, Hikari Murayama, Pratik Sachdeva

# Abstract

The Voting Rights Act (VRA) of 1965 was established to provide every citizen the right to vote in a fair electoral process. Specifically, the VRA prohibits discriminatory voting practices such as gerrymandering, racially polarized voting, and voter dilution from being enacted on racial and language minorities. Each of these discriminatory practices can aid in bloc-ing minority voters from electing a candidate of their choice that is more representative of their own communities. However, proving that racially polarized voting and voting dilution exist is a challenge due to the need to know the racial identity of the voter, which is not commonly present in voter records. Our work aimed to estimate the racial identities of voters and use the methods of ecological inference to improve upon an R package tool called, eiCompare. eiCompare uses aggregate data to infer individual characteristics and compares three methods of ecological inference: King’s ecological inference, iterative ecological inference and RxC, to detect racially polarized voting and voter dilution. We added several features to the tool including: geocoding capabilities, improved functionality for estimating the racial identities of voters, provided additional visualization to ecological inference results, utilized parallel processing to make the package more efficient to run and built-in the ability to conduct performance analyses. With the new improvements of the eiCompare package, we believe that the package may be more accessible to cases involving proposed racially polarized voting and voter dilution.
