Project proposal
================
Team name

``` r
library(tidyverse)
library(broom)
library(skimr)
library(jsonlite)
```

## 1. Introduction

Our general research question is: How do energy usage trends in New York
City public schools vary on a per-student basis, and what geographic and
socioeconomic factors are associated with those variations?

The primary motivation is to identify energy inefficiency hotspots and
to explore potential environmental equity issues. We will also estimate
carbon costs based on the type and amount of energy used by schools and
compare these across neighborhoods of differing financial health.

## 2. Data

We placed our raw data files in the /data folder and created a README in
that folder that includes variable definitions and data dimensions.

``` r
library(jsonlite)
library(dplyr)
library(skimr)

data <- fromJSON("../data/dataset.json") %>%
  as_tibble()
glimpse(data)
```

    ## Rows: 153
    ## Columns: 60
    ## $ building_address <chr> "512 West 212th St [Shorac Kappock School]\n", "512 W…
    ## $ borough          <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    ## $ measurement      <chr> "Electricity Demand (KW)", "Electricity Usage (KWH)",…
    ## $ `_1`             <chr> "144.00", "30400.00", "109.00", "114.65", "123.20", "…
    ## $ `_2`             <chr> "68.00", "31000.00", "112.00", "117.00", "150.40", "5…
    ## $ fy_2009_7_1_2008 <chr> "148.00", "46200.00", "305.00", "188.18", "150.40", "…
    ## $ aug_08           <chr> "158.00", "45200.00", "2298.00", "384.07", "124.80", …
    ## $ sep_08           <chr> "166.00", "51800.00", "7666.00", "943.39", "136.00", …
    ## $ oct_08           <chr> "180.00", "58400.00", "13710.00", "1570.32", "144.00"…
    ## $ nov_08           <chr> "168.00", "56600.00", "16197.00", "1812.87", "144.00"…
    ## $ dec_08           <chr> "166.00", "52200.00", "12846.00", "1462.76", "144.00"…
    ## $ jan_09           <chr> "166.00", "55200.00", "10322.00", "1220.60", "142.40"…
    ## $ feb_09           <chr> "162.00", "44200.00", "4660.00", "616.85", "142.40", …
    ## $ mar_09           <chr> "142.00", "48600.00", "1138.00", "279.67", "145.60", …
    ## $ apr_09           <chr> "146.00", "47400.00", "348.00", "196.57", "140.80", "…
    ## $ may_09           <chr> "146.00", "34200.00", "150.00", "131.72", "128.00", "…
    ## $ jun_09           <chr> "90.00", "30600.00", "112.00", "115.64", "134.40", "5…
    ## $ fy_2010_7_1_2009 <chr> "148.00", "44800.00", "243.00", "177.20", "148.80", "…
    ## $ aug_09           <chr> "152.00", "52400.00", "2835.00", "462.34", "128.00", …
    ## $ sep_09           <chr> "170.00", "54200.00", "5873.00", "772.28", "128.00", …
    ## $ oct_09           <chr> "178.00", "63200.00", "14903.00", "1706.00", "128.00"…
    ## $ nov_09           <chr> "184.00", "60800.00", "15662.00", "1773.71", "126.40"…
    ## $ dec_09           <chr> "190.00", "63200.00", "16322.00", "1847.90", "126.40"…
    ## $ jan_10           <chr> "188.00", "58000.00", "8197.00", "1017.65", "124.80",…
    ## $ feb_10           <chr> "172.00", "48200.00", "2966.00", "461.11", "121.60", …
    ## $ mar_10           <chr> "158.00", "50800.00", "1298.00", "303.18", "140.80", …
    ## $ apr_10           <chr> "154.00", "52200.00", "275.00", "205.66", "160.00", "…
    ## $ may_10           <chr> "136.00", "33000.00", "106.00", "123.23", "153.60", "…
    ## $ jun_10           <chr> "64.00", "28600.00", "93.00", "106.91", "139.20", "50…
    ## $ fy_2011_7_1_2010 <chr> "140.00", "42600.00", "193.00", "164.69", "156.80", "…
    ## $ aug_10           <chr> "156.00", "52800.00", "1645.00", "344.70", "148.80", …
    ## $ sep_10           <chr> "166.00", "53800.00", "6448.00", "828.42", "134.40", …
    ## $ oct_10           <chr> "178.00", "65200.00", "16825.00", "1905.03", "134.40"…
    ## $ nov_10           <chr> "178.00", "55600.00", "16787.00", "1868.46", "134.40"…
    ## $ dec_10           <chr> "178.00", "59600.00", "14045.00", "1607.91", "134.40"…
    ## $ jan_11           <chr> "172.00", "58000.00", "10629.00", "1260.85", "129.60"…
    ## $ feb_11           <chr> "164.00", "48200.00", "5373.00", "701.81", "131.20", …
    ## $ mar_11           <chr> "154.00", "49600.00", "831.00", "252.38", "124.80", "…
    ## $ apr_11           <chr> "170.00", "52000.00", "339.00", "211.37", "182.40", "…
    ## $ may_11           <chr> "156.00", "31400.00", "124.00", "119.57", "171.20", "…
    ## $ jun_11           <chr> "82.00", "20600.00", "115.00", "81.81", "145.60", "54…
    ## $ fy_2012_7_1_2011 <chr> "58.00", "9800.00", "273.00", "60.75", "174.40", "545…
    ## $ aug_11           <chr> "168.00", "91600.00", "778.00", "390.43", "169.60", "…
    ## $ sep_11           <chr> "164.00", "58800.00", "6037.00", "804.38", "139.20", …
    ## $ oct_11           <chr> "162.00", "53400.00", "8103.00", "992.55", "132.80", …
    ## $ nov_11           <chr> "166.00", "57600.00", "13972.00", "1593.79", "132.80"…
    ## $ dec_11           <chr> "160.00", "58600.00", "12298.00", "1429.80", "132.80"…
    ## $ jan_12           <chr> "156.00", "50400.00", "6866.00", "858.61", "128.00", …
    ## $ feb_12           <chr> "146.00", "43800.00", "3781.00", "527.59", "158.40", …
    ## $ mar_12           <chr> "146.00", "47400.00", "1410.00", "302.77", "134.40", …
    ## $ apr_12           <chr> "156.00", "53600.00", "302.00", "213.14", "187.20", "…
    ## $ postcode         <chr> "10034", "10034", "10034", "10034", "10029", "10029",…
    ## $ latitude         <chr> "40.867826", "40.867826", "40.867826", "40.867826", "…
    ## $ longitude        <chr> "-73.917059", "-73.917059", "-73.917059", "-73.917059…
    ## $ community_board  <chr> "12", "12", "12", "12", "11", "11", "11", "11", "12",…
    ## $ council_district <chr> "10", "10", "10", "10", "8", "8", "8", "8", "10", "10…
    ## $ census_tract     <chr> "303", "303", "303", "303", "164", "164", "164", "164…
    ## $ bin              <chr> "1064871", "1064871", "1064871", "1064871", "1052384"…
    ## $ bbl              <chr> "1022290012", "1022290012", "1022290012", "1022290012…
    ## $ nta              <chr> "Marble Hill-Inwood                                  …

