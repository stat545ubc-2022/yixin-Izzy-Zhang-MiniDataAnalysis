Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
cancer_sample
```

    ## # A tibble: 569 × 32
    ##          ID diagnosis radius_mean texture_mean perimeter_mean area_mean
    ##       <dbl> <chr>           <dbl>        <dbl>          <dbl>     <dbl>
    ##  1   842302 M                18.0         10.4          123.      1001 
    ##  2   842517 M                20.6         17.8          133.      1326 
    ##  3 84300903 M                19.7         21.2          130       1203 
    ##  4 84348301 M                11.4         20.4           77.6      386.
    ##  5 84358402 M                20.3         14.3          135.      1297 
    ##  6   843786 M                12.4         15.7           82.6      477.
    ##  7   844359 M                18.2         20.0          120.      1040 
    ##  8 84458202 M                13.7         20.8           90.2      578.
    ##  9   844981 M                13           21.8           87.5      520.
    ## 10 84501001 M                12.5         24.0           84.0      476.
    ## # … with 559 more rows, and 26 more variables: smoothness_mean <dbl>,
    ## #   compactness_mean <dbl>, concavity_mean <dbl>, concave_points_mean <dbl>,
    ## #   symmetry_mean <dbl>, fractal_dimension_mean <dbl>, radius_se <dbl>,
    ## #   texture_se <dbl>, perimeter_se <dbl>, area_se <dbl>, smoothness_se <dbl>,
    ## #   compactness_se <dbl>, concavity_se <dbl>, concave_points_se <dbl>,
    ## #   symmetry_se <dbl>, fractal_dimension_se <dbl>, radius_worst <dbl>,
    ## #   texture_worst <dbl>, perimeter_worst <dbl>, area_worst <dbl>, …

The data is tidy because each row is an observation, each column is a
variable, and each cell is a value.

Pick 8 variables:

``` r
cancer_sample <- cancer_sample %>% select(diagnosis, radius_mean, texture_mean, smoothness_mean, compactness_mean, concavity_mean, concave_points_mean, symmetry_mean)
cancer_sample
```

    ## # A tibble: 569 × 8
    ##    diagnosis radius_mean texture_mean smoothness_mean compactness_mean
    ##    <chr>           <dbl>        <dbl>           <dbl>            <dbl>
    ##  1 M                18.0         10.4          0.118            0.278 
    ##  2 M                20.6         17.8          0.0847           0.0786
    ##  3 M                19.7         21.2          0.110            0.160 
    ##  4 M                11.4         20.4          0.142            0.284 
    ##  5 M                20.3         14.3          0.100            0.133 
    ##  6 M                12.4         15.7          0.128            0.17  
    ##  7 M                18.2         20.0          0.0946           0.109 
    ##  8 M                13.7         20.8          0.119            0.164 
    ##  9 M                13           21.8          0.127            0.193 
    ## 10 M                12.5         24.0          0.119            0.240 
    ## # … with 559 more rows, and 3 more variables: concavity_mean <dbl>,
    ## #   concave_points_mean <dbl>, symmetry_mean <dbl>

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

Untidy data: (concatenate two variables together in one column)

``` r
cancer_sample$concavity_summary <- paste(cancer_sample$concavity_mean, cancer_sample$concave_points_mean, sep= "_")
cancer_sample
```

    ## # A tibble: 569 × 9
    ##    diagnosis radius_mean texture_mean smoothness_mean compactness_mean
    ##    <chr>           <dbl>        <dbl>           <dbl>            <dbl>
    ##  1 M                18.0         10.4          0.118            0.278 
    ##  2 M                20.6         17.8          0.0847           0.0786
    ##  3 M                19.7         21.2          0.110            0.160 
    ##  4 M                11.4         20.4          0.142            0.284 
    ##  5 M                20.3         14.3          0.100            0.133 
    ##  6 M                12.4         15.7          0.128            0.17  
    ##  7 M                18.2         20.0          0.0946           0.109 
    ##  8 M                13.7         20.8          0.119            0.164 
    ##  9 M                13           21.8          0.127            0.193 
    ## 10 M                12.5         24.0          0.119            0.240 
    ## # … with 559 more rows, and 4 more variables: concavity_mean <dbl>,
    ## #   concave_points_mean <dbl>, symmetry_mean <dbl>, concavity_summary <chr>

Tidy Data: (Separate concavity_summary)

``` r
cancer_sample <- cancer_sample %>% separate(concavity_summary, c("concavity_mean", "concave_points_mean"), "_")
cancer_sample
```

    ## # A tibble: 569 × 8
    ##    diagnosis radius_mean texture_mean smoothness_mean compactness_mean
    ##    <chr>           <dbl>        <dbl>           <dbl>            <dbl>
    ##  1 M                18.0         10.4          0.118            0.278 
    ##  2 M                20.6         17.8          0.0847           0.0786
    ##  3 M                19.7         21.2          0.110            0.160 
    ##  4 M                11.4         20.4          0.142            0.284 
    ##  5 M                20.3         14.3          0.100            0.133 
    ##  6 M                12.4         15.7          0.128            0.17  
    ##  7 M                18.2         20.0          0.0946           0.109 
    ##  8 M                13.7         20.8          0.119            0.164 
    ##  9 M                13           21.8          0.127            0.193 
    ## 10 M                12.5         24.0          0.119            0.240 
    ## # … with 559 more rows, and 3 more variables: symmetry_mean <dbl>,
    ## #   concavity_mean <chr>, concave_points_mean <chr>

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *Do malignant and benign tumors have different sizes?*
2.  *Is there distribution difference between M vs B in each variable?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

I chose *1* because it is easy to test (t-test) and helps give some
intuition of the relationship between tumor size and diagnosis. I chose
*2* because this is easy to visualize using plots. (we may only focus on
certain variables)
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

``` r
#leaving only radius and concavity for fulture analysis
reduced_cancer <- cancer_sample %>% select(diagnosis, radius_mean, smoothness_mean, concavity_mean, concave_points_mean)

