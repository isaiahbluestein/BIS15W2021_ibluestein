---
title: "Lab 2 Homework"
author: "Isaiah Bluestein"
date: "2021-01-11"
md_document:
    variant: markdown_github
output: 
  html_document: 
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

2. What is a data matrix in R?  

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  


```r
measurements <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
measurements_matrix <- matrix(measurements, nrow =8, byrow = T)
measurements_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
spring_names <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", "Too Hot Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")

spring_names 
```

```
## [1] "Bluebell Spring"  "Opal Spring"      "Riverside Spring" "Too Hot Spring"  
## [5] "Mystery Spring"   "Emerald Spring"   "Black Spring"     "Pearl Spring"
```

```r
scientist_names <- c("Jill", "Steve", "Susan")
scientist_names
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
rownames(measurements_matrix) <- spring_names  
colnames(measurements_matrix) <-scientist_names



measurements_matrix
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## Too Hot Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```


6. Calculate the mean temperature of all three springs.


```r
combined_temp <- rowSums(measurements_matrix)
mean_temp <- combined_temp/3
Mean <- mean_temp
Mean
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##         30.85000         30.20000         32.73333         35.46667
```


7. Add this as a new column in the data matrix.  


```r
measurements_with_mean <- cbind(measurements_matrix, Mean)
measurements_with_mean
```

```
##                   Jill Steve Susan     Mean
## Bluebell Spring  36.25 35.40 35.30 35.65000
## Opal Spring      35.15 35.35 33.35 34.61667
## Riverside Spring 30.70 29.65 29.20 29.85000
## Too Hot Spring   39.70 40.05 38.65 39.46667
## Mystery Spring   31.85 31.40 29.30 30.85000
## Emerald Spring   30.20 30.65 29.75 30.20000
## Black Spring     32.90 32.50 32.80 32.73333
## Pearl Spring     36.80 36.45 33.15 35.46667
```


8. Show Susan's value for Opal Spring only.


```r
susans_values <- measurements_with_mean[ ,3]  
susans_values
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##            35.30            33.35            29.20            38.65 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##            29.30            29.75            32.80            33.15
```


9. Calculate the mean for Jill's column only.  

```r
jills_measurements <- measurements_with_mean[ ,1]
jills_measurements
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##            36.25            35.15            30.70            39.70 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##            31.85            30.20            32.90            36.80
```

```r
jills_total <- sum(jills_measurements)

jills_mean <- jills_total/8
jills_mean
```

```
## [1] 34.19375
```


10. Use the data matrix to perform one calculation or operation of your interest.


```r
measurements_in_fahrenheit <- (measurements_with_mean * 9/5) + 32 
measurements_in_fahrenheit
```

```
##                    Jill  Steve  Susan   Mean
## Bluebell Spring   97.25  95.72  95.54  96.17
## Opal Spring       95.27  95.63  92.03  94.31
## Riverside Spring  87.26  85.37  84.56  85.73
## Too Hot Spring   103.46 104.09 101.57 103.04
## Mystery Spring    89.33  88.52  84.74  87.53
## Emerald Spring    86.36  87.17  85.55  86.36
## Black Spring      91.22  90.50  91.04  90.92
## Pearl Spring      98.24  97.61  91.67  95.84
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