``` r
skim(data)
```

|                                                  |      |
|:-------------------------------------------------|:-----|
| Name                                             | data |
| Number of rows                                   | 153  |
| Number of columns                                | 60   |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |      |
| Column type frequency:                           |      |
| character                                        | 60   |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |      |
| Group variables                                  | None |

Data summary

**Variable type: character**

| skim_variable    | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:-----------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| building_address |         0 |          1.00 |  28 |  62 |     0 |       36 |          0 |
| borough          |         0 |          1.00 |   1 |   1 |     0 |        5 |          0 |
| measurement      |         0 |          1.00 |  12 |  23 |     0 |        7 |          0 |
| \_1              |         0 |          1.00 |   4 |   9 |     0 |      137 |          0 |
| \_2              |         0 |          1.00 |   4 |   9 |     0 |      142 |          0 |
| fy_2009_7_1_2008 |         0 |          1.00 |   4 |   9 |     0 |      141 |          0 |
| aug_08           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| sep_08           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| oct_08           |         0 |          1.00 |   4 |   9 |     0 |      145 |          0 |
| nov_08           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| dec_08           |         0 |          1.00 |   4 |   9 |     0 |      145 |          0 |
| jan_09           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| feb_09           |         0 |          1.00 |   4 |   9 |     0 |      144 |          0 |
| mar_09           |         0 |          1.00 |   4 |   9 |     0 |      143 |          0 |
| apr_09           |         0 |          1.00 |   4 |   9 |     0 |      141 |          0 |
| may_09           |         0 |          1.00 |   4 |   9 |     0 |      141 |          0 |
| jun_09           |         0 |          1.00 |   4 |   9 |     0 |      141 |          0 |
| fy_2010_7_1_2009 |         0 |          1.00 |   4 |   9 |     0 |      140 |          0 |
| aug_09           |         0 |          1.00 |   4 |   9 |     0 |      142 |          0 |
| sep_09           |         0 |          1.00 |   4 |   9 |     0 |      143 |          0 |
| oct_09           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| nov_09           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| dec_09           |         0 |          1.00 |   4 |   9 |     0 |      145 |          0 |
| jan_10           |         0 |          1.00 |   4 |   9 |     0 |      146 |          0 |
| feb_10           |         0 |          1.00 |   4 |   9 |     0 |      148 |          0 |
| mar_10           |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| apr_10           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| may_10           |         0 |          1.00 |   4 |   9 |     0 |      146 |          0 |
| jun_10           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| fy_2011_7_1_2010 |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| aug_10           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| sep_10           |         0 |          1.00 |   4 |   9 |     0 |      151 |          0 |
| oct_10           |         0 |          1.00 |   4 |   9 |     0 |      153 |          0 |
| nov_10           |         0 |          1.00 |   4 |   9 |     0 |      152 |          0 |
| dec_10           |         0 |          1.00 |   4 |   9 |     0 |      152 |          0 |
| jan_11           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| feb_11           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| mar_11           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| apr_11           |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| may_11           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| jun_11           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| fy_2012_7_1_2011 |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| aug_11           |         0 |          1.00 |   4 |   9 |     0 |      148 |          0 |
| sep_11           |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| oct_11           |         0 |          1.00 |   4 |   9 |     0 |      149 |          0 |
| nov_11           |         0 |          1.00 |   4 |   9 |     0 |      151 |          0 |
| dec_11           |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| jan_12           |         0 |          1.00 |   4 |   9 |     0 |      147 |          0 |
| feb_12           |         0 |          1.00 |   4 |   9 |     0 |      151 |          0 |
| mar_12           |         0 |          1.00 |   4 |   9 |     0 |      150 |          0 |
| apr_12           |         0 |          1.00 |   4 |   9 |     0 |      145 |          0 |
| postcode         |         0 |          1.00 |   5 |   5 |     0 |       31 |          0 |
| latitude         |         0 |          1.00 |   7 |   9 |     0 |       36 |          0 |
| longitude        |         0 |          1.00 |   9 |  10 |     0 |       36 |          0 |
| community_board  |         0 |          1.00 |   1 |   2 |     0 |       17 |          0 |
| council_district |         0 |          1.00 |   1 |   2 |     0 |       26 |          0 |
| census_tract     |         0 |          1.00 |   1 |   5 |     0 |       35 |          0 |
| bin              |         9 |          0.94 |   7 |   7 |     0 |       34 |          0 |
| bbl              |         9 |          0.94 |  10 |  10 |     0 |       34 |          0 |
| nta              |         0 |          1.00 |  75 |  75 |     0 |       32 |          0 |

