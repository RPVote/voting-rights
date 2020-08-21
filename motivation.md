---
layout: page
title: Motivation
---

In the spring of 2020, the NAACP won a landmark [voting rights case](https://forward.com/news/breaking-news/447322/east-ramapo-judge-rules-violation/) in the East
Ramapo School District of Rockmount County, New York by convincing the judge
that precinct boundaries drawn for school board elections substantially diluted
the Black and Latino minority vote. Vote dilution violates Section 2 of the
Voting Rights Act (VRA), which “prohibits drawing election districts in ways
that improperly dilute minorities’ voting power.” During the case, expert
witnesses Dr. Matt Barreto and Dr. Loren Collingwood, also the DSSG project
leads, employed a wide array of novel data science methods to quantify vote
dilution. Their victory cemented the validity of these tools in a legal setting
and future voting rights litigation.

In this DSSG project, we build on Dr. Barreto and Dr. Collingwood's initial
version of `eiCompare`, software in the form of an `R` package that enables
anyone in the world to measure vote dilution using the same tools that were used
in the East Ramapo case (Collingwood et al., 2016). The goal of this project is
to expand the software's functionality, incorporate additional statistically rigorous tests, and make it accessible to everyone so that groups and organizations
throughout the country can use it to fight for their electoral rights.

![Image 1. Decelle (2017) "Students of the East Ramapo School District hold a
sign during the One Voice United Rally in Albany". Retrieved July 7, 2020 from
The Atlantic website:
https://www.theatlantic.com/education/archive/2017/11/another-blow-to-one-of-americas-most-controversial-school-board/546227/
.](images/eastramapo_schooldistrict_rally_theatlantic.PNG)

### The legal context

Without fair representation, minority citizens cannot effectively support and
protect their own institutions. The East Ramapo School Board example illustrates
how vote dilution can cause an institution like education to erode over time.
During the court proceedings, lawyers from the New York Civil Liberties Union
(NYCLU) explained how a lack of minority representation on the board led to the
gradual siphoning of funds from public schools attended by Black and Latino
students to private schools attended by white students. As a result, the
educational experiences of public school attendees began to deteriorate:

* “The board eliminated hundreds of public school teaching, staff, and
  administrative positions and eliminated classes and programs”
* “Public school buildings fell into disrepair and custodial services were
  reduced”
* “Students were given academically deficient schedules full of free time and
  filler”
* “The board closed two public schools over minority opposition”
* “Graduation and test scores sank”

To convince the judge that the defunding of public schools ultimately resulted
from vote dilution, the plaintiffs’ evidence needed to pass the **Gingles
Test**, established as precedent for assessing minority vote dilution in the
1986 case Thornburg v. Gingles. The Gingles Test requires showing:

  1. The group of minority voters is sufficiently large and geographically
     compact
  2. Minority voters are politically cohesive in supporting their candidate of
     choice
  3. The majority votes in a bloc to usually defeat the minority’s preferred
     candidate

Requirement #1 can be assessed easily by looking at Census demographic data.
Requirements #2 and #3, however, can be difficult to demonstrate because voting
is confidential - we can never know who voted for which candidate with
certainty. Recently, innovations in quantitative social science have enabled
rigorous new analytic approaches to estimating the voting behavior of different
demographic groups.

### The statistical tools

Satisfying the requirements presented by the Gingles Test requires a number of
statistical techniques. Some of the most important methods are the following:

* **Ecological Inference (EI):** EI refers to a set of statistical techniques
  for estimating the behavior of individuals using data on aggregate behavior
  (Rosen et al., 2001; King, 1997).  In the context of elections, it has been
  applied in academic and legal settings to estimate the proportion of voters
  identifying with a particular race who voted for a particular candidate 
  (Barreto et al., 2019). To see how eiCompare applies these techniques, see 
  [this tutorial](ei.md), with more advanced demonstrations [here]
  (parallel_processing.md) and [here](visualizations.md).

* **Bayesian Improved Surname Geocoding (BISG):** To execute EI we need measures
  of how many people identifying with each racial group actually turned out to
  vote in the election. BISG uses demographic data (i.e. surnames, racial
  identity, and geographic data) collected from the US Census Bureau, along with
  field-specific data like voter information to predict the race or ethnicity of
  each person in the sample population (Imai and Khana, 2016; Elliott et al.
  2009). To learn more about the steps involved in BISG, see [this
  tutorial](geocoding.md) on geocoding  voter addresses and [this
  one](bisg.md) on BISG itself.

* **Performance Analysis:** `eiCompare` also contains tools to analyze maps and
  predict whether the maps will 'perform', that is, enable minority voters to
  elect their preferred candidates. Performance analysis uses BISG as well, in
  combination with more advanced spatial methods. To learn more about it, see
  [this tutorial](performance_analysis.md) that walks through an application of
  performance analysis to assess real maps that proposed as part of the East
  Ramapo case.

These tools helped the NAACP win the East Ramapo School Board case. We envision them being applied in future voting rights cases throughout the United States, particularly leading up to widespread 2021 redistricting.

## Project Goals

We hope that the new version of `eiCompare` we develop will empower
organizations like the ACLU, NAACP, and thousands of local community groups to
begin their own legal battles against vote dilution. Throughout the project, we
sought to achieve the following goals:

1. Make the software in `eiCompare` accessible to users at all skill levels,
   from potential code contributors to those with little or no experience
   programming in `R`.
2. Improve the statistical and methodological techniques in the package to make
   them as efficient, accurate, and robust as possible.
3. Apply the tools within the package to real data, and transform these
   applications into instructional materials for users.
4. Ensure that our work incorporates the feedback of our stakeholders in
   academia and national advocacy organizations, including the
   [ACLU](https://www.aclu.org/) and
   [NDRC](https://democraticredistricting.com/).

## Stakeholders

The overall goal of this project is to develop a suite of tools, as an easily
accessible software package, that allow other practitioners to assess the
presence of voter dilution. Thus, beyond this team, the primary stakeholders
most reliant on this project include:

* **Matt Barreto** and **Loren Collingwood**, the project leads, who will
  continue the development of the suite of tools after handoff. Most recently,
  Matt and Loren served as expert witnesses in the East Ramapo school district
  case, using eiCompare to perform the analyses that informed their testimony.
* The **New York Civil Liberties Union (NYCLU)**, which has committed to using
  the package in-house after its successful usage in the recent East Ramapo
  case. Within NYCLU, we are in contact with **Perry Grossman**, who was the
  lead attorney in East Ramapo and is informing our development of the package
  according to his team's needs.
* **Data science practitioners** at NYCLU and other advocacy organizations who
  will use the package to analyze data in litigation.

These primary stakeholders will benefit from this package by streamlining their
data analysis pipeline for voting right litigation. Beyond these primary
stakeholders, there are a host of secondary stakeholders whose viewpoints may
influence the development of the project:

* Other **expert witnesses** who may rely on the software package for their
  testimony in future voting rights cases.
* The charts and graphs produced by the package will eventually be reviewed by
  **judges**, requiring that these visualizations effectively communicate the
  key ideas.
* Other **advocacy groups**, such as the UCLA Voting Rights Project.
* **Academics** within the realm of political science who may use the package
  for their own research questions.

The benefits these stakeholders receive from the package stem from its usability
and accuracy in their use cases. Thus, their viewpoint may inform the
development of certain functionalities or documentation in the package. Overall,
however, the ultimate beneficiaries of this project are:

* **Voters in minority groups** who do not receive adequate representation in
  their local governments due to unrepresentative districting.
* These voters are often represented by **local community organizations** who
  advocate on their behalf, both within the realm of voting rights and beyond.

These stakeholders will benefit from the package via its usage in upcoming
voting rights litigation. While these stakeholders will likely not use the
package, their role in pushing forward voting rights cases is instrumental.
Their experiences and advocacy spur litigation (by raising awareness of their
voting rights issues). Furthermore, they provide narratives that substantiate
and enrich the evidence quantified by the data.

## Ethics

There are multiple facets of ethics our team has considered for this project.
Ethical considerations must be taken into account from the moment voting
registration and history data is in our possession. Furthermore, the team also
considered how imprecision can have implications beyond our direct stakeholders
and how results using the eiCompare package can affect various individuals and
groups. We aimed to address these concerns as we completed the project.

### Voter privacy

Firstly, we need to consider the privacy of voters. eiCompare processes voter
registration and election history data from all over the country. These data
files contain a great deal of information, such as names and addresses, that can
be linked directly back to an individual. Although these data files are
generally publicly available by filling out an inquiry form to a state or county
level database, our team understood that this data should not be freely
distributed and protected. We needed to protect the individuals within the
databases as disseminating this information does not add to our project. Making
this data freely available could lead to the nefarious use of sensitive data
that may otherwise not have occurred. Data used in this project as part of
examples and package development have been anonymized by removing personal
information and creating an alternate unique ID.

### Consequences of results

Secondly, eiCompare requires multiple processing and analysis steps to assess
and visualize the presence of voter dilution from raw voter files. Each step
requires meticulous error handling, missing data analysis, and accuracy
assessments to ensure viability of produced results.. Overestimation and
underestimation of voting dilution can support or go against the narratives
presented by representative constituents. However, by including checks and
sensitivity analyses along the way, a user will have more confidence in their
analysis and in turn will better the package’s reputation as a reliable tool. A
well polished package would be indispensable for rigorous analytical work for
litigation as well as inform equitable redistricting in 2021.

### Diverse stakeholders

Lastly, we considered the multitude of potential direct and indirect
stakeholders, as well as primary and secondary stakeholders, who would be
affected by eiCompare. Each individual and group has their own benefits and
risks with the creation of eiCompare and the team considered the package’s
impact on all of these players. As stated above, accuracy, as well as a user
friendly package, benefits the greater good of the community to make just and
equitable voting more attainable. Even though a courtroom may set some of these
stakeholders on opposing sides, a stable analysis from eiCompare would aid in
ensuring fair elections.

## Outcomes

Throughout the course of the project, we engaged with stakeholders to better
understand how software tools could better serve their needs. Thus, we strived
to ensure project had immediate and direct impact on the ability of these
stakeholders to conduct their work. Generally, the software tools we developed
make it easier, simpler, and faster to assess the prevalence of racially
polarized voting in a jurisdiction given voter and election data.

Stakeholders who have directly used `eiCompare` to perform EI on datasets have
communicated their needs and difficulties with using the package. Thus, we have
directly shaped the package in respond to their feedback. This has led to the
development of improved visualizations, preprocessing functions, documentation,
and additional features.

Furthermore, the East Ramapo case was directly impacted by the efforts of this
project. The analyses used in the court case have been formalized and folded
into the package. These tools, within `eiCompare` were then used to develop
outputs that were presented to the judge in the final stages of the court case,
demonstrating the ability of the package to effectively push forward litigation.

Lastly, in the longer term, this project can be leveraged at a wider scale by
other organizations focused on promoting redistricting and voting rights. For
example, the National Democratic Redistricting Committee utilizes a variety of
statistical analyses in their work. They operationalize these analyses to run at
scale in order to study redistricting in a variety of contexts. The NDRC has
expressed interest in scaling `eiCompare` and the tools it provides once the
package has reached completion.

## References

1. Barreto, M., Collingwood, L., Garcia-Rios, S., & Oskooii, K. A. (2019).
   Estimating Candidate Support in Voting Rights Act Cases: Comparing Iterative
   EI and EI-R× C Methods. Sociological Methods & Research, 0049124119852394.

2. Collingwood, L., Oskooii, K., Garcia-Rios, S., & Barreto, M. (2016).
   eiCompare: Comparing Ecological Inference Estimates across EI and EI: RC. R
   J., 8(2), 92.

3. Elliott, M. N., Morrison, P. A., Fremont, A., McCaffrey, D. F., Pantoja, P.,
   & Lurie, N. (2009). Using the Census Bureau’s surname list to improve
   estimates of race/ethnicity and associated disparities. Health Services and
   Outcomes Research Methodology, 9(2), 69.

4. King, G. (1997). A solution to the ecological inference problem:
   reconstructing individual behavior from aggregate data. Princeton: Princeton
   University Press.

5. Imai, K., & Khanna, K. (2016). Improving ecological inference by predicting
   individual ethnicity from voter registration records. Political Analysis,
   263-272.
