---
title: "Lab 3 Homework"
author: "Isaiah Bluestein"
date: "2021-01-19"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

```r
getwd()
```

```
## [1] "D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_ibluestein/lab3"
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
help(msleep)
```

```
## starting httpd help server ... done
```

```r
#These data were taken from V. M. Savage and G. B. West. A quantitative, theoretical framework for understanding mammalian sleep. Proceedings of the National Academy of Sciences, 104 (3):1051-1056, 2007.
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- msleep
glimpse(sleep)
```

```
## Rows: 83
## Columns: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Greater s...
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bos", "...
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi", "c...
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorpha", "...
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", NA, "d...
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1, 3.0...
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0.6, 0....
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.3833333...
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9, 21.0...
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0.07000...
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.490, 0...
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim(sleep)
```

```
## [1] 83 11
```

```r
#There are 11 variables and 83 observations
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
anyNA(sleep)
```

```
## [1] TRUE
```

```r
#Yes, there are NAs in the data
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```

```r
#32 herbivores
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small <- subset(sleep, bodywt<= 1)
small
```

```
## # A tibble: 36 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Owl ~ Aotus omni  Prim~ <NA>                17         1.8      NA       7  
##  2 Grea~ Blar~ omni  Sori~ lc                  14.9       2.3       0.133   9.1
##  3 Vesp~ Calo~ <NA>  Rode~ <NA>                 7        NA        NA      17  
##  4 Guin~ Cavis herbi Rode~ domesticated         9.4       0.8       0.217  14.6
##  5 Chin~ Chin~ herbi Rode~ domesticated        12.5       1.5       0.117  11.5
##  6 Star~ Cond~ omni  Sori~ lc                  10.3       2.2      NA      13.7
##  7 Afri~ Cric~ omni  Rode~ <NA>                 8.3       2        NA      15.7
##  8 Less~ Cryp~ omni  Sori~ lc                   9.1       1.4       0.15   14.9
##  9 Big ~ Epte~ inse~ Chir~ lc                  19.7       3.9       0.117   4.3
## 10 Euro~ Erin~ omni  Erin~ lc                  10.1       3.5       0.283  13.9
## # ... with 26 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

```r
large <- subset(sleep, bodywt >= 200)
large
```

```
## # A tibble: 7 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667  20  
## 2 Asia~ Elep~ herbi Prob~ en                   3.9      NA        NA      20.1
## 3 Horse Equus herbi Peri~ domesticated         2.9       0.6       1      21.1
## 4 Gira~ Gira~ herbi Arti~ cd                   1.9       0.4      NA      22.1
## 5 Pilo~ Glob~ carni Ceta~ cd                   2.7       0.1      NA      21.4
## 6 Afri~ Loxo~ herbi Prob~ vu                   3.3      NA        NA      20.7
## 7 Braz~ Tapi~ herbi Peri~ vu                   4.4       1         0.9    19.6
## # ... with 2 more variables: brainwt <dbl>, bodywt <dbl>
```

8. What is the mean weight for both the small and large mammals?

```r
small_weight <- small$bodywt
small_mean_weight <- mean(small_weight)
small_mean_weight
```

```
## [1] 0.2596667
```


```r
large_weight <- large$bodywt
large_mean_weight <- mean(large_weight)
large_mean_weight
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

```r
small_sleep <- small$sleep_total
small_sleep_mean <- mean(small_sleep)
small_sleep_mean
```

```
## [1] 12.65833
```



```r
large_sleep <- large$sleep_total
large_sleep_mean <- mean(large_sleep)
large_sleep_mean
```

```
## [1] 3.3
```

```r
#small animals sleep more on average than large animals
```

10. Which animal is the sleepiest among the entire dataframe?


```r
sleep[order(sleep$sleep_total),]
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Gira~ Gira~ herbi Arti~ cd                   1.9       0.4      NA      22.1
##  2 Pilo~ Glob~ carni Ceta~ cd                   2.7       0.1      NA      21.4
##  3 Horse Equus herbi Peri~ domesticated         2.9       0.6       1      21.1
##  4 Roe ~ Capr~ herbi Arti~ lc                   3        NA        NA      21  
##  5 Donk~ Equus herbi Peri~ domesticated         3.1       0.4      NA      20.9
##  6 Afri~ Loxo~ herbi Prob~ vu                   3.3      NA        NA      20.7
##  7 Casp~ Phoca carni Carn~ vu                   3.5       0.4      NA      20.5
##  8 Sheep Ovis  herbi Arti~ domesticated         3.8       0.6      NA      20.2
##  9 Asia~ Elep~ herbi Prob~ en                   3.9      NA        NA      20.1
## 10 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667  20  
## # ... with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

```r
#the sleepiest animals is the Little Brown Bat. To find this, I sorted the dataframe by sleep_total and scrolled to the bottom to find the largest number.   
```


```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```

```r
sleepmax <- subset(sleep, sleep_total>=19.9)
sleepmax
```

```
## # A tibble: 1 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Litt~ Myot~ inse~ Chir~ <NA>                19.9         2         0.2   4.1
## # ... with 2 more variables: brainwt <dbl>, bodywt <dbl>
```






## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