#changing characters to numeric
reduced_cancer <- reduced_cancer %>% mutate(across(all_of("concavity_mean"), as.numeric),
       across(all_of("concave_points_mean"), as.numeric))

#remove outliers
reduced_cancer <- reduced_cancer %>%
  filter(!(abs(radius_mean - median(radius_mean)) > 2*sd(radius_mean)))

reduced_cancer <- reduced_cancer %>%
  filter(!(abs(smoothness_mean - median(smoothness_mean)) > 2*sd(smoothness_mean)))

reduced_cancer <- reduced_cancer %>%
  filter(!(abs(concavity_mean - median(concavity_mean)) > 2*sd(concavity_mean))) 

reduced_cancer <- reduced_cancer %>%
  filter(!(abs(concave_points_mean - median(concave_points_mean)) > 2*sd(concave_points_mean)))

##arrange diagnosis and radius
reduced_cancer <- reduced_cancer %>% arrange(reduced_cancer, diagnosis, radius_mean)

reduced_cancer
```

    ## # A tibble: 427 × 5
    ##    diagnosis radius_mean smoothness_mean concavity_mean concave_points_mean
    ##    <chr>           <dbl>           <dbl>          <dbl>               <dbl>
    ##  1 B                6.98          0.117          0                  0      
    ##  2 B                7.69          0.0867         0.0925             0.0136 
    ##  3 B                7.73          0.0810         0                  0      
    ##  4 B                8.20          0.086          0.0159             0.00592
    ##  5 B                8.22          0.0940         0.132              0.0217 
    ##  6 B                8.57          0.104          0.0256             0.0151 
    ##  7 B                8.60          0.107          0                  0      
    ##  8 B                8.62          0.0975         0.0206             0.00780
    ##  9 B                8.67          0.0914         0                  0      
    ## 10 B                8.73          0.115          0.0413             0.0192 
    ## # … with 417 more rows

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

I am choosing the first plot for mini-project-1. Output may be different
because we removed outliers in this analysis. On top of scatterplot, I
added boxplots to better visualize the distribution between different
groups.

``` r
#create categorical variables based on radius (size)
reduced_cancer$size <- cut(reduced_cancer$radius_mean,
                       breaks=c(0, 10, 15, 20, 25, 30),
                       labels=c('very small', 'small', 'medium', 'large', 'very large'))
