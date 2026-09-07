RMS Titanic
================
(Your name here)
2020-

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [First Look](#first-look)
  - [**q1** Perform a glimpse of `df_titanic`. What variables are in
    this
    dataset?](#q1-perform-a-glimpse-of-df_titanic-what-variables-are-in-this-dataset)
  - [**q2** Skim the Wikipedia article on the RMS Titanic, and look for
    a total count of souls aboard. Compare against the total computed
    below. Are there any differences? Are those differences large or
    small? What might account for those
    differences?](#q2-skim-the-wikipedia-article-on-the-rms-titanic-and-look-for-a-total-count-of-souls-aboard-compare-against-the-total-computed-below-are-there-any-differences-are-those-differences-large-or-small-what-might-account-for-those-differences)
  - [**q3** Create a plot showing the count of persons who *did*
    survive, along with aesthetics for `Class` and `Sex`. Document your
    observations
    below.](#q3-create-a-plot-showing-the-count-of-persons-who-did-survive-along-with-aesthetics-for-class-and-sex-document-your-observations-below)
- [Deeper Look](#deeper-look)
  - [**q4** Replicate your visual from q3, but display `Prop` in place
    of `n`. Document your observations, and note any new/different
    observations you make in comparison with q3. Is there anything
    *fishy* in your
    plot?](#q4-replicate-your-visual-from-q3-but-display-prop-in-place-of-n-document-your-observations-and-note-any-newdifferent-observations-you-make-in-comparison-with-q3-is-there-anything-fishy-in-your-plot)
  - [**q5** Create a plot showing the group-proportion of occupants who
    *did* survive, along with aesthetics for `Class`, `Sex`, *and*
    `Age`. Document your observations
    below.](#q5-create-a-plot-showing-the-group-proportion-of-occupants-who-did-survive-along-with-aesthetics-for-class-sex-and-age-document-your-observations-below)
- [Notes](#notes)

*Purpose*: Most datasets have at least a few variables. Part of our task
in analyzing a dataset is to understand trends as they vary across these
different variables. Unless we’re careful and thorough, we can easily
miss these patterns. In this challenge you’ll analyze a dataset with a
small number of categorical variables and try to find differences among
the groups.

*Reading*: (Optional) [Wikipedia
article](https://en.wikipedia.org/wiki/RMS_Titanic) on the RMS Titanic.

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

``` r
df_titanic <- as_tibble(Titanic)
```

*Background*: The RMS Titanic sank on its maiden voyage in 1912; about
67% of its passengers died.

# First Look

<!-- -------------------------------------------------- -->

### **q1** Perform a glimpse of `df_titanic`. What variables are in this dataset?

``` r
## TASK: Perform a `glimpse` of df_titanic
glimpse(df_titanic)
```

    ## Rows: 32
    ## Columns: 5
    ## $ Class    <chr> "1st", "2nd", "3rd", "Crew", "1st", "2nd", "3rd", "Crew", "1s…
    ## $ Sex      <chr> "Male", "Male", "Male", "Male", "Female", "Female", "Female",…
    ## $ Age      <chr> "Child", "Child", "Child", "Child", "Child", "Child", "Child"…
    ## $ Survived <chr> "No", "No", "No", "No", "No", "No", "No", "No", "No", "No", "…
    ## $ n        <dbl> 0, 0, 35, 0, 0, 0, 17, 0, 118, 154, 387, 670, 4, 13, 89, 3, 5…

**Observations**:

- (List all variables here)

### **q2** Skim the [Wikipedia article](https://en.wikipedia.org/wiki/RMS_Titanic) on the RMS Titanic, and look for a total count of souls aboard. Compare against the total computed below. Are there any differences? Are those differences large or small? What might account for those differences?

``` r
## NOTE: No need to edit! We'll cover how to
## do this calculation in a later exercise.
df_titanic %>% summarize(total = sum(n))
```

    ## # A tibble: 1 × 1
    ##   total
    ##   <dbl>
    ## 1  2201

**Observations**:

- Write your observations here
  - According to the Wikipedia page, there were 2208 passengers and crew
    aboard
- Are there any differences?
  - The data set includes 2201, which is slightly different
- If yes, what might account for those differences?
  - Different sources having different information, inaccurate records

### **q3** Create a plot showing the count of persons who *did* survive, along with aesthetics for `Class` and `Sex`. Document your observations below.

*Note*: There are many ways to do this.

``` r
## TASK: Visualize counts against `Class` and `Sex`

survivors <- df_titanic |>
  filter(Survived == "Yes") |>
  select(-c(Age, Survived)) |>
  group_by(Sex, Class) |>
  summarise(n = sum(n))
```

    ## `summarise()` has regrouped the output.
    ## ℹ Summaries were computed grouped by Sex and Class.
    ## ℹ Output is grouped by Sex.
    ## ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ## ℹ Use `summarise(.by = c(Sex, Class))` for per-operation grouping
    ##   (`?dplyr::dplyr_by`) instead.

``` r
survivors
```

    ## # A tibble: 8 × 3
    ## # Groups:   Sex [2]
    ##   Sex    Class     n
    ##   <chr>  <chr> <dbl>
    ## 1 Female 1st     141
    ## 2 Female 2nd      93
    ## 3 Female 3rd      90
    ## 4 Female Crew     20
    ## 5 Male   1st      62
    ## 6 Male   2nd      25
    ## 7 Male   3rd      88
    ## 8 Male   Crew    192

``` r
ggplot(survivors, aes(x = Sex, y = n, fill = Class)) +
  geom_col()
```

![](c01-titanic-assignment_files/figure-gfm/q3-task-1.png)<!-- -->

**Observations**:

- Most of the men who survived were crew.
- A large portion of 1st and 2nd class women survived, but very few 1st
  and 2nd class men.

# Deeper Look

<!-- -------------------------------------------------- -->

Raw counts give us a sense of totals, but they are not as useful for
understanding differences between groups. This is because the
differences we see in counts could be due to either the relative size of
the group OR differences in outcomes for those groups. To make
comparisons between groups, we should also consider *proportions*.\[1\]

The following code computes proportions within each `Class, Sex, Age`
group.

``` r
## NOTE: No need to edit! We'll cover how to
## do this calculation in a later exercise.
df_prop <-
  df_titanic %>%
  group_by(Class, Sex, Age) %>%
  mutate(
    Total = sum(n),
    Prop = n / Total
  ) %>%
  ungroup()
df_prop
```

    ## # A tibble: 32 × 7
    ##    Class Sex    Age   Survived     n Total    Prop
    ##    <chr> <chr>  <chr> <chr>    <dbl> <dbl>   <dbl>
    ##  1 1st   Male   Child No           0     5   0    
    ##  2 2nd   Male   Child No           0    11   0    
    ##  3 3rd   Male   Child No          35    48   0.729
    ##  4 Crew  Male   Child No           0     0 NaN    
    ##  5 1st   Female Child No           0     1   0    
    ##  6 2nd   Female Child No           0    13   0    
    ##  7 3rd   Female Child No          17    31   0.548
    ##  8 Crew  Female Child No           0     0 NaN    
    ##  9 1st   Male   Adult No         118   175   0.674
    ## 10 2nd   Male   Adult No         154   168   0.917
    ## # ℹ 22 more rows

### **q4** Replicate your visual from q3, but display `Prop` in place of `n`. Document your observations, and note any new/different observations you make in comparison with q3. Is there anything *fishy* in your plot?

``` r
survivors_prop <- df_prop |>
  filter(Survived == "Yes") |>
  select(-c(Age, Survived)) |>
  group_by(Sex, Class) |>
  summarise(Prop = sum(Prop, na.rm = TRUE))
```

    ## `summarise()` has regrouped the output.
    ## ℹ Summaries were computed grouped by Sex and Class.
    ## ℹ Output is grouped by Sex.
    ## ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ## ℹ Use `summarise(.by = c(Sex, Class))` for per-operation grouping
    ##   (`?dplyr::dplyr_by`) instead.

``` r
survivors_prop
```

    ## # A tibble: 8 × 3
    ## # Groups:   Sex [2]
    ##   Sex    Class  Prop
    ##   <chr>  <chr> <dbl>
    ## 1 Female 1st   1.97 
    ## 2 Female 2nd   1.86 
    ## 3 Female 3rd   0.912
    ## 4 Female Crew  0.870
    ## 5 Male   1st   1.33 
    ## 6 Male   2nd   1.08 
    ## 7 Male   3rd   0.433
    ## 8 Male   Crew  0.223

``` r
ggplot(survivors_prop, aes(x = Sex, y = Prop, fill = Class)) +
  geom_col()
```

![](c01-titanic-assignment_files/figure-gfm/q4-task-1.png)<!-- -->

**Observations**:

- Write your observations here.
  - Women were significantly more likely to survive than men
  - Very few crew survived
- Is there anything *fishy* going on in your plot?

### **q5** Create a plot showing the group-proportion of occupants who *did* survive, along with aesthetics for `Class`, `Sex`, *and* `Age`. Document your observations below.

*Hint*: Don’t forget that you can use `facet_grid` to help consider
additional variables!

``` r
survivors_prop <- df_prop |>
  filter(Survived == "Yes") |>
  select(-c(Survived))

survivors_prop
```

    ## # A tibble: 16 × 6
    ##    Class Sex    Age       n Total     Prop
    ##    <chr> <chr>  <chr> <dbl> <dbl>    <dbl>
    ##  1 1st   Male   Child     5     5   1     
    ##  2 2nd   Male   Child    11    11   1     
    ##  3 3rd   Male   Child    13    48   0.271 
    ##  4 Crew  Male   Child     0     0 NaN     
    ##  5 1st   Female Child     1     1   1     
    ##  6 2nd   Female Child    13    13   1     
    ##  7 3rd   Female Child    14    31   0.452 
    ##  8 Crew  Female Child     0     0 NaN     
    ##  9 1st   Male   Adult    57   175   0.326 
    ## 10 2nd   Male   Adult    14   168   0.0833
    ## 11 3rd   Male   Adult    75   462   0.162 
    ## 12 Crew  Male   Adult   192   862   0.223 
    ## 13 1st   Female Adult   140   144   0.972 
    ## 14 2nd   Female Adult    80    93   0.860 
    ## 15 3rd   Female Adult    76   165   0.461 
    ## 16 Crew  Female Adult    20    23   0.870

``` r
ggplot(survivors_prop, aes(x = Sex, y = Prop, fill = Class)) +
  geom_col() +
  facet_grid(. ~ Age)
```

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_col()`).

![](c01-titanic-assignment_files/figure-gfm/q5-task-1.png)<!-- -->

**Observations**:

- (Write your observations here.)
- If you saw something *fishy* in q4 above, use your new plot to explain
  the fishy-ness.
  - Children were very likely to survive

# Notes

<!-- -------------------------------------------------- -->

\[1\] This is basically the same idea as [Dimensional
Analysis](https://en.wikipedia.org/wiki/Dimensional_analysis); computing
proportions is akin to non-dimensionalizing a quantity.