``` r
library(readr)
library(dplyr)
library(skimr)

wealth <- read_csv("../data/Neighborhood_Financial_Health_Digital_Mapping_and_Data_Tool.csv")
```

    ## Rows: 385 Columns: 52
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (26): Borough, Neighborhoods, CD, Goal, GoalName, GoalFullName, GoalRank...
    ## dbl (26): Year Published, PUMA, Join, NYC_Poverty_Rate, Median_Income, Perc_...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(wealth)
```

    ## Rows: 385
    ## Columns: 52
    ## $ `Year Published` <dbl> 2020, 2020, 2020, 2020, 2020, 2020, 2020, 2020, 2020,…
    ## $ PUMA             <dbl> 3701, 3701, 3701, 3701, 3701, 3701, 3701, 3702, 3706,…
    ## $ Borough          <chr> "Bronx", "Bronx", "Bronx", "Bronx", "Bronx", "Bronx",…
    ## $ Neighborhoods    <chr> "Riverdale, Fieldston & Kingsbridge", "Riverdale, Fie…
    ## $ CD               <chr> "BX Community District 8", "BX Community District 8",…
    ## $ Join             <dbl> 3701, 3701, 3701, 3701, 3701, 3701, 3701, 3702, 3706,…
    ## $ NYC_Poverty_Rate <dbl> 0.152, 0.152, 0.152, 0.152, 0.152, 0.152, 0.152, 0.23…
    ## $ Median_Income    <dbl> 30437, 30437, 30437, 30437, 30437, 30437, 30437, 2613…
    ## $ Perc_White       <dbl> 0.324, 0.324, 0.324, 0.324, 0.324, 0.324, 0.324, 0.06…
    ## $ Perc_Black       <dbl> 0.124, 0.124, 0.124, 0.124, 0.124, 0.124, 0.124, 0.65…
    ## $ Perc_Asian       <dbl> 0.052, 0.052, 0.052, 0.052, 0.052, 0.052, 0.052, 0.02…
    ## $ Perc_Other       <dbl> 0.023, 0.023, 0.023, 0.023, 0.023, 0.023, 0.023, 0.02…
    ## $ Perc_Hispanic    <dbl> 0.477, 0.477, 0.477, 0.477, 0.477, 0.477, 0.477, 0.22…
    ## $ Goal             <chr> "Build Assets", "Financial Services", "Financial Shoc…
    ## $ GoalName         <chr> "Goal 5: Build Assets", "Goal 1: Financial Services",…
    ## $ GoalFullName     <chr> "Goal 5: Opportunities to Build Assets & Plan for the…
    ## $ TotalOutcome     <dbl> -0.4667782, -3.5604221, 7.9116966, 0.3347182, 1.78464…
    ## $ GoalRank         <chr> "29", "40", "10", "19", "20", "18", "No Majority", "3…
    ## $ IndexScore       <dbl> 5.1847204, 1.9190357, 7.4913156, 4.5850109, 3.9708558…
    ## $ ScoreRank        <chr> "Middle", "Bottom", "Top", "Middle", "Middle", "Top",…
    ## $ Ind1             <chr> "Homeownership Opportunity", "Bank / Credit Union-To-…
    ## $ Ind1Outcome      <chr> "0.316", "0.125694444", "0.64", "206.27", "0", NA, NA…
    ## $ Ind1Rank         <dbl> 27, 24, 21, 14, 37, 40, NA, 17, 47, 40, 44, 40, 37, 4…
    ## $ Ind1Definition   <chr> "Percentage of housing units that are owner-occupied"…
    ## $ Ind2             <chr> "Neighborhood Tenure", "Bank / Credit Union Density",…
    ## $ Ind2Outcome      <dbl> 0.6580000, 1.6391950, 0.9080000, 0.7050400, 0.5219789…
    ## $ Ind2Rank         <dbl> 31, 27, 3, 15, 17, 19, NA, 18, 48, 52, 31, 39, 23, 37…
    ## $ Ind2Definition   <chr> "Percentage of residents who have lived in their home…
    ## $ Ind3             <chr> "Minority And Women-Owned Business Enterprise", "IDNY…
    ## $ Ind3Outcome      <dbl> 23.67726, 0.00000, 0.90700, 0.12000, 0.63400, NA, NA,…
    ## $ Ind3Rank         <dbl> 45, 28, 20, 38, 29, 20, NA, 42, 37, 28, 30, 24, 27, 2…
    ## $ Ind3Definition   <chr> "Number of NYC certified M/WBEs per 100,000 residents…
    ## $ Ind4             <chr> "Retirement Security", "Mortgage Origination By Race"…
    ## $ Ind4Outcome      <chr> "3.59:1", "Asian 6.23%; Black or African American (no…
    ## $ Ind4Rank         <dbl> 12, 34, 22, 42, 19, 10, NA, 42, 41, 10, 46, 16, 26, 4…
    ## $ Ind4Definition   <chr> "Median income-to-poverty line ratio of residents age…
    ## $ Ind5             <chr> "Community Efficacy", "Bank / Credit Union Utilizatio…
    ## $ Ind5Outcome      <dbl> 24.1325927, 0.8890000, 0.2850000, 0.1373393, 0.776000…
    ## $ Ind5Rank         <dbl> 31, 29, 18, 38, 19, 29, NA, 50, 53, 39, 41, 15, 34, 3…
    ## $ Ind5Definition   <chr> "Number of community and public institutions per 10,0…
    ## $ Ind6             <chr> "Small Business Employment Opportunity", "Affordable …
    ## $ Ind6Outcome      <dbl> 0.2350942, 0.3888889, 37.0000000, 44.5000000, 0.72400…
    ## $ Ind6Rank         <dbl> 32, 38, 27, 41, 28, NA, NA, 24, NA, 31, 53, 46, 10, N…
    ## $ Ind6Definition   <chr> "Percentage of jobs in small businesses (less than 20…
    ## $ Ind7             <chr> "Participatory Budgeting", "Mobile Banking Utilizatio…
    ## $ Ind7Outcome      <dbl> 0.071776769, 0.205800000, 0.000000000, NA, 0.96600000…
    ## $ Ind7Rank         <dbl> 26, 23, 40, NA, 9, NA, NA, 41, NA, 44, 40, NA, 16, NA…
    ## $ Ind7Definition   <chr> "Percentage of eligible residents casting a vote in m…
    ## $ Ind8             <chr> NA, NA, NA, NA, "Adults With High School Diploma", NA…
    ## $ Ind8Outcome      <dbl> NA, NA, NA, NA, 0.8450324, NA, NA, NA, NA, NA, NA, NA…
    ## $ Ind8Rank         <dbl> NA, NA, NA, NA, 21, NA, NA, NA, NA, NA, NA, NA, 31, N…
    ## $ Ind8Definition   <chr> NA, NA, NA, NA, "Percentage of adults with a high sch…

``` r
skim(wealth)
```

|                                                  |        |
|:-------------------------------------------------|:-------|
| Name                                             | wealth |
| Number of rows                                   | 385    |
| Number of columns                                | 52     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |        |
| Column type frequency:                           |        |
| character                                        | 26     |
| numeric                                          | 26     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |        |
| Group variables                                  | None   |

Data summary

**Variable type: character**

| skim_variable  | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:---------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Borough        |         0 |          1.00 |   5 |  13 |     0 |        5 |          0 |
| Neighborhoods  |         0 |          1.00 |   8 |  48 |     0 |       55 |          0 |
| CD             |         0 |          1.00 |  23 |  28 |     0 |       55 |          0 |
| Goal           |         0 |          1.00 |  12 |  23 |     0 |        7 |          0 |
| GoalName       |         0 |          1.00 |  20 |  37 |     0 |        7 |          0 |
| GoalFullName   |         0 |          1.00 |  26 |  61 |     0 |        7 |          0 |
| GoalRank       |         0 |          1.00 |   1 |  11 |     0 |       60 |          0 |
| ScoreRank      |        55 |          0.86 |   3 |   6 |     0 |        3 |          0 |
| Ind1           |        55 |          0.86 |  19 |  61 |     0 |        6 |          0 |
| Ind1Outcome    |       110 |          0.71 |   1 |  11 |     0 |      212 |          0 |
| Ind1Definition |       110 |          0.71 |  44 |  77 |     0 |        5 |          0 |
| Ind2           |        55 |          0.86 |  17 |  59 |     0 |        6 |          0 |
| Ind2Definition |       110 |          0.71 |  61 | 127 |     0 |        5 |          0 |
| Ind3           |        55 |          0.86 |  16 |  48 |     0 |        6 |          0 |
| Ind3Definition |       110 |          0.71 |  45 |  96 |     0 |        5 |          0 |
| Ind4           |        55 |          0.86 |  11 |  59 |     0 |        6 |          0 |
| Ind4Outcome    |       110 |          0.71 |   4 | 166 |     0 |      249 |          0 |
| Ind4Definition |       110 |          0.71 |  63 |  76 |     0 |        5 |          0 |
| Ind5           |        55 |          0.86 |  18 |  59 |     0 |        6 |          0 |
| Ind5Definition |       110 |          0.71 |  59 |  72 |     0 |        5 |          0 |
| Ind6           |       110 |          0.71 |  19 |  37 |     0 |        5 |          0 |
| Ind6Definition |       110 |          0.71 |  45 | 113 |     0 |        5 |          0 |
| Ind7           |       165 |          0.57 |  20 |  26 |     0 |        4 |          0 |
| Ind7Definition |       165 |          0.57 |  46 |  92 |     0 |        4 |          0 |
| Ind8           |       330 |          0.14 |  31 |  31 |     0 |        1 |          0 |
| Ind8Definition |       330 |          0.14 |  60 |  60 |     0 |        1 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate | mean | sd | p0 | p25 | p50 | p75 | p100 | hist |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|:---|
| Year Published | 0 | 1.00 | 2020.00 | 0.00 | 2020.00 | 2020.00 | 2020.00 | 2020.00 | 2020.00 | ▁▁▇▁▁ |
| PUMA | 0 | 1.00 | 3936.22 | 148.82 | 3701.00 | 3804.00 | 4005.00 | 4101.00 | 4114.00 | ▅▅▁▇▆ |
| Join | 0 | 1.00 | 3936.22 | 148.82 | 3701.00 | 3804.00 | 4005.00 | 4101.00 | 4114.00 | ▅▅▁▇▆ |
| NYC_Poverty_Rate | 0 | 1.00 | 0.20 | 0.06 | 0.07 | 0.15 | 0.20 | 0.25 | 0.34 | ▃▃▇▃▂ |
| Median_Income | 0 | 1.00 | 31237.60 | 14811.27 | 14213.00 | 22264.00 | 26140.00 | 32210.00 | 79181.00 | ▇▅▁▁▁ |
| Perc_White | 0 | 1.00 | 0.32 | 0.24 | 0.01 | 0.12 | 0.28 | 0.53 | 0.84 | ▇▇▃▃▃ |
| Perc_Black | 0 | 1.00 | 0.23 | 0.23 | 0.01 | 0.04 | 0.12 | 0.31 | 0.87 | ▇▃▁▂▁ |
| Perc_Asian | 0 | 1.00 | 0.13 | 0.12 | 0.01 | 0.04 | 0.08 | 0.16 | 0.52 | ▇▃▁▁▁ |
| Perc_Other | 0 | 1.00 | 0.03 | 0.02 | 0.01 | 0.02 | 0.02 | 0.03 | 0.17 | ▇▁▁▁▁ |
| Perc_Hispanic | 0 | 1.00 | 0.29 | 0.20 | 0.07 | 0.14 | 0.23 | 0.42 | 0.70 | ▇▃▂▁▃ |
| TotalOutcome | 18 | 0.95 | 0.06 | 12.77 | -46.42 | -5.32 | 0.09 | 3.53 | 85.96 | ▁▇▂▁▁ |
| IndexScore | 55 | 0.86 | 4.07 | 2.47 | 0.00 | 2.30 | 3.76 | 5.53 | 10.00 | ▃▇▅▂▂ |
| Ind1Rank | 55 | 0.86 | 27.38 | 15.35 | 1.00 | 14.00 | 28.00 | 39.00 | 55.00 | ▇▇▇▇▆ |
| Ind2Outcome | 110 | 0.71 | 0.90 | 1.30 | 0.02 | 0.51 | 0.62 | 0.76 | 17.35 | ▇▁▁▁▁ |
| Ind2Rank | 55 | 0.86 | 27.99 | 15.90 | 1.00 | 14.00 | 28.00 | 42.00 | 55.00 | ▇▇▇▇▇ |
| Ind3Outcome | 111 | 0.71 | 10.74 | 30.73 | 0.00 | 0.09 | 0.64 | 0.92 | 301.39 | ▇▁▁▁▁ |
| Ind3Rank | 56 | 0.85 | 26.38 | 15.18 | 1.00 | 14.00 | 27.00 | 38.00 | 55.00 | ▆▆▇▅▅ |
| Ind4Rank | 55 | 0.86 | 27.89 | 15.91 | 1.00 | 14.00 | 28.00 | 42.00 | 55.00 | ▇▇▇▇▇ |
| Ind5Outcome | 110 | 0.71 | 9.01 | 29.46 | 0.10 | 0.27 | 0.75 | 0.94 | 300.52 | ▇▁▁▁▁ |
| Ind5Rank | 55 | 0.86 | 27.95 | 15.90 | 1.00 | 14.00 | 28.00 | 42.00 | 55.00 | ▇▇▇▇▇ |
| Ind6Outcome | 110 | 0.71 | 17.48 | 24.15 | 0.09 | 0.38 | 0.74 | 37.60 | 99.60 | ▇▂▂▁▁ |
| Ind6Rank | 110 | 0.71 | 27.89 | 15.88 | 1.00 | 14.00 | 28.00 | 42.00 | 55.00 | ▇▇▇▇▇ |
| Ind7Outcome | 178 | 0.54 | 0.83 | 1.50 | 0.00 | 0.13 | 0.27 | 0.93 | 10.07 | ▇▁▁▁▁ |
| Ind7Rank | 165 | 0.57 | 27.06 | 14.83 | 1.00 | 14.00 | 28.00 | 40.00 | 55.00 | ▅▆▅▇▂ |
| Ind8Outcome | 330 | 0.14 | 0.81 | 0.09 | 0.60 | 0.76 | 0.83 | 0.86 | 0.97 | ▂▂▆▇▃ |
| Ind8Rank | 330 | 0.14 | 28.00 | 16.02 | 1.00 | 14.50 | 28.00 | 41.50 | 55.00 | ▇▇▇▇▇ |

``` r
library(readr)
library(dplyr)
library(skimr)

