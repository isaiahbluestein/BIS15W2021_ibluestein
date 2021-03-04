---
title: "Lab 13 Homework"
author: "Isaiah Bluestein"
date: "2021-03-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.3.3     √ purrr   0.3.4
## √ tibble  3.1.0     √ dplyr   1.0.4
## √ tidyr   1.1.2     √ stringr 1.4.0
## √ readr   1.4.0     √ forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```



```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(RColorBrewer)
library(janitor)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", ~
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, ~
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"~
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano~
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35~
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15~
```

```r
UC_admit %>% 
  naniar::miss_var_summary()
```

```
## # A tibble: 6 x 3
##   variable        n_miss pct_miss
##   <chr>            <int>    <dbl>
## 1 Perc FR              1   0.0463
## 2 FilteredCountFR      1   0.0463
## 3 Campus               0   0     
## 4 Academic_Yr          0   0     
## 5 Category             0   0     
## 6 Ethnicity            0   0
```

```r
UC_admit_clean <- clean_names(UC_admit)
#The data is pretty clean. Some unknowns for ethnicity, but I didn't change them to NA's because I feel like Unknown is a little more informative. 
```


```r
UC_admit_clean$academic_yr <- as.factor(UC_admit_clean$academic_yr)
```





**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**


```r
UC_admit_clean %>% 
  filter(ethnicity != "All") %>% 
  ggplot(aes(x = factor(academic_yr), y = filtered_count_fr, fill = ethnicity))+
  geom_col(position = "dodge")+
  theme_linedraw()+
  scale_fill_brewer(palette = "Set2")+
  labs(title = "Ethnicity of UC Admissions", x = "Year", y = "n")
```

```
## Warning: Removed 1 rows containing missing values (geom_col).
```

![](lab13_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->



```r
ui <- fluidPage(    
  
  titlePanel("UC Campus Admissions"),
  sidebarLayout(      
  sidebarPanel(
  selectInput("x", "Select Variable", choices = c("academic_yr", "campus", "category"), 
              selected = "academic_yr"),
      hr()),
  
   mainPanel(
      plotOutput("UCEthnicity")  
   )
    )
    
  )
server <- function(input, output) {
  
  
  output$UCEthnicity <- renderPlot({
    
    UC_admit_clean %>%
    filter(ethnicity != "All") %>% 
    ggplot(aes_string(x=input$x, y = "filtered_count_fr", fill = "ethnicity")) + 
    theme_linedraw()+
    geom_col(position = "dodge")+
    scale_fill_brewer(palette = "Set2")+
    labs(title = "Ethnicity of UC Admissions", x = NULL, y = "Count")+
    coord_flip()
  })
}
shinyApp(ui, server)
```

**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**


```r
ui <- fluidPage(    
  
  titlePanel("UC Campus Admissions"),
  sidebarLayout(      
  sidebarPanel(
  selectInput("x", "Select Variable", choices = c("ethnicity", "campus", "category"), 
              selected = "ethnicity"),
      hr()),
  
   mainPanel(
      plotOutput("UCAdmissions")  
   )
    )
    
  )
server <- function(input, output) {
  
  
  output$UCAdmissions <- renderPlot({
    
    UC_admit_clean %>%
    filter(ethnicity != "All") %>% 
    ggplot(aes_string(x = "academic_yr", y = "filtered_count_fr", fill = input$x)) + 
    geom_col(position = "dodge")+
    theme_linedraw()+
    scale_fill_brewer(palette = "Set2")+
    labs(title = "UC Admissions by Year", x = input$x, y= "count")
  })
}
shinyApp(ui, server)
```

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
