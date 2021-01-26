---
title: "Midterm 1"
author: "Isaiah Bluestein"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library("janitor")
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?** 

R is an open source scripting language. Rstudio is a graphic user interface (GUI) that allows its users to interact with the program in a simpler, more visually appealing, and powerful way. GitHub is an online platform that allows coders to organize, store, and share their code. It enables people to easily share, utilize, and understand code used for data science. RMarkdown is useful since it can produce output files that are visually appealing and easy to read, such as PDFs and Word documents. These aspects all work synergistically to help the R community be transparent, welcoming, and to conduct great science.  


**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**

We discussed vectors, data matrices, and data frames. We are working primarily with data frames for several reasons: they are an efficient way of packaging and working with data, they display useful information such as the data class, and there are many functions in the tidyverse (like the dplyr functions) that make working with data frames simple and easy.


In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, …
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00…
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M…
```


**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- janitor::clean_names(elephants)
elephants$sex <- as.factor(elephants$sex)
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, …
## $ height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00…
## $ sex    <fct> M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M,…
```


**5. (2 points) How many male and female elephants are represented in the data?**

```r
elephants %>% 
  count(sex)
```

```
## # A tibble: 2 x 2
##   sex       n
## * <fct> <int>
## 1 F       150
## 2 M       138
```


**6. (2 points) What is the average age all elephants in the data?**

```r
elephants %>% 
  summarize(average_age = mean(age))
```

```
## # A tibble: 1 x 1
##   average_age
##         <dbl>
## 1        11.0
```


**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>% 
  group_by(sex) %>% 
  summarize(mean_age = mean(age), mean_height = mean(height), n = n())
```

```
## # A tibble: 2 x 4
##   sex   mean_age mean_height     n
## * <fct>    <dbl>       <dbl> <int>
## 1 F        12.8         190.   150
## 2 M         8.95        185.   138
```


**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

```r
elephants %>%
  group_by(sex) %>% 
  filter(age > 25) %>% 
  summarize(min_height = min(height), max_height = max(height), mean_height = mean(height), n = n())
```

```
## # A tibble: 2 x 5
##   sex   min_height max_height mean_height     n
##   <fct>      <dbl>      <dbl>       <dbl> <int>
## 1 F           206.       278.        233.    25
## 2 M           237.       304.        273.     8
```


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**

```r
gabon <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

```r
glimpse(gabon)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16,…
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.0…
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "…
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 5…
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "P…
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, …
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.0…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.7…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.4…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.2…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, …
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 2…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.8…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.5…
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, …
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.8…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.9…
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11,…
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.7…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.9…
```

```r
gabon$HuntCat <- as.factor(gabon$HuntCat)
gabon$LandUse <- as.factor(gabon$LandUse)
glimpse(gabon)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16,…
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.0…
## $ HuntCat                 <fct> Moderate, None, None, None, None, None, None,…
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 5…
## $ LandUse                 <fct> Park, Park, Park, Logging, Park, Park, Park, …
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, …
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.0…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.7…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.4…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.2…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, …
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 2…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.8…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.5…
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, …
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.8…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.9…
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11,…
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.7…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.9…
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

```r
gabon %>%
  filter(HuntCat == "High" | HuntCat == "Moderate") %>% 
  group_by(HuntCat) %>% 
  summarize(mean_Diversity_BirdSpecies = mean(Diversity_BirdSpecies), mean_Diversity_MammalSpecies = mean(Diversity_MammalSpecies))
```

```
## # A tibble: 2 x 3
##   HuntCat  mean_Diversity_BirdSpecies mean_Diversity_MammalSpecies
## * <fct>                         <dbl>                        <dbl>
## 1 High                           1.66                         1.74
## 2 Moderate                       1.62                         1.68
```


**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
gabon %>% 
  filter(Distance < 5 | Distance > 20) %>%
  group_by(Distance < 5, Distance > 20) %>% 
  summarize(across(c(RA_Apes, RA_Birds, RA_Elephant, RA_Monkeys, RA_Rodent, RA_Ungulate), mean))
```

```
## `summarise()` has grouped output by 'Distance < 5'. You can override using the `.groups` argument.
```

```
## # A tibble: 2 x 8
## # Groups:   Distance < 5 [2]
##   `Distance < 5` `Distance > 20` RA_Apes RA_Birds RA_Elephant RA_Monkeys
##   <lgl>          <lgl>             <dbl>    <dbl>       <dbl>      <dbl>
## 1 FALSE          TRUE               7.21     44.5      0.557        40.1
## 2 TRUE           FALSE              0.08     70.4      0.0967       24.1
## # … with 2 more variables: RA_Rodent <dbl>, RA_Ungulate <dbl>
```


**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

```r
#Elephants are afraid of mice, so are there less elephants in areas with lots of rodents? 
#I'll include distance from people too, since that affects both rodent and elephant abundance. 
gabon %>% 
  select(RA_Elephant, RA_Rodent, Distance) %>% 
  arrange(desc(RA_Elephant))
```

```
## # A tibble: 24 x 3
##    RA_Elephant RA_Rodent Distance
##          <dbl>     <dbl>    <dbl>
##  1        2.3       2.09    18.8 
##  2        2.2       4.37     5.78
##  3        1.33      1.06    18.3 
##  4        1.11      3.1     24.1 
##  5        1         2.52    16.0 
##  6        0.99      2.97     6.61
##  7        0.86      6.04    17.3 
##  8        0.77      5.41     5.14
##  9        0.56      3.66    20.8 
## 10        0.52      4.54     8.23
## # … with 14 more rows
```

```r
#Hmmm, kind of hard to tell, but it seems that there are less elephants when there are more rodents. 
```

```r
#let's look at some averages! Maybe that will help. 
gabon %>% 
  summarise(mean_RA_rodent = mean(RA_Rodent))
```

```
## # A tibble: 1 x 1
##   mean_RA_rodent
##            <dbl>
## 1           3.28
```

```r
#So the average relative abundance of rodents is 3.279. What is the average elephant abundance when the rodent abundance is above or below its mean?
gabon %>% 
  group_by(RA_Rodent < 3.277917, RA_Rodent > 3.277917) %>% 
  summarize(mean_ra_elephant = mean(RA_Elephant), n = n())
```

```
## `summarise()` has grouped output by 'RA_Rodent < 3.277917'. You can override using the `.groups` argument.
```

```
## # A tibble: 2 x 4
## # Groups:   RA_Rodent < 3.277917 [2]
##   `RA_Rodent < 3.277917` `RA_Rodent > 3.277917` mean_ra_elephant     n
##   <lgl>                  <lgl>                             <dbl> <int>
## 1 FALSE                  TRUE                              0.505    11
## 2 TRUE                   FALSE                             0.578    13
```
