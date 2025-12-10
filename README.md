Energy, Emissions, and Equity in NYC Public Schools
================
by Jack, Ben, and Margaret

## Summary

This project investigates how energy use varies across New York City public schools and how those differences relate to student population and socioeconomic conditions. Our goal was to understand two things:
(1) Which boroughs consume the most total energy
(2) How these patterns change once energy use is normalized on a per-student basis, revealing potential inequities in school infrastructure and resource distribution.

We also incorporated an estimated carbon emissions model to compare energy consumption with environmental impact and combined these findings with neighborhood financial health data to explore whether school energy intensity corresponds with socioeconomic conditions. These analyses give insight into how wealth, enrollment, and building efficiency combine to produce uneven energy and carbon outcomes across the city.

Methods

All datasets were obtained from publicly available NYC sources:

- **School Energy Data (NYC DOE sustainability reporting)**: monthly mmBTU usage by school and by energy type.

Enrollment Data (NYSED public district enrollment): total PreK–12 student counts by borough.

Neighborhood Financial Health Data: median household income and demographic indicators.

Data were cleaned in R using tidyverse. Major steps were:

Variable assignment, cleaning, and mutates

Converting DOE energy tables to numeric formats and computing total annual mmBTU use for each school.

Mapping borough codes (1–5) to borough names.

Aggregating to borough-level totals.

Normalizing usage by creating energy-per-student metrics.

Estimating carbon emissions using approximate DOE conversion factors.

Aggregating median household income by borough using the financial-health dataset.

Graphics were produced using ggplot2 with a colorblind-safe viridis palette.

Analysis
Total School Energy Usage (Plot 1)

Manhattan shows the highest total energy consumption, followed by the Bronx and Brooklyn. Queens and Staten Island consume substantially less. Since this plot reflects raw mmBTU totals, boroughs with more schools and larger buildings dominate the upper range. This establishes a baseline for understanding how physical size and infrastructure drive energy use independent of student population.

Energy Per Student (Plot 2)

After normalizing by enrollment, Manhattan still leads, but the ordering of the middle boroughs reverses:

Queens falls to the lowest per-student usage.

The Bronx rises into the upper tier.

This Bronx–Queens inversion is consistent with differences in building age, efficiency, and maintenance backlog, which are documented in several NYC infrastructure audits. Queens’ school buildings tend to be newer or better insulated, while many Bronx facilities are older, steam-heated, and less efficient relative to the number of students they serve.

This supports our major finding: energy intensity is not determined solely by total demand, but by the interaction of resources, building conditions, and enrollment.

Average Monthly Usage (Box Plot, Plot 3)

Monthly distributions further illustrate these differences. Manhattan and the Bronx show wide variance, indicating inconsistent or inefficient energy patterns across their schools. Queens shows the tightest distribution, reinforcing its relative efficiency. This aligns with the per-student analysis: boroughs with newer, standardized buildings show more stable monthly usage.

Wealth Per Capita (Plot 4)

Median household income increases from the Bronx → Brooklyn → Queens → Staten Island → Manhattan. When combined with Plot 2, a pattern emerges:
Wealthier boroughs generally have lower energy use per student, despite higher total usage.
This suggests better-maintained buildings, upgraded heating systems, and more consistent access to capital improvements.

Estimated Carbon Emissions (Plot 5)

Estimated emissions track closely with total usage rather than per-student usage. This reinforces that Manhattan’s large building stock—not necessarily inefficiency—drives its carbon footprint. The borough ranking matches Plot 1 almost exactly, confirming strong alignment between mmBTU consumption and CO₂ output when fuel mixes are similar.

Taken together, Plots 1 & 5 show the scale of borough emissions, while Plots 2 & 4 reveal efficiency and equity questions.

Conclusions

Our findings show that:

Total energy use is dominated by borough size and number of schools.

Energy-per-student metrics reveal hidden inequities, with the Bronx using far more energy per student than Queens despite similar total usage.

These differences align with long-documented disparities in school building age, maintenance investment, and renovation cycles.

Wealthier boroughs tend to have both more efficient buildings and more stable monthly usage patterns.

Carbon emissions follow raw energy totals closely, meaning boroughs with the highest usage—not necessarily those with the worst efficiency—produce the most emissions. However, the box plot gave us some insight: lower-income boroughs face greater instability and inefficiency in energy use, while wealthier boroughs have more stable, efficient systems that deliver more energy per student.

Limitations

We cannot separate fuel types with precision, limiting the accuracy of carbon estimates.

School-level infrastructure quality data were not available for direct inclusion.

Wealth was represented by median household income, which is a proxy, not a school-specific measure.

Future Directions

Incorporate building age and renovation data (DOE’s Building Condition Survey).

Break down energy by fuel source for more accurate carbon modeling.

Map school-level variation using geospatial methods.

## Handout

Our handout can be found [here](handout/handout.pdf).

## Memo

A link to the code and how we created our graphics in our memo can be found [here](memo/memo.html).

## Data

New York City Department of Education & School Construction Authority. 2008–2012. NYC School Building Energy Usage (mmBTUs) Dataset. NYC DOE Division of School Facilities. https://data.cityofnewyork.us
. Accessed 5 December 2025.

New York State Education Department. 2024. Public School Enrollment Data, 2024–2025. NYSED Information and Reporting Services. https://data.nysed.gov
. Accessed 5 December 2025.

New York City Mayor’s Office for Economic Opportunity. 2022. Neighborhood Financial Health Digital Mapping and Data Tool. NYC Opportunity. https://data.cityofnewyork.us
. Accessed 5 December 2025.

## References

New York City School Construction Authority. Building Condition Assessment Survey (BCAS), 2023.
New York City Department of Education & School Construction Authority. 2008–2012.
New York State Education Department. Public School Enrollment Data, 2024–25.
New York City Comptroller. State of School Infrastructure Report, 2021.
U.S. EPA. Emission Factors for Greenhouse Gas Inventories, 2023.
