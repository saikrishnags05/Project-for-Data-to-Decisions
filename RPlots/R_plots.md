Rplots
================
# Contributor Statement

Chad, Sai and Rhonda each contributed to the project plan. Sai worked on
Research Question 1 Chad worked on Research Question 2 Rhonda worked on
Research Question 3 Rhonda proofread

Each person went ahead and readdressed their research question from the
initial “Research Questions” assignment. Secondly, each person chose a
particularly informative plot that best-fit or best-explained the
exploration and analysis of their research question. Thirdly, each
person provided their own explanation of their plot in one or two
paragraphs. Chad combined the work of all three team persons and also
performed the proofreading.


## Research Question 1

**My Research Question asks how much delay (in days) exists between enrolling in an event, entering into the system, and the enrollment approval.

Reason:-

- This will analyze the time each person takes to within HFS to enroll and then begin their appointments for both Nebraska and Iowa.
- In turn, this will inform HFS' understanding of the enrollment process and its delays for its clients.
- Hopefully, HFS can use this data to identify where the slowdowns exist and how the delays can be mitigated.

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
```
##  (Links to an external site.)Average days Taken for Enrollment (Iowa)
``` r
HFS_data$state[HFS_data$state == "IA"] <- "iowa"
IA<-filter(HFS_data,HFS_data$state=="iowa")#,HFS_data$program_name=='Mental Health')
ag_ia<-aggregate(IA$AD_APD~IA$facility+IA$AD_year+IA$program_name,IA,mean)
ag_ia$`IA$AD_APD`<-round(ag_ia$`IA$AD_APD`,0)
```

The below graph displays the average time taken for enrollment within Iowa for HFS, over the past eight years. We notice:

- The enrollment time is decreasing from year to year.
- Many of the enrollment events concern the program named Mental Health.
- Enrollment time takes less time for schools.  It is likely that many persons who enroll in schools are children.
- Per Substance Use, only one facility ensures that persons are both enrolled and accepted simultaneously or very quickly.
- The below plot tells us a lot about the enrollments over the past two years.  For example, we might blame Covid-19 for the large delays in enrollment.

``` r
p <- ggplot(data = ag_ia, aes(y=`IA$AD_APD`,x=`IA$facility`,color= `IA$AD_year` ))
p + geom_point()+facet_wrap(~`IA$program_name`,scale='free')+
theme(axis.text.x = element_text(angle=90, vjust=1, hjust=1))+
  labs(title = "Average Time taken for a person to enroll for a program") +ylab('Average days for a person to register')+xlab('Facility in Iowa')+ labs(colour = "Years")
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/Sai_Krishna_IA.jpeg)<!-- -->


## Average days Taken for Enrollment (Nebraska)
``` r
HFS_data$state[HFS_data$state == "NE"] <- "nebraska"
ne<-filter(HFS_data,HFS_data$state=="nebraska")#,HFS_data$program_name=='Mental Health')
ag_ne<-aggregate(ne$AD_APD~ne$facility+ne$AD_year+ne$program_name,ne,mean)
ag_ne$`ne$AD_APD`<-round(ag_ne$`ne$AD_APD`,0)
```

The below graph displays the average time taken for enrollment within Nebraska for HFS, over the past ten years. We notice:

HFS should seek to understand why location is closely coupled to missing appointments. This might be related to particular locations being more difficult to access, such as difficult traffic or limited parking. On the other hand, each location might have very different populations, whether via ethnicity or financial differences that provide different levels of privilege, which in turn affect the client’s ability to allocate time for appointments. Further research should seek to understand what about location or the clients that visit these locations affect missed appointments.

- There are few facilities where it is taking more time to enroll for a person compared to the previous year.
- Most of the facilities conduct events on Mental Health
- We can also tell that the entire enrollment process is late in all the regions in Nebraska when it is compared to previous years
- From the plot I can tell that all the enrollments are being late for the past 2 years there may be multiple reasons. Example: - lockdown because Covid-19 which stopped the process