enrollment <- read_csv("../data/Enrollment.csv")
```

    ## Rows: 16598 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): ENTITY_CD, ENTITY_NAME
    ## dbl (21): YEAR, PK, PKHALF, PKFULL, KHALF, KFULL, 1, 2, 3, 4, 5, 6, 7, 8, 9,...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(enrollment)
```

    ## Rows: 16,598
    ## Columns: 23
    ## $ ENTITY_CD   <chr> "000000000001", "000000000002", "000000000003", "000000000…
    ## $ ENTITY_NAME <chr> "NYC Public Schools", "Large Cities", "High Need/Resource …
    ## $ YEAR        <dbl> 2023, 2023, 2023, 2023, 2024, 2024, 2024, 2024, 2023, 2023…
    ## $ PK          <dbl> 97253, 7815, 12273, 7239, 103784, 7636, 12549, 7412, 24786…
    ## $ PKHALF      <dbl> 860, 781, 3182, 1500, 669, 544, 1807, 1426, 6956, 1164, 0,…
    ## $ PKFULL      <dbl> 96393, 7034, 9091, 5739, 103115, 7092, 10742, 5986, 17830,…
    ## $ KHALF       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 526, 0, 0, 0, 535, 0, 0, 0, 0, 0, …
    ## $ KFULL       <dbl> 53911, 6401, 13935, 9966, 51544, 6081, 13569, 9505, 47931,…
    ## $ `1`         <dbl> 56221, 6389, 14334, 10156, 56088, 6420, 14316, 9832, 50681…
    ## $ `2`         <dbl> 55450, 6345, 13761, 9676, 56844, 6298, 14451, 10054, 49650…
    ## $ `3`         <dbl> 55644, 6524, 14115, 9934, 56187, 6238, 13651, 9595, 50881,…
    ## $ `4`         <dbl> 56594, 6710, 14050, 9791, 56869, 6418, 14173, 9935, 51038,…
    ## $ `5`         <dbl> 58709, 6556, 14458, 9993, 57837, 6536, 14003, 9856, 51760,…
    ## $ `6`         <dbl> 56703, 6513, 14151, 10151, 57181, 6474, 14341, 10100, 5286…
    ## $ `7`         <dbl> 58294, 6709, 14512, 10201, 58017, 6400, 14175, 10257, 5281…
    ## $ `8`         <dbl> 61019, 7044, 15042, 10472, 59976, 6691, 14539, 10170, 5385…
    ## $ `9`         <dbl> 71724, 8052, 16605, 11326, 73324, 8043, 16546, 10921, 5637…
    ## $ `10`        <dbl> 70858, 7316, 16244, 10424, 71091, 7252, 16009, 10551, 5624…
    ## $ `11`        <dbl> 63301, 6360, 14370, 9811, 64413, 6574, 15029, 9794, 54395,…
    ## $ `12`        <dbl> 61348, 6430, 14181, 9571, 60613, 6425, 15079, 9765, 54900,…
    ## $ UGE         <dbl> 7785, 474, 847, 236, 4591, 430, 784, 204, 1698, 934, 127, …
    ## $ UGS         <dbl> 10283, 899, 1294, 695, 10051, 866, 1274, 684, 3524, 1578, …
    ## $ K12         <dbl> 797844, 88722, 191899, 132403, 794626, 87146, 191939, 1312…

