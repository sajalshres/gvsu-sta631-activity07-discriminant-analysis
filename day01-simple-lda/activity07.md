Activity 7 - Linear Discriminant Analysis
================

## Task 2: Load the necessary packages

``` r
library(tidyverse)
library(tidymodels)
library(discrim)
```

## Task 3: Load the data and

``` r
resume <- readr::read_csv("https://www.openintro.org/data/csv/resume.csv")
```

Answer the following question:

1.  TODO: Add answer here

## Task 4: LDA

``` r
# Convert received_callback to a factor with more informative labels
resume <- resume %>% 
  mutate(received_callback = factor(received_callback, labels = c("No", "Yes")))

# LDA
lda_years <- discrim_linear() %>% 
  set_mode("classification") %>% 
  set_engine("MASS") %>% 
  fit(received_callback ~ log(years_experience), data = resume)

lda_years
```

    ## parsnip model object
    ## 
    ## Call:
    ## lda(received_callback ~ log(years_experience), data = data)
    ## 
    ## Prior probabilities of groups:
    ##         No        Yes 
    ## 0.91950719 0.08049281 
    ## 
    ## Group means:
    ##     log(years_experience)
    ## No               1.867135
    ## Yes              1.998715
    ## 
    ## Coefficients of linear discriminants:
    ##                            LD1
    ## log(years_experience) 1.638023

## Task 5: Predictions

``` r
predict(lda_years, new_data = resume, type = "prob")
```

    ## # A tibble: 4,870 × 2
    ##    .pred_No .pred_Yes
    ##       <dbl>     <dbl>
    ##  1    0.923    0.0769
    ##  2    0.923    0.0769
    ##  3    0.923    0.0769
    ##  4    0.923    0.0769
    ##  5    0.884    0.116 
    ##  6    0.923    0.0769
    ##  7    0.928    0.0724
    ##  8    0.885    0.115 
    ##  9    0.939    0.0612
    ## 10    0.923    0.0769
    ## # … with 4,860 more rows

``` r
augment(lda_years, new_data = resume) %>% 
  conf_mat(truth = received_callback, estimate = .pred_class)
```

    ##           Truth
    ## Prediction   No  Yes
    ##        No  4478  392
    ##        Yes    0    0

`{augment(lda_years, new_data = resume) %>%}   accuracy(truth = received_callback, estimate = .pred_class)`

## Challenge: Compare with logistic regression

Fit a similar model as we just explored, but instead using logistic
regression. Discuss how the results are similar and different.

``` r
# TODO: perform logistic regression
```
