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

**Data Science Leads:** Scott Henderson (University of Washington) and Spencer Wood (University of Washington)

**DSSG Fellows:** Juandalyn Burke (University of Washington), Ari Decter-Frain (Cornell University), Hikari Murayama (UC Berkley), Pratik Sachdeva (UC Berkley)

# Abstract

A more representative democracy benefits everyone. The Voting Rights Act (VRA) of 1965 was established to provide every citizen the right to vote in a fair electoral process. Specifically, the VRA prohibits discriminatory voting practices such as racially polarized voting, and voter dilution from being enacted on racial and language minorities. Racially polarized voting occurs when groups of different race or ethnicities have divergent candidate preferences. In addition, voting dilution happens when the racial majority is able to outvote and bloc the racial minority's vote. However, proving that racially polarized voting and voting dilution exist is a challenge. The United States election process does not require collecting information about the race or ethnicity of the voting population in an election, and therefore difficult to assess voting behavior for different racial and ethnic groups. Our work aimed to estimate the racial identities of voters and use the methods of ecological inference to improve a software package in R called, eiCompare<https://github.com/RPVote/eiCompare> that facilitates various advanced methods for quantifying vote dilution. eiCompare uses aggregate data to infer individual racial and ethnic identifications and compares three methods of ecological inference: King’s ecological inference, iterative ecological inference and RxC, to detect racially polarized voting and voter dilution. We added several features to the tool including: geocoding capabilities, improved functionality for estimating the racial identities of voters, improved visualization of ecological inference results, parallel processing, and the ability to conduct a performance analysis with historical voting data. With the new improvements of the eiCompare, we believe that the package may be more accessible to future voting rights cases and readily used to identify areas were racially polarized voting and voter dilution occur.

For more program-related information, visit the UW Data Science for Social Good (DSSG) program<https://escience.washington.edu/dssg/>
