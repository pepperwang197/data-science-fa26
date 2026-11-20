US Income
================
Pepper Wang
2026-17-11

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [Setup](#setup)
  - [**q1** Load the population data from c06; simply replace
    `filename_pop`
    below.](#q1-load-the-population-data-from-c06-simply-replace-filename_pop-below)
  - [**q2** Obtain median income data from the Census
    Bureau:](#q2-obtain-median-income-data-from-the-census-bureau)
  - [**q3** Tidy the `df_income` dataset by completing the code below.
    Pivot and rename the columns to arrive at the column names
    `id, geographic_area_name, category, income_estimate, income_moe`.](#q3-tidy-the-df_income-dataset-by-completing-the-code-below-pivot-and-rename-the-columns-to-arrive-at-the-column-names-id-geographic_area_name-category-income_estimate-income_moe)
  - [**q4** Convert the margin of error to standard error. Additionally,
    compute a 99% confidence interval on income, and normalize the
    standard error to `income_CV = income_SE / income_estimate`. Provide
    these columns with the names
    `income_SE, income_lo, income_hi, income_CV`.](#q4-convert-the-margin-of-error-to-standard-error-additionally-compute-a-99-confidence-interval-on-income-and-normalize-the-standard-error-to-income_cv--income_se--income_estimate-provide-these-columns-with-the-names-income_se-income_lo-income_hi-income_cv)
  - [**q5** Join `df_q4` and `df_pop`.](#q5-join-df_q4-and-df_pop)
- [Analysis](#analysis)
  - [**q6** Study the following graph, making sure to note what you can
    *and can’t* conclude based on the estimates and confidence
    intervals. Document your observations below and answer the
    questions.](#q6-study-the-following-graph-making-sure-to-note-what-you-can-and-cant-conclude-based-on-the-estimates-and-confidence-intervals-document-your-observations-below-and-answer-the-questions)
  - [**q7** Plot the standard error against population for all counties.
    Create a visual that effectively highlights the trends in the data.
    Answer the questions under *observations*
    below.](#q7-plot-the-standard-error-against-population-for-all-counties-create-a-visual-that-effectively-highlights-the-trends-in-the-data-answer-the-questions-under-observations-below)
- [Going Further](#going-further)
  - [**q8** Pose your own question about the data. Create a
    visualization (or table) here, and document your
    observations.](#q8-pose-your-own-question-about-the-data-create-a-visualization-or-table-here-and-document-your-observations)
- [References](#references)

*Purpose*: We’ve been learning how to quantify uncertainty in estimates
through the exercises; now its time to put those skills to use studying
real data. In this challenge we’ll use concepts like confidence
intervals to help us make sense of census data.

*Reading*: - [Using ACS Estimates and Margin of
Error](https://www.census.gov/data/academy/webinars/2020/calculating-margins-of-error-acs.html)
(Optional, see the PDF on the page) - [Patterns and Causes of
Uncertainty in the American Community
Survey](https://www.sciencedirect.com/science/article/pii/S0143622813002518?casa_token=VddzQ1-spHMAAAAA:FTq92LXgiPVloJUVjnHs8Ma1HwvPigisAYtzfqaGbbRRwoknNq56Y2IzszmGgIGH4JAPzQN0)
(Optional, particularly the *Uncertainty in surveys* section under the
Introduction.)

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

# Setup

<!-- ----------------------------------------------------------------------- -->

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

### **q1** Load the population data from c06; simply replace `filename_pop` below.

``` r
## TODO: Give the filename for your copy of Table B01003
filename_pop <- "data/population_data_2018.csv"

## NOTE: No need to edit
df_pop <-
  read_csv(
    filename_pop,
    skip = 1,
  ) %>% 
  rename(
    population_estimate = `Estimate!!Total`
  )
```

    ## New names:
    ## Rows: 3220 Columns: 5
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (3): Geography, Geographic Area Name, Margin of Error!!Total dbl (1):
    ## Estimate!!Total lgl (1): ...5
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `` -> `...5`

``` r
df_pop
```

    ## # A tibble: 3,220 × 5
    ##    Geography   `Geographic Area Name` population_estimate Margin of Error!!Tot…¹
    ##    <chr>       <chr>                                <dbl> <chr>                 
    ##  1 0500000US0… Autauga County, Alaba…               55200 *****                 
    ##  2 0500000US0… Baldwin County, Alaba…              208107 *****                 
    ##  3 0500000US0… Barbour County, Alaba…               25782 *****                 
    ##  4 0500000US0… Bibb County, Alabama                 22527 *****                 
    ##  5 0500000US0… Blount County, Alabama               57645 *****                 
    ##  6 0500000US0… Bullock County, Alaba…               10352 *****                 
    ##  7 0500000US0… Butler County, Alabama               20025 *****                 
    ##  8 0500000US0… Calhoun County, Alaba…              115098 *****                 
    ##  9 0500000US0… Chambers County, Alab…               33826 *****                 
    ## 10 0500000US0… Cherokee County, Alab…               25853 *****                 
    ## # ℹ 3,210 more rows
    ## # ℹ abbreviated name: ¹​`Margin of Error!!Total`
    ## # ℹ 1 more variable: ...5 <lgl>

You might wonder why the `Margin of Error` in the population estimates
is listed as `*****`. From the [documentation (PDF
link)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwj81Omy16TrAhXsguAKHTzKDQEQFjABegQIBxAB&url=https%3A%2F%2Fwww2.census.gov%2Fprograms-surveys%2Facs%2Ftech_docs%2Faccuracy%2FMultiyearACSAccuracyofData2018.pdf%3F&usg=AOvVaw2TOrVuBDlkDI2gde6ugce_)
for the ACS:

> If the margin of error is displayed as ‘\*\*\*\*\*’ (five asterisks),
> the estimate has been controlled to be equal to a fixed value and so
> it has no sampling error. A standard error of zero should be used for
> these controlled estimates when completing calculations, such as those
> in the following section.

This means that for cases listed as `*****` the US Census Bureau
recommends treating the margin of error (and thus standard error) as
zero.

### **q2** Obtain median income data from the Census Bureau:

- `Filter > Topics > Income and Poverty > Income and Poverty`
- `Filter > Geography > County > All counties in United States`
- Look for `Median Income in the Past 12 Months` (Table S1903)
- Download the 2018 5-year ACS estimates; save to your `data` folder and
  add the filename below.

``` r
## TODO: Give the filename for your copy of Table S1903
filename_income <- "data/income.csv"

## NOTE: No need to edit
df_income <-
  read_csv(filename_income, skip = 1)
```

    ## New names:
    ## • `` -> `...243`

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 3220 Columns: 243
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (66): Geography, Geographic Area Name, Estimate!!Median income (dollars...
    ## dbl (176): Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO...
    ## lgl   (1): ...243
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_income |> names()
```

    ##   [1] "Geography"                                                                                                                                                                              
    ##   [2] "Geographic Area Name"                                                                                                                                                                   
    ##   [3] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households"                                                                                    
    ##   [4] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households"                                                                         
    ##   [5] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!White"                                                                 
    ##   [6] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!White"                                                      
    ##   [7] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Black or African American"                                             
    ##   [8] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Black or African American"                                  
    ##   [9] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!American Indian and Alaska Native"                                     
    ##  [10] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!American Indian and Alaska Native"                          
    ##  [11] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Asian"                                                                 
    ##  [12] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Asian"                                                      
    ##  [13] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Native Hawaiian and Other Pacific Islander"                            
    ##  [14] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Native Hawaiian and Other Pacific Islander"                 
    ##  [15] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Some other race"                                                       
    ##  [16] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Some other race"                                            
    ##  [17] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Two or more races"                                                                 
    ##  [18] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Two or more races"                                                      
    ##  [19] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Hispanic or Latino origin (of any race)"                                           
    ##  [20] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Hispanic or Latino origin (of any race)"                                
    ##  [21] "Estimate!!Number!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!White alone, not Hispanic or Latino"                                               
    ##  [22] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!White alone, not Hispanic or Latino"                                    
    ##  [23] "Estimate!!Number!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!15 to 24 years"                                                                                                               
    ##  [24] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!15 to 24 years"                                                                                                    
    ##  [25] "Estimate!!Number!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!25 to 44 years"                                                                                                               
    ##  [26] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!25 to 44 years"                                                                                                    
    ##  [27] "Estimate!!Number!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!45 to 64 years"                                                                                                               
    ##  [28] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!45 to 64 years"                                                                                                    
    ##  [29] "Estimate!!Number!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!65 years and over"                                                                                                            
    ##  [30] "Margin of Error!!Number MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!65 years and over"                                                                                                 
    ##  [31] "Estimate!!Number!!FAMILIES!!Families"                                                                                                                                                   
    ##  [32] "Margin of Error!!Number MOE!!FAMILIES!!Families"                                                                                                                                        
    ##  [33] "Estimate!!Number!!FAMILIES!!Families!!With own children of householder under 18 years"                                                                                                  
    ##  [34] "Margin of Error!!Number MOE!!FAMILIES!!Families!!With own children of householder under 18 years"                                                                                       
    ##  [35] "Estimate!!Number!!FAMILIES!!Families!!With no own children of householder under 18 years"                                                                                               
    ##  [36] "Margin of Error!!Number MOE!!FAMILIES!!Families!!With no own children of householder under 18 years"                                                                                    
    ##  [37] "Estimate!!Number!!FAMILIES!!Families!!Married-couple families"                                                                                                                          
    ##  [38] "Margin of Error!!Number MOE!!FAMILIES!!Families!!Married-couple families"                                                                                                               
    ##  [39] "Estimate!!Number!!FAMILIES!!Families!!Married-couple families!!With own children under 18 years"                                                                                        
    ##  [40] "Margin of Error!!Number MOE!!FAMILIES!!Families!!Married-couple families!!With own children under 18 years"                                                                             
    ##  [41] "Estimate!!Number!!FAMILIES!!Families!!Female householder, no husband present"                                                                                                           
    ##  [42] "Margin of Error!!Number MOE!!FAMILIES!!Families!!Female householder, no husband present"                                                                                                
    ##  [43] "Estimate!!Number!!FAMILIES!!Families!!Female householder, no husband present!!With own children under 18 years"                                                                         
    ##  [44] "Margin of Error!!Number MOE!!FAMILIES!!Families!!Female householder, no husband present!!With own children under 18 years"                                                              
    ##  [45] "Estimate!!Number!!FAMILIES!!Families!!Male householder, no wife present"                                                                                                                
    ##  [46] "Margin of Error!!Number MOE!!FAMILIES!!Families!!Male householder, no wife present"                                                                                                     
    ##  [47] "Estimate!!Number!!FAMILIES!!Families!!Male householder, no wife present!!With own children under 18 years"                                                                              
    ##  [48] "Margin of Error!!Number MOE!!FAMILIES!!Families!!Male householder, no wife present!!With own children under 18 years"                                                                   
    ##  [49] "Estimate!!Number!!FAMILY INCOME BY FAMILY SIZE!!2-person families"                                                                                                                      
    ##  [50] "Margin of Error!!Number MOE!!FAMILY INCOME BY FAMILY SIZE!!2-person families"                                                                                                           
    ##  [51] "Estimate!!Number!!FAMILY INCOME BY FAMILY SIZE!!3-person families"                                                                                                                      
    ##  [52] "Margin of Error!!Number MOE!!FAMILY INCOME BY FAMILY SIZE!!3-person families"                                                                                                           
    ##  [53] "Estimate!!Number!!FAMILY INCOME BY FAMILY SIZE!!4-person families"                                                                                                                      
    ##  [54] "Margin of Error!!Number MOE!!FAMILY INCOME BY FAMILY SIZE!!4-person families"                                                                                                           
    ##  [55] "Estimate!!Number!!FAMILY INCOME BY FAMILY SIZE!!5-person families"                                                                                                                      
    ##  [56] "Margin of Error!!Number MOE!!FAMILY INCOME BY FAMILY SIZE!!5-person families"                                                                                                           
    ##  [57] "Estimate!!Number!!FAMILY INCOME BY FAMILY SIZE!!6-person families"                                                                                                                      
    ##  [58] "Margin of Error!!Number MOE!!FAMILY INCOME BY FAMILY SIZE!!6-person families"                                                                                                           
    ##  [59] "Estimate!!Number!!FAMILY INCOME BY FAMILY SIZE!!7-or-more person families"                                                                                                              
    ##  [60] "Margin of Error!!Number MOE!!FAMILY INCOME BY FAMILY SIZE!!7-or-more person families"                                                                                                   
    ##  [61] "Estimate!!Number!!FAMILY INCOME BY NUMBER OF EARNERS!!No earners"                                                                                                                       
    ##  [62] "Margin of Error!!Number MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!No earners"                                                                                                            
    ##  [63] "Estimate!!Number!!FAMILY INCOME BY NUMBER OF EARNERS!!1 earner"                                                                                                                         
    ##  [64] "Margin of Error!!Number MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!1 earner"                                                                                                              
    ##  [65] "Estimate!!Number!!FAMILY INCOME BY NUMBER OF EARNERS!!2 earners"                                                                                                                        
    ##  [66] "Margin of Error!!Number MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!2 earners"                                                                                                             
    ##  [67] "Estimate!!Number!!FAMILY INCOME BY NUMBER OF EARNERS!!3 or more earners"                                                                                                                
    ##  [68] "Margin of Error!!Number MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!3 or more earners"                                                                                                     
    ##  [69] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households"                                                                                                                           
    ##  [70] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households"                                                                                                                
    ##  [71] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder"                                                                                                       
    ##  [72] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder"                                                                                            
    ##  [73] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Living alone"                                                                                         
    ##  [74] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Living alone"                                                                              
    ##  [75] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Not living alone"                                                                                     
    ##  [76] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Not living alone"                                                                          
    ##  [77] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder"                                                                                                         
    ##  [78] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder"                                                                                              
    ##  [79] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Living alone"                                                                                           
    ##  [80] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Living alone"                                                                                
    ##  [81] "Estimate!!Number!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Not living alone"                                                                                       
    ##  [82] "Margin of Error!!Number MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Not living alone"                                                                            
    ##  [83] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households"                                                                      
    ##  [84] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households"                                                           
    ##  [85] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households"                                                                   
    ##  [86] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households"                                                        
    ##  [87] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!White"                                                   
    ##  [88] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!White"                                        
    ##  [89] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!White"                                                
    ##  [90] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!White"                                     
    ##  [91] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Black or African American"                               
    ##  [92] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Black or African American"                    
    ##  [93] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Black or African American"                            
    ##  [94] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Black or African American"                 
    ##  [95] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!American Indian and Alaska Native"                       
    ##  [96] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!American Indian and Alaska Native"            
    ##  [97] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!American Indian and Alaska Native"                    
    ##  [98] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!American Indian and Alaska Native"         
    ##  [99] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Asian"                                                   
    ## [100] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Asian"                                        
    ## [101] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Asian"                                                
    ## [102] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Asian"                                     
    ## [103] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Native Hawaiian and Other Pacific Islander"              
    ## [104] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Native Hawaiian and Other Pacific Islander"   
    ## [105] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Native Hawaiian and Other Pacific Islander"           
    ## [106] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Native Hawaiian and Other Pacific Islander"
    ## [107] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Some other race"                                         
    ## [108] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Some other race"                              
    ## [109] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Some other race"                                      
    ## [110] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!One race--!!Some other race"                           
    ## [111] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Two or more races"                                                   
    ## [112] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Two or more races"                                        
    ## [113] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Two or more races"                                                
    ## [114] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Two or more races"                                     
    ## [115] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Hispanic or Latino origin (of any race)"                             
    ## [116] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Hispanic or Latino origin (of any race)"                  
    ## [117] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Hispanic or Latino origin (of any race)"                          
    ## [118] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!Hispanic or Latino origin (of any race)"               
    ## [119] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!White alone, not Hispanic or Latino"                                 
    ## [120] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!White alone, not Hispanic or Latino"                      
    ## [121] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!White alone, not Hispanic or Latino"                              
    ## [122] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY RACE AND HISPANIC OR LATINO ORIGIN OF HOUSEHOLDER!!Households!!White alone, not Hispanic or Latino"                   
    ## [123] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!15 to 24 years"                                                                                                 
    ## [124] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!15 to 24 years"                                                                                      
    ## [125] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!15 to 24 years"                                                                                              
    ## [126] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!15 to 24 years"                                                                                   
    ## [127] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!25 to 44 years"                                                                                                 
    ## [128] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!25 to 44 years"                                                                                      
    ## [129] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!25 to 44 years"                                                                                              
    ## [130] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!25 to 44 years"                                                                                   
    ## [131] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!45 to 64 years"                                                                                                 
    ## [132] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!45 to 64 years"                                                                                      
    ## [133] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!45 to 64 years"                                                                                              
    ## [134] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!45 to 64 years"                                                                                   
    ## [135] "Estimate!!Percent Distribution!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!65 years and over"                                                                                              
    ## [136] "Margin of Error!!Percent Distribution MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!65 years and over"                                                                                   
    ## [137] "Estimate!!Median income (dollars)!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!65 years and over"                                                                                           
    ## [138] "Margin of Error!!Median income (dollars) MOE!!HOUSEHOLD INCOME BY AGE OF HOUSEHOLDER!!65 years and over"                                                                                
    ## [139] "Estimate!!Percent Distribution!!FAMILIES!!Families"                                                                                                                                     
    ## [140] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families"                                                                                                                          
    ## [141] "Estimate!!Median income (dollars)!!FAMILIES!!Families"                                                                                                                                  
    ## [142] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families"                                                                                                                       
    ## [143] "Estimate!!Percent Distribution!!FAMILIES!!Families!!With own children of householder under 18 years"                                                                                    
    ## [144] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!With own children of householder under 18 years"                                                                         
    ## [145] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!With own children of householder under 18 years"                                                                                 
    ## [146] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!With own children of householder under 18 years"                                                                      
    ## [147] "Estimate!!Percent Distribution!!FAMILIES!!Families!!With no own children of householder under 18 years"                                                                                 
    ## [148] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!With no own children of householder under 18 years"                                                                      
    ## [149] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!With no own children of householder under 18 years"                                                                              
    ## [150] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!With no own children of householder under 18 years"                                                                   
    ## [151] "Estimate!!Percent Distribution!!FAMILIES!!Families!!Married-couple families"                                                                                                            
    ## [152] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!Married-couple families"                                                                                                 
    ## [153] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!Married-couple families"                                                                                                         
    ## [154] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!Married-couple families"                                                                                              
    ## [155] "Estimate!!Percent Distribution!!FAMILIES!!Families!!Married-couple families!!With own children under 18 years"                                                                          
    ## [156] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!Married-couple families!!With own children under 18 years"                                                               
    ## [157] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!Married-couple families!!With own children under 18 years"                                                                       
    ## [158] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!Married-couple families!!With own children under 18 years"                                                            
    ## [159] "Estimate!!Percent Distribution!!FAMILIES!!Families!!Female householder, no husband present"                                                                                             
    ## [160] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!Female householder, no husband present"                                                                                  
    ## [161] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!Female householder, no husband present"                                                                                          
    ## [162] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!Female householder, no husband present"                                                                               
    ## [163] "Estimate!!Percent Distribution!!FAMILIES!!Families!!Female householder, no husband present!!With own children under 18 years"                                                           
    ## [164] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!Female householder, no husband present!!With own children under 18 years"                                                
    ## [165] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!Female householder, no husband present!!With own children under 18 years"                                                        
    ## [166] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!Female householder, no husband present!!With own children under 18 years"                                             
    ## [167] "Estimate!!Percent Distribution!!FAMILIES!!Families!!Male householder, no wife present"                                                                                                  
    ## [168] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!Male householder, no wife present"                                                                                       
    ## [169] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!Male householder, no wife present"                                                                                               
    ## [170] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!Male householder, no wife present"                                                                                    
    ## [171] "Estimate!!Percent Distribution!!FAMILIES!!Families!!Male householder, no wife present!!With own children under 18 years"                                                                
    ## [172] "Margin of Error!!Percent Distribution MOE!!FAMILIES!!Families!!Male householder, no wife present!!With own children under 18 years"                                                     
    ## [173] "Estimate!!Median income (dollars)!!FAMILIES!!Families!!Male householder, no wife present!!With own children under 18 years"                                                             
    ## [174] "Margin of Error!!Median income (dollars) MOE!!FAMILIES!!Families!!Male householder, no wife present!!With own children under 18 years"                                                  
    ## [175] "Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!2-person families"                                                                                                        
    ## [176] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY FAMILY SIZE!!2-person families"                                                                                             
    ## [177] "Estimate!!Median income (dollars)!!FAMILY INCOME BY FAMILY SIZE!!2-person families"                                                                                                     
    ## [178] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY FAMILY SIZE!!2-person families"                                                                                          
    ## [179] "Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!3-person families"                                                                                                        
    ## [180] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY FAMILY SIZE!!3-person families"                                                                                             
    ## [181] "Estimate!!Median income (dollars)!!FAMILY INCOME BY FAMILY SIZE!!3-person families"                                                                                                     
    ## [182] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY FAMILY SIZE!!3-person families"                                                                                          
    ## [183] "Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!4-person families"                                                                                                        
    ## [184] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY FAMILY SIZE!!4-person families"                                                                                             
    ## [185] "Estimate!!Median income (dollars)!!FAMILY INCOME BY FAMILY SIZE!!4-person families"                                                                                                     
    ## [186] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY FAMILY SIZE!!4-person families"                                                                                          
    ## [187] "Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!5-person families"                                                                                                        
    ## [188] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY FAMILY SIZE!!5-person families"                                                                                             
    ## [189] "Estimate!!Median income (dollars)!!FAMILY INCOME BY FAMILY SIZE!!5-person families"                                                                                                     
    ## [190] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY FAMILY SIZE!!5-person families"                                                                                          
    ## [191] "Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!6-person families"                                                                                                        
    ## [192] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY FAMILY SIZE!!6-person families"                                                                                             
    ## [193] "Estimate!!Median income (dollars)!!FAMILY INCOME BY FAMILY SIZE!!6-person families"                                                                                                     
    ## [194] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY FAMILY SIZE!!6-person families"                                                                                          
    ## [195] "Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!7-or-more person families"                                                                                                
    ## [196] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY FAMILY SIZE!!7-or-more person families"                                                                                     
    ## [197] "Estimate!!Median income (dollars)!!FAMILY INCOME BY FAMILY SIZE!!7-or-more person families"                                                                                             
    ## [198] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY FAMILY SIZE!!7-or-more person families"                                                                                  
    ## [199] "Estimate!!Percent Distribution!!FAMILY INCOME BY NUMBER OF EARNERS!!No earners"                                                                                                         
    ## [200] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!No earners"                                                                                              
    ## [201] "Estimate!!Median income (dollars)!!FAMILY INCOME BY NUMBER OF EARNERS!!No earners"                                                                                                      
    ## [202] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!No earners"                                                                                           
    ## [203] "Estimate!!Percent Distribution!!FAMILY INCOME BY NUMBER OF EARNERS!!1 earner"                                                                                                           
    ## [204] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!1 earner"                                                                                                
    ## [205] "Estimate!!Median income (dollars)!!FAMILY INCOME BY NUMBER OF EARNERS!!1 earner"                                                                                                        
    ## [206] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!1 earner"                                                                                             
    ## [207] "Estimate!!Percent Distribution!!FAMILY INCOME BY NUMBER OF EARNERS!!2 earners"                                                                                                          
    ## [208] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!2 earners"                                                                                               
    ## [209] "Estimate!!Median income (dollars)!!FAMILY INCOME BY NUMBER OF EARNERS!!2 earners"                                                                                                       
    ## [210] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!2 earners"                                                                                            
    ## [211] "Estimate!!Percent Distribution!!FAMILY INCOME BY NUMBER OF EARNERS!!3 or more earners"                                                                                                  
    ## [212] "Margin of Error!!Percent Distribution MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!3 or more earners"                                                                                       
    ## [213] "Estimate!!Median income (dollars)!!FAMILY INCOME BY NUMBER OF EARNERS!!3 or more earners"                                                                                               
    ## [214] "Margin of Error!!Median income (dollars) MOE!!FAMILY INCOME BY NUMBER OF EARNERS!!3 or more earners"                                                                                    
    ## [215] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households"                                                                                                             
    ## [216] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households"                                                                                                  
    ## [217] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households"                                                                                                          
    ## [218] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households"                                                                                               
    ## [219] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder"                                                                                         
    ## [220] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder"                                                                              
    ## [221] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder"                                                                                      
    ## [222] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder"                                                                           
    ## [223] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Living alone"                                                                           
    ## [224] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Living alone"                                                                
    ## [225] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Living alone"                                                                        
    ## [226] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Living alone"                                                             
    ## [227] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Not living alone"                                                                       
    ## [228] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Not living alone"                                                            
    ## [229] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Not living alone"                                                                    
    ## [230] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Female householder!!Not living alone"                                                         
    ## [231] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder"                                                                                           
    ## [232] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder"                                                                                
    ## [233] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder"                                                                                        
    ## [234] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder"                                                                             
    ## [235] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Living alone"                                                                             
    ## [236] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Living alone"                                                                  
    ## [237] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Living alone"                                                                          
    ## [238] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Living alone"                                                               
    ## [239] "Estimate!!Percent Distribution!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Not living alone"                                                                         
    ## [240] "Margin of Error!!Percent Distribution MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Not living alone"                                                              
    ## [241] "Estimate!!Median income (dollars)!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Not living alone"                                                                      
    ## [242] "Margin of Error!!Median income (dollars) MOE!!NONFAMILY HOUSEHOLDS!!Nonfamily households!!Male householder!!Not living alone"                                                           
    ## [243] "...243"

Use the following test to check that you downloaded the correct file:

``` r
## NOTE: No need to edit, use to check you got the right file.
assertthat::assert_that(
  df_income %>%
    filter(Geography == "0500000US01001") %>%
    pull(`Estimate!!Percent Distribution!!FAMILY INCOME BY FAMILY SIZE!!2-person families`)
  == 45.6
)
```

    ## [1] TRUE

``` r
print("Well done!")
```

    ## [1] "Well done!"

This dataset is in desperate need of some *tidying*. To simplify the
task, we’ll start by considering the `\\d-person families` columns
first.

### **q3** Tidy the `df_income` dataset by completing the code below. Pivot and rename the columns to arrive at the column names `id, geographic_area_name, category, income_estimate, income_moe`.

*Hint*: You can do this in a single pivot using the `".value"` argument
and a `names_pattern` using capture groups `"()"`. Remember that you can
use an OR operator `|` in a regex to allow for multiple possibilities in
a capture group, for example `"(Estimate|Margin of Error)"`.

``` r
df_q3 <-
  df_income %>%
  select(
    Geography,
    contains("Geographic"),
    # This will select only the numeric d-person family columns;
    # it will ignore the annotation columns
    contains("median") & matches("\\d-person families") & !contains("Annotation of")
  ) %>%
  mutate(across(contains("median"), as.numeric)) %>%
## TODO: Pivot the data, rename the columns
  pivot_longer(
    cols = -c("Geography", "Geographic Area Name"),
    names_to = c(".value", "category"),
    names_pattern = "(Estimate|Margin of Error).+(\\d-person families)"
  ) |>
  rename(
    "geographic_area_name" = "Geographic Area Name",
    "income_estimate" = "Estimate",
    "income_moe" = "Margin of Error"
  ) |>
  glimpse()
```

    ## Warning: There were 8 warnings in `mutate()`.
    ## The first warning was:
    ## ℹ In argument: `across(contains("median"), as.numeric)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion
    ## ℹ Run `dplyr::last_dplyr_warnings()` to see the 7 remaining warnings.

    ## Rows: 16,100
    ## Columns: 5
    ## $ Geography            <chr> "0500000US01001", "0500000US01001", "0500000US010…
    ## $ geographic_area_name <chr> "Autauga County, Alabama", "Autauga County, Alaba…
    ## $ category             <chr> "2-person families", "3-person families", "4-pers…
    ## $ income_estimate      <dbl> 64947, 80172, 85455, 88601, 103787, 63975, 79390,…
    ## $ income_moe           <dbl> 6663, 14181, 10692, 20739, 12387, 2297, 8851, 519…

``` r
df_q3
```

    ## # A tibble: 16,100 × 5
    ##    Geography      geographic_area_name    category    income_estimate income_moe
    ##    <chr>          <chr>                   <chr>                 <dbl>      <dbl>
    ##  1 0500000US01001 Autauga County, Alabama 2-person f…           64947       6663
    ##  2 0500000US01001 Autauga County, Alabama 3-person f…           80172      14181
    ##  3 0500000US01001 Autauga County, Alabama 4-person f…           85455      10692
    ##  4 0500000US01001 Autauga County, Alabama 5-person f…           88601      20739
    ##  5 0500000US01001 Autauga County, Alabama 6-person f…          103787      12387
    ##  6 0500000US01003 Baldwin County, Alabama 2-person f…           63975       2297
    ##  7 0500000US01003 Baldwin County, Alabama 3-person f…           79390       8851
    ##  8 0500000US01003 Baldwin County, Alabama 4-person f…           88458       5199
    ##  9 0500000US01003 Baldwin County, Alabama 5-person f…           91259       7011
    ## 10 0500000US01003 Baldwin County, Alabama 6-person f…           69609      23175
    ## # ℹ 16,090 more rows

Use the following tests to check your work:

``` r
## NOTE: No need to edit
assertthat::assert_that(setequal(
  names(df_q3),
  c("Geography", "geographic_area_name", "category", "income_estimate", "income_moe")
))
```

    ## [1] TRUE

``` r
assertthat::assert_that(
  df_q3 %>%
    filter(Geography == "0500000US01001", category == "2-person families") %>%
    pull(income_moe)
  == 6663
)
```

    ## [1] TRUE

``` r
print("Nice!")
```

    ## [1] "Nice!"

The data gives finite values for the Margin of Error, which is closely
related to the Standard Error. The Census Bureau documentation gives the
following relationship between Margin of Error and Standard Error:

$$\text{MOE} = 1.645 \times \text{SE}.$$

### **q4** Convert the margin of error to standard error. Additionally, compute a 99% confidence interval on income, and normalize the standard error to `income_CV = income_SE / income_estimate`. Provide these columns with the names `income_SE, income_lo, income_hi, income_CV`.

``` r
df_q4 <- df_q3 |>
  mutate(
    income_SE = income_moe / 1.645,
    income_CV = income_SE / income_estimate,
    income_lo = income_estimate - qnorm(1 - (1 - 0.99) / 2) * income_SE,
    income_hi = income_estimate + qnorm(1 - (1 - 0.99) / 2) * income_SE,
  )

df_q4
```

    ## # A tibble: 16,100 × 9
    ##    Geography  geographic_area_name category income_estimate income_moe income_SE
    ##    <chr>      <chr>                <chr>              <dbl>      <dbl>     <dbl>
    ##  1 0500000US… Autauga County, Ala… 2-perso…           64947       6663     4050.
    ##  2 0500000US… Autauga County, Ala… 3-perso…           80172      14181     8621.
    ##  3 0500000US… Autauga County, Ala… 4-perso…           85455      10692     6500.
    ##  4 0500000US… Autauga County, Ala… 5-perso…           88601      20739    12607.
    ##  5 0500000US… Autauga County, Ala… 6-perso…          103787      12387     7530.
    ##  6 0500000US… Baldwin County, Ala… 2-perso…           63975       2297     1396.
    ##  7 0500000US… Baldwin County, Ala… 3-perso…           79390       8851     5381.
    ##  8 0500000US… Baldwin County, Ala… 4-perso…           88458       5199     3160.
    ##  9 0500000US… Baldwin County, Ala… 5-perso…           91259       7011     4262.
    ## 10 0500000US… Baldwin County, Ala… 6-perso…           69609      23175    14088.
    ## # ℹ 16,090 more rows
    ## # ℹ 3 more variables: income_CV <dbl>, income_lo <dbl>, income_hi <dbl>

Use the following tests to check your work:

``` r
## NOTE: No need to edit
assertthat::assert_that(setequal(
  names(df_q4),
  c("Geography", "geographic_area_name", "category", "income_estimate", "income_moe",
    "income_SE", "income_lo", "income_hi", "income_CV")
))
```

    ## [1] TRUE

``` r
assertthat::assert_that(
  abs(
    df_q4 %>%
    filter(Geography == "0500000US01001", category == "2-person families") %>%
    pull(income_SE) - 4050.456
  ) / 4050.456 < 1e-3
)
```

    ## [1] TRUE

``` r
assertthat::assert_that(
  abs(
    df_q4 %>%
    filter(Geography == "0500000US01001", category == "2-person families") %>%
    pull(income_lo) - 54513.72
  ) / 54513.72 < 1e-3
)
```

    ## [1] TRUE

``` r
assertthat::assert_that(
  abs(
    df_q4 %>%
    filter(Geography == "0500000US01001", category == "2-person families") %>%
    pull(income_hi) - 75380.28
  ) / 75380.28 < 1e-3
)
```

    ## [1] TRUE

``` r
assertthat::assert_that(
  abs(
    df_q4 %>%
    filter(Geography == "0500000US01001", category == "2-person families") %>%
    pull(income_CV) - 0.06236556
  ) / 0.06236556 < 1e-3
)
```

    ## [1] TRUE

``` r
print("Nice!")
```

    ## [1] "Nice!"

One last wrangling step: We need to join the two datasets so we can
compare population with income.

### **q5** Join `df_q4` and `df_pop`.

``` r
## TODO: Join df_q4 and df_pop by the appropriate column


df_data <- inner_join(
  df_q4,
  df_pop
) |>
select(-c("Geographic Area Name", "Margin of Error!!Total", "...5"))
```

    ## Joining with `by = join_by(Geography)`

``` r
df_data
```

    ## # A tibble: 16,100 × 10
    ##    Geography  geographic_area_name category income_estimate income_moe income_SE
    ##    <chr>      <chr>                <chr>              <dbl>      <dbl>     <dbl>
    ##  1 0500000US… Autauga County, Ala… 2-perso…           64947       6663     4050.
    ##  2 0500000US… Autauga County, Ala… 3-perso…           80172      14181     8621.
    ##  3 0500000US… Autauga County, Ala… 4-perso…           85455      10692     6500.
    ##  4 0500000US… Autauga County, Ala… 5-perso…           88601      20739    12607.
    ##  5 0500000US… Autauga County, Ala… 6-perso…          103787      12387     7530.
    ##  6 0500000US… Baldwin County, Ala… 2-perso…           63975       2297     1396.
    ##  7 0500000US… Baldwin County, Ala… 3-perso…           79390       8851     5381.
    ##  8 0500000US… Baldwin County, Ala… 4-perso…           88458       5199     3160.
    ##  9 0500000US… Baldwin County, Ala… 5-perso…           91259       7011     4262.
    ## 10 0500000US… Baldwin County, Ala… 6-perso…           69609      23175    14088.
    ## # ℹ 16,090 more rows
    ## # ℹ 4 more variables: income_CV <dbl>, income_lo <dbl>, income_hi <dbl>,
    ## #   population_estimate <dbl>

# Analysis

<!-- ----------------------------------------------------------------------- -->

We now have both estimates and confidence intervals for
`\\d-person families`. Now we can compare cases with quantified
uncertainties: Let’s practice!

### **q6** Study the following graph, making sure to note what you can *and can’t* conclude based on the estimates and confidence intervals. Document your observations below and answer the questions.

``` r
## NOTE: No need to edit; run and inspect
wid <- 0.5

df_data %>%
  filter(str_detect(geographic_area_name, "Massachusetts")) %>%
  mutate(
    county = str_remove(geographic_area_name, " County,.*$"),
    county = fct_reorder(county, income_estimate)
  ) %>%

  ggplot(aes(county, income_estimate, color = category)) +
  geom_errorbar(
    aes(ymin = income_lo, ymax = income_hi),
    position = position_dodge(width = wid)
  ) +
  geom_point(position = position_dodge(width = wid)) +

  coord_flip() +
  labs(
    x = "County",
    y = "Median Household Income"
  )
```

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `county = fct_reorder(county, income_estimate)`.
    ## Caused by warning:
    ## ! `fct_reorder()` removing 2 missing values.
    ## ℹ Use `.na_rm = TRUE` to silence this message.
    ## ℹ Use `.na_rm = FALSE` to preserve NAs.

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](c09-income-assignment_files/figure-gfm/q6-task-1.png)<!-- -->

**Observations**:

- Document your observations here.
  - Bigger families tend to have larger variation in household income.
  - Nantucket has a lot of variation in all family sizes. Dukes also
    does, but to a lesser extent.
- Can you confidently distinguish between household incomes in Suffolk
  county? Why or why not?
  - No, because the confidence intervals of all household incomes have a
    lot of overlap and are all very similar.
- Which counties have the widest confidence intervals?
  - Nantucket and Dukes

In the next task you’ll investigate the relationship between population
and uncertainty.

### **q7** Plot the standard error against population for all counties. Create a visual that effectively highlights the trends in the data. Answer the questions under *observations* below.

*Hint*: Remember that standard error is a function of *both* variability
(e.g. variance) and sample size.

``` r
df_data |>
  # filter(str_detect(geographic_area_name, "Massachusetts")) %>%
  ggplot(aes(income_SE, population_estimate)) +
  geom_point(alpha = 0.1) +
  scale_x_log10() +
  scale_y_log10()
```

    ## Warning: Removed 814 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](c09-income-assignment_files/figure-gfm/q7-task-1.png)<!-- -->

**Observations**:

- What *overall* trend do you see between `SE` and population? Why might
  this trend exist?
  - There are large outliers in both `SE` and population. The data is
    very scattered, but there is an overall downward trend between
    population and SE.
- What does this *overall* trend tell you about the relative ease of
  studying small vs large counties?
  - Small counties seem harder to study, possibly because of smaller
    sample sizes.

# Going Further

<!-- ----------------------------------------------------------------------- -->

Now it’s your turn! You have income data for every county in the United
States: Pose your own question and try to answer it with the data.

### **q8** Pose your own question about the data. Create a visualization (or table) here, and document your observations.

``` r
## TODO: Pose and answer your own question about the data
df_data |>
  # filter(str_detect(geographic_area_name, "Washington")) %>%
  ggplot(aes(income_SE, population_estimate)) +
  # ggplot(aes(population_estimate)) +
  # geom_histogram(bins = 50) +
  geom_point(alpha = 0.1) +
  scale_x_log10() +
  scale_y_log10() +
  facet_grid(category ~ .)
```

    ## Warning: Removed 814 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](c09-income-assignment_files/figure-gfm/q8-task-1.png)<!-- -->

**Observations**:

- Overall, `income_SE` is higher for larger family sizes.
- Also, the larger the family size, the more variation there is in
  `income_SE`. This may be because there are some regions with many
  large families and some regions with very few.
- `income_SE` tends to be lower for counties with larger populations.

Ideas:

- Compare trends across counties that are relevant to you; e.g. places
  you’ve lived, places you’ve been, places in the US that are
  interesting to you.
- In q3 we tidied the median `\\d-person families` columns only.
  - Tidy the other median columns to learn about other people groups.
  - Tidy the percentage columns to learn about how many households of
    each category are in each county.
- Your own idea!

# References

<!-- ----------------------------------------------------------------------- -->

\[1\] Spielman SE, Folch DC, Nagle NN (2014) Patterns and causes of
uncertainty in the American Community Survey. Applied Geography 46:
147–157. <pmid:25404783>
[link](https://www.sciencedirect.com/science/article/pii/S0143622813002518?casa_token=VddzQ1-spHMAAAAA:FTq92LXgiPVloJUVjnHs8Ma1HwvPigisAYtzfqaGbbRRwoknNqZ6Y2IzszmGgIGH4JAPzQN0)