``` r
skim(enrollment)
```

|                                                  |            |
|:-------------------------------------------------|:-----------|
| Name                                             | enrollment |
| Number of rows                                   | 16598      |
| Number of columns                                | 23         |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |            |
| Column type frequency:                           |            |
| character                                        | 2          |
| numeric                                          | 21         |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |            |
| Group variables                                  | None       |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| ENTITY_CD     |         0 |             1 |  12 |  12 |     0 |     5579 |          0 |
| ENTITY_NAME   |         0 |             1 |   4 |  53 |     0 |     5653 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate | mean | sd | p0 | p25 | p50 | p75 | p100 | hist |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|:---|
| YEAR | 0 | 1 | 2023.00 | 0.82 | 2022 | 2022 | 2023 | 2024.00 | 2024 | ▇▁▇▁▇ |
| PK | 0 | 1 | 141.48 | 2912.35 | 0 | 0 | 0 | 34.00 | 167140 | ▇▁▁▁▁ |
| PKHALF | 0 | 1 | 11.96 | 239.69 | 0 | 0 | 0 | 0.00 | 19584 | ▇▁▁▁▁ |
| PKFULL | 0 | 1 | 129.52 | 2748.94 | 0 | 0 | 0 | 29.00 | 157128 | ▇▁▁▁▁ |
| KHALF | 0 | 1 | 0.47 | 14.31 | 0 | 0 | 0 | 0.00 | 535 | ▇▁▁▁▁ |
| KFULL | 0 | 1 | 162.86 | 2733.30 | 0 | 0 | 35 | 73.00 | 173243 | ▇▁▁▁▁ |
| 1 | 0 | 1 | 168.92 | 2838.53 | 0 | 0 | 38 | 75.00 | 178037 | ▇▁▁▁▁ |
| 2 | 0 | 1 | 170.53 | 2864.40 | 0 | 0 | 38 | 77.00 | 180097 | ▇▁▁▁▁ |
| 3 | 0 | 1 | 170.39 | 2861.79 | 0 | 0 | 37 | 76.00 | 178846 | ▇▁▁▁▁ |
| 4 | 0 | 1 | 172.75 | 2900.13 | 0 | 0 | 36 | 77.00 | 182097 | ▇▁▁▁▁ |
| 5 | 0 | 1 | 174.67 | 2932.68 | 0 | 0 | 34 | 78.00 | 183551 | ▇▁▁▁▁ |
| 6 | 0 | 1 | 176.00 | 2954.28 | 0 | 0 | 0 | 75.00 | 185728 | ▇▁▁▁▁ |
| 7 | 0 | 1 | 179.06 | 3003.01 | 0 | 0 | 0 | 75.00 | 190255 | ▇▁▁▁▁ |
| 8 | 0 | 1 | 182.73 | 3064.35 | 0 | 0 | 0 | 76.00 | 193606 | ▇▁▁▁▁ |
| 9 | 0 | 1 | 197.40 | 3316.82 | 0 | 0 | 0 | 76.00 | 206583 | ▇▁▁▁▁ |
| 10 | 0 | 1 | 191.84 | 3224.08 | 0 | 0 | 0 | 72.75 | 199757 | ▇▁▁▁▁ |
| 11 | 0 | 1 | 180.13 | 3020.26 | 0 | 0 | 0 | 68.00 | 188582 | ▇▁▁▁▁ |
| 12 | 0 | 1 | 177.25 | 2964.51 | 0 | 0 | 0 | 67.00 | 185621 | ▇▁▁▁▁ |
| UGE | 0 | 1 | 11.15 | 234.19 | 0 | 0 | 0 | 1.00 | 12212 | ▇▁▁▁▁ |
| UGS | 0 | 1 | 18.52 | 366.44 | 0 | 0 | 0 | 2.00 | 18816 | ▇▁▁▁▁ |
| K12 | 0 | 1 | 2334.65 | 39158.05 | 0 | 304 | 446 | 720.00 | 2448537 | ▇▁▁▁▁ |

## 3. Data analysis plan

Our analysis will explore patterns of energy use per student across New
York City public schools, linking them with neighborhood financial
health indicators. We will visualize and quantify how energy consumption
and estimated carbon emissions differ by borough and neighborhood
wealth.

**Planned analyses**

Normalization: Compute energy per student for each school.

Carbon estimation: Apply EPA emission factors by energy type
(electricity, gas, steam).

Geographic comparisons: Aggregate by borough and neighborhood.

Socioeconomic correlation: Compare energy intensity with income and
financial-health indicators.

Outlier detection: Identify schools with unusually high per-student
usage.

``` r
# Code goes here
# Code to calculate summary statistics
# Code for a visualization
```