head(reduced_cancer$size)
```

    ## [1] very small very small very small very small very small very small
    ## Levels: very small small medium large very large

``` r
#plot size against smoothness, separated by diagnosis. jitter to see points better
ggplot(reduced_cancer, aes(x=size, y = smoothness_mean)) + 
  geom_boxplot()+
  geom_jitter(aes(color = diagnosis), alpha = 0.6)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**1**: I reorder the plots so that smoothness_mean in each group are in
ascending order. I used this function to better visualize the
distribution differences in each size group.

``` r
ggplot(reduced_cancer, aes(x=fct_reorder(size, smoothness_mean), y = smoothness_mean)) + 
  geom_boxplot()+
  geom_jitter(aes(color = diagnosis), alpha = 0.6)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**2**: I grouped “large”, “very large” and “very small” into “others”
group since they have very few data points.

``` r
reduced_cancer$size <- fct_other(reduced_cancer$size, keep = c("medium", "small"))
ggplot(reduced_cancer, aes(x=fct_reorder(size, smoothness_mean), y = smoothness_mean)) + 
  geom_boxplot()+
  geom_jitter(aes(color = diagnosis), alpha = 0.6)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: Do malignant and benign tumors have different
sizes?

**Variable of interest**: radius_mean (grouped by diagnosis)

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

t-test based on benign vs malignant radius mean. H0: mu_benign ==
mu_malignant HA: mu_benign != mu_malignant

``` r
benign_radius <- reduced_cancer %>% filter(diagnosis == "B") %>% pull(radius_mean)
malignant_radius <- reduced_cancer %>% filter(diagnosis == "M") %>% pull(radius_mean)
radius_test <- t.test(benign_radius, malignant_radius)
radius_test
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  benign_radius and malignant_radius
    ## t = -13.743, df = 115.6, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -3.943522 -2.950017
    ## sample estimates:
    ## mean of x mean of y 
    ##  12.18553  15.63230

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
library(broom)
test_summary <- tidy(radius_test)
test_summary <- rename(test_summary, mean_difference = estimate, 
        benign_radius_mean = estimate1, 
        malignant_radius_mean = estimate2)
test_summary
```

    ## # A tibble: 1 × 10
    ##   mean_difference benign_radius_m… malignant_radiu… statistic  p.value parameter
    ##             <dbl>            <dbl>            <dbl>     <dbl>    <dbl>     <dbl>
    ## 1           -3.45             12.2             15.6     -13.7 4.57e-26      116.
    ## # … with 4 more variables: conf.low <dbl>, conf.high <dbl>, method <chr>,
    ## #   alternative <chr>

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
library(here)
```

    ## here() starts at /Users/yixinzhang/Desktop/yixin-Izzy-Zhang-MiniDataAnalysis

``` r
here::here()
```

    ## [1] "/Users/yixinzhang/Desktop/yixin-Izzy-Zhang-MiniDataAnalysis"

``` r
write_csv(test_summary, here::here("output", "mini_project_result.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(test_summary, "mini_project_result.rds")
test_summary_read <- readRDS("mini_project_result.rds")
test_summary_read
```

    ## # A tibble: 1 × 10
    ##   mean_difference benign_radius_m… malignant_radiu… statistic  p.value parameter
    ##             <dbl>            <dbl>            <dbl>     <dbl>    <dbl>     <dbl>
    ## 1           -3.45             12.2             15.6     -13.7 4.57e-26      116.
    ## # … with 4 more variables: conf.low <dbl>, conf.high <dbl>, method <chr>,
    ## #   alternative <chr>

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
