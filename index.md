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

# Abstract or executive summary

Section 2 of the Voting Rights Act (VRA) allows voters to challenge district
boundaries if they believe gerrymandering has been used to dilute their vote and
block them from getting candidates of choice for their community elected. To win
a VRA lawsuit, plaintiffs must prove that voting patterns in their community are
"racially polarized" with Whites and minorities voting in opposite directions
for different candidates. However, most states do not collect racial data on
voters, and the voter's ballot is secret. To analyze voting patterns, social
scientists use a statistical method called Ecological Inference (EI) to
determine how different groups vote. But this method relies on imprecise census
data and often creates biased estimates of voting patterns. Recently a new
methodology has been developed for estimating voters’ race and ethnicity, which
offers great promise for improving voting estimates and being a helpful tool for
upholding minority voting rights.

This project uses the existing eiCompare R software package (Collingwood et al. 2016) to update and modernize ecological inference (EI) analysis to be used in voting rights and redistricting efforts. In particular, we propose numerous methodological, programming, and statistical advancements to the EI models in eiCompare to allow for a more accurate and precise model to capture racial voting patterns. In particular we will incorporate Bayesian Improved Surname Geocoding (BISG, see Imai and Khanna 2016) to analyze the surname and address of voters to estimate probabilities of their race or ethnicity, which can then be used in EI models. We will also troubleshoot and address bugs in the various EI model code to ensure that accurate estimates of voter preferences are being calculated and can be better used by state and federal courts when evaluating voting rights claims.