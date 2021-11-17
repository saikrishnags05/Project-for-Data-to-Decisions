R_Plot_Assignment
================
Rhonda Silva
11/16/2021

``` r
library('ggplot2')
library('tidyverse')
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v tibble  3.1.5     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.0.2     v forcats 0.5.1
    ## v purrr   0.3.4

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library('dplyr')
HFS <- read.csv("HFS Service Data.csv")
HFS.Ethnicity2<-HFS [c(2,3,4,24,25,45,49)]
HFS.Ethnicity2 = HFS.Ethnicity2[HFS.Ethnicity2$ethnic_identity != "Not Collected",]

HFS.Ethnicity2$ethnic_identity[HFS.Ethnicity2$ethnic_identity== "Not Spanish/Hispanic/Latino"]<- "Not Latino"

HFS.Ethnicity2$ethnic_identity[HFS.Ethnicity2$ethnic_identity== "Mexican"]<- "Latino"

HFS.Ethnicity2$ethnic_identity[HFS.Ethnicity2$ethnic_identity== "Other Hispanic or Latino"]<- "Latino"
```

## RQ 3

The data cleaning results in a new subset of 5 variables based on the
attributes analyzed in the first research question. Important columns
within this data cleaning are listed below: program_unit_desc
ethnic_identity age facility program_type

This BoxPlot identifies that most Latinos receive services for Mental
Health Programs and are between the ages of 18 and 50.

``` r
ggplot(HFS.Ethnicity2) +
  aes(x = program_unit_description, y = age, colour = ethnic_identity) +
  geom_boxplot(shape = "circle", 
               fill = "#112446") +
  scale_color_hue(direction = 1) +
  coord_flip() +
  theme_minimal()
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/RQ_3_1.jpeg)<!-- -->

##Bar chart of Ethnicity & Facility

This BarChart shows us that the majority of Latinos served attend the
North Omaha Campus and the Heartland Family Service-Central location.

``` r
p
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/RQ3_2.jpeg)<!-- -->