``` r
#,out.width = "1200",out.height='800'
p <- ggplot(data = ag_ne, aes(y=`ne$AD_APD`,x=`ne$facility`,color=`ne$AD_year` ))
p + geom_point()+facet_wrap(~`ne$program_name`,scale='free')+
  theme(axis.text.x = element_text(angle=90, vjust=1, hjust=1))+
  labs(title = "Average Time taken for a person to enroll for a program") +ylab('Average days for a person to register')+xlab('Facility in Nebraska')+ labs(colour = "Years")
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/Sai_Krishna_NE.jpeg)<!-- -->


# Research Question 2

HFS facility locations have a significant effect on the number of missed appointments.

The research question builds on existing knowledge that Omaha and Iowa contain pockets of segregated populations, both in terms of ethnicity, finances, and privilege. The research question poses that location might have a significant effect on missed appointments. If a particular location has a population with financial access to vehicles or even multiple vehicles, they might be more able to make their appointments. Initially, we hope to identify that facility location affects missed appointments. If we can show this is true, we can further explore what about the facility location negatively affects clients’ ability to make their appointments. This research data specifically looks at the mental health program, the largest for HFS. This research also breaks down no-shows by the type of therapist, knowing that different types of therapists might handle different appointments, each of which might be related to the likelihood of missing appointments. This research question does not break down the data by ethnicity, only because the ethnicity counts were not large enough to be meaningful for each facility and job title.

``` r
library('tidyverse') 
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.5     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.0.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library('ggplot2') # for sample plot if required
library('dplyr') # to use pipelines '%>%' for data set
```

``` r
HFS_data<-read.csv("HFS Service Data.csv") # read data set
```

``` r
data <- HFS_data
therapists = data %>% filter(data$job_title == "THERAPIST I" | data$job_title == "THERAPIST II" | data$job_title == "THERAPIST III" | data$job_title == "LEAD THERAPIST" | data$job_title == "Therapist")
```

#### Plot: No show percentage by facility, ethnicity, and type of therapist

``` r
# possibly look at no show rate by facility
mental_health <- therapists %>% filter(program_name == "Mental Health")
noshow_stats <- mental_health %>% group_by(facility, job_title) %>% count(is_noshow) %>% filter(n > 20)
noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
shows <- noshow_stats %>% filter(is_noshow == "TRUE")
noshow_stats_consolidated <- inner_join(shows,noshows,by=c("facility","job_title"), suffix = c("no", "yes"))
#noshow_stats_consolidated <- select(noshow_stats_consolidated, -c(is_noshowno, is_noshowyes))
noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
df <- noshow_percent

ggplot(data = df) + geom_point(mapping = aes(x = reorder(job_title,noshow_percent), y = noshow_percent))  + facet_wrap(~ reorder(facility, -noshow_percent)) +
ggtitle("No Shows is a Location Problem\n(No Show Percentage by Job Title Across Facilities)") +
xlab("Job Title") +
ylab("Percentage of Clients Who Miss Appointments") +
 theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), legend.position = "none") 
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/NoShow_By_Location.png)<!-- -->

The purpose of the above graph is to highlight the stark difference in the y-axis for each facet. The y-axis shows the no-show percentage, which captures the percentage of appointments that are missed, e.g., clients do not show up for their appointments. The facet is a facility, such as the “Center Mall Office” and “Omaha (Blondo) Reporting Center.” The graph shows significant differences in no-show percentage by facility. The range itself varies from around 5% to almost 30%, depending on the facility. The plot also separates out job function since it has a significant effect on no-show percentage and tends to drop from Therapist I to Therapist II.

The takeaway is that no-show percentage is tightly related to the facility. This means that the rate at which clients skip or miss appointments are tightly coupled with the specific facility. This means that some facilities show much higher rates of appointment misses than others. For example, the Gendler HFS location should expect very high appointment no shows, up to 25% contrasted with the Sarpy location that averages fewer than 10% no shows.

## Research Question 3

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

## Bar chart of Ethnicity & Facility

This BarChart shows us that the majority of Latinos served attend the
North Omaha Campus and the Heartland Family Service-Central location.

``` r
p
```

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/RPlots/Rplots_files/figure-gfm/RQ3_2.jpeg)<!-- -->
