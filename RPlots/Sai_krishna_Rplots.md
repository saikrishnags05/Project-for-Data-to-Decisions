Rplots
================

## RQ 1

**My Research Question is to tell that how much time of delay it take to
takes for enrolling into the event, Entering into system and Approving
the enrollment.** `By Sai Krishna`

Reason:-

-   This will help in analyzing the time taking per enrolling from
    starting to ending w.r.t to State Nebraska and Iowa at their
    facility.

-   This will help the HFS to understand the average time taken for
    enrollment for person

-   If we find any kind any slow processing in any of the facility then
    we can try to improve by providing required technical support or man
    power to perform the task done in less time.

``` r
library('ggplot2') # for sample plot if required
library('dplyr') # to use pipelines '%>%' for data set
```

    ##
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ##
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ##
    ##     intersect, setdiff, setequal, union

``` r
HFS_data<-read.csv("HFS Service Data.csv") # read data set


selected_columns<-c("program_name","facility","actual_date","event_name","date_entered",   
                    "approved_date","zip","state","age","ethnic_identity")
# Above list is used for selecting the columns in the dataset

HFS_data<-HFS_data %>%
  select(selected_columns)  # this command is used for selecting the required columns
```

    ## Note: Using an external vector in selections is ambiguous.
    ## i Use `all_of(selected_columns)` instead of `selected_columns` to silence this message.
    ## i See <https://tidyselect.r-lib.org/reference/faq-external-vector.html>.
    ## This message is displayed once per session.

``` r
HFS_data$AD<-as.Date(-(HFS_data$actual_date), origin = '2021-08-25')
HFS_data$ED<-as.Date(-(HFS_data$date_entered), origin = '2021-08-25')
HFS_data$AD_M_Y <- format(as.Date(HFS_data$AD), "%Y-%m")
HFS_data$AD_year<-(format(as.Date(HFS_data$ED), "%Y"))
HFS_data$AD_ED<-(HFS_data$actual_date-HFS_data$date_entered)
HFS_data$ED_APD<-(HFS_data$date_entered-HFS_data$approved_date)
HFS_data$AD_APD<-(abs(HFS_data$actual_date-HFS_data$approved_date))
HFS_data$state[HFS_data$state == "NE"] <- "nebraska"
HFS_data$state[HFS_data$state == "IA"] <- "iowa"
```

``` r
IA<-filter(HFS_data,HFS_data$state=="iowa")#,HFS_data$program_name=='Mental Health')
ag_ia<-aggregate(IA$AD_APD~IA$facility+IA$AD_year+IA$program_name,IA,mean)
ag_ia$`IA$AD_APD`<-round(ag_ia$`IA$AD_APD`,0)
```

If we observe the Graph we can tell that over all behavior of enrollment
process w.r.t the time taken for per person to enroll for an event from
past **8 years** in the state of **Iowa**.


* There are few facilities where it is taking more time to enroll for a person compared to the previous year.
* Most of the facilities conduct events on **Mental Health**
* We can notice that in the enrolling time is so less in schools and we can even assume that most of the people who enroll to the course are children.
* In **Substance Use** we can clearly tell that out of 5 facilities only 1 facility have organized for more years and their time of enrollment is also almost equal
* From the plot, I can tell that all the enrollments are been late for the past 2 years there may be multiple reasons.
**Example: -** lockdown because Covid-19 which stopped the process

``` r
p <- ggplot(data = ag_ia, aes(y=`IA$AD_APD`,x=`IA$facility`,color= `IA$AD_year` ))
p + geom_point()+facet_wrap(~`IA$program_name`,scale='free')+
theme(axis.text.x = element_text(angle=90, vjust=1, hjust=1))+
  labs(title = "Average Time taken for a person to enroll for a program") +ylab('Average days for a person to register')+xlab('Facility in Iowa')+ labs(colour = "Years")
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/Sai_Krishna_IA.jpeg)<!-- -->

``` r
ne<-filter(HFS_data,HFS_data$state=="nebraska")#,HFS_data$program_name=='Mental Health')
ag_ne<-aggregate(ne$AD_APD~ne$facility+ne$AD_year+ne$program_name,ne,mean)
ag_ne$`ne$AD_APD`<-round(ag_ne$`ne$AD_APD`,0)
```

If we observe the Graph, we can tell that overall behavior of enrollment
process w.r.t the time taken for per person to enroll for an event from
past **10 years** in the state of **Nebraska**.

* There are few facilities where it is taking more time to enroll for a person compared to the previous year.
* Most of the facilities conduct events on **Mental Health**
* We can also tell that the entire enrollment process is late in all the regions in Nebraska when it is compared to previous years
* From the plot I can tell that all the enrollments are being late for the past 2 years there may be multiple reasons. 

**Example: -** lockdown because Covid-19 which stopped the process


``` r
#,out.width = "1200",out.height='800'
p <- ggplot(data = ag_ne, aes(y=`ne$AD_APD`,x=`ne$facility`,color=`ne$AD_year` ))
p + geom_point()+facet_wrap(~`ne$program_name`,scale='free')+
  theme(axis.text.x = element_text(angle=90, vjust=1, hjust=1))+
  labs(title = "Average Time taken for a person to enroll for a program") +ylab('Average days for a person to register')+xlab('Facility in Nebraska')+ labs(colour = "Years")
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/Sai_Krishna_NE.jpeg)<!-- -->
