---
layout: page
title: Motivation
---

**Questions**

This project develops tools for quantifying racially polarized voting in elections at all levels of the US government. Better measurement of racially polarized voting will empower organizations like the ACLU, NAACP, and thousands of local community groups to begin their own legal battles against vote dilution. To this end, we seek to answer the following questions:

1. How can we manage the noise and missing data that propagates uncertainty throughout our estimates of racially polarized voting?
2. How can we produce informative, comprehensible, and compelling visualizations of our results that can be used in court?
3. How do we integrate or use other sources of data (i.e. twitter, newspapers, school data, housing information, etc.) to improve data-driven evidence?
4. How do we ensure ethical consideration and practices while creating tools that use identifiable information?

**Background**

Why is this important?
What work has previously been done?

**Stakeholders**

The overall goal of this project is to develop a suite of tools, as an easily accessible software package, that allow other practitioners to assess the presence of voter dilution. Thus, beyond this team, the primary stakeholders most reliant on this project include:

* **Matt Barreto** and **Loren Collingwood**, the project leads, who will
  continue the development of the suite of tools after handoff. Most recently,
  Matt and Loren served as expert witnesses in the East Ramapo school district
  case, using eiCompare to perform the analyses that informed their testimony.
* The **New York Civil Liberties Union (NYCLU)**, which has committed to using the
  package in-house after its successful usage in the recent East Ramapo case.
  Within NYCLU, we are in contact with **Perry Grossman**, who was the lead attorney
  in East Ramapo and is informing our development of the package according to
  his team's needs.
* **Data science practitioners** at NYCLU and other advocacy organizations who
  will use the package to analyze data in litigation.

These primary stakeholders will benefit from this package by streamlining their data analysis pipeline for voting right litigation. Beyond these primary stakeholders, there are a host of secondary stakeholders whose viewpoints may influence the development of the project:

* Other **expert witnesses** who may rely on the software package for their
  testimony in future voting rights cases.
* The charts and graphs produced by the package will eventually be reviewed by
  **judges**, requiring that these visualizations effectively communicate the key
  ideas.
* Other **advocacy groups**, such as the UCLA Voting Rights Project.
* **Academics** within the realm of political science who may use the package for
  their own research questions.

The benefits these stakeholders receive from the package stem from its usability and accuracy in their use cases. Thus, their viewpoint may inform the development of certain functionalities or documentation in the package. Overall, however, the ultimate beneficiaries of this project are:

* **Voters in minority groups** who do not receive adequate representation in their local governments due to unrepresentative districting.
* These voters are often represented by **local community organizations** who advocate on their behalf, both within the realm of voting rights and beyond. 

These stakeholders will benefit from the package via its usage in upcoming voting rights litigation. While these stakeholders will likely not use the package, their role in pushing forward voting rights cases is instrumental. Their experiences and advocacy spur litigation (by raising awareness of their voting rights issues). Furthermore, they provide narratives that substantiate and enrich the evidence quantified by the data.

**Ethics**

There are multiple facets of ethics our team has considered for this project. Ethical considerations must be taken into account from the moment voting registration and history data is in our possession. Furthermore, the team also considered how imprecision can have implications beyond our direct stakeholders and how results using the eiCompare package can affect various individuals and groups. We aimed to address these concerns as we completed the project.

*Voter privacy*

Firstly, we need to consider the privacy of voters. eiCompare processes voter registration and election history data from all over the country. These data files contain a great deal of information, such as names and addresses, that can be linked directly back to an individual. Although these data files are generally publicly available by filling out an inquiry form to a state or county level database, our team understood that this data should not be freely distributed and protected. We needed to protect the individuals within the databases as disseminating this information does not add to our project. Making this data freely available could lead to the nefarious use of sensitive data that may otherwise not have occurred. Data used in this project as part of examples and package development have been anonymized by removing personal information and creating an alternate unique ID. 

*Consequences of results*

Secondly, eiCompare requires multiple processing and analysis steps to assess and visualize the presence of voter dilution from raw voter files. Each step requires meticulous error handling, missing data analysis, and accuracy assessments to ensure viability of produced results.. Overestimation and underestimation of voting dilution can support or go against the narratives presented by representative constituents. However, by including checks and sensitivity analyses along the way, a user will have more confidence in their analysis and in turn will better the package’s reputation as a reliable tool. A well polished package would be indispensable for rigorous analytical work for litigation as well as inform equitable redistricting in 2021. 

*Diverse stakeholders*

Lastly, we considered the multitude of potential direct and indirect stakeholders, as well as primary and secondary stakeholders, who would be affected by eiCompare. Each individual and group has their own benefits and risks with the creation of eiCompare and the team considered the package’s impact on all of these players. As stated above, accuracy, as well as a user friendly package, benefits the greater good of the community to make just and equitable voting more attainable. Even though a courtroom may set some of these stakeholders on opposing sides, a stable analysis from eiCompare would aid in ensuring fair elections.
