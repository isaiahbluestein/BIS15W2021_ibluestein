---
title: "Test Markdown"
output: 
  html_document: 
    keep_md: yes
---



# This is my first markdown file 
## This should be smaller 
### and even smaller 

```r
4*45 
```

```
## [1] 180
```


```r
#install.packages("tidyverse")
library("tidyverse")
```


```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](First-Rmarkdown_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
