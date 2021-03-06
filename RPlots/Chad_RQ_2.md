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
paragraphs. Chad combined the work of all three team persons and Rhonda
performed the proofreading.

# Research Question 2

HFS facility locations have a significant effect on the number of missed
appointments.

The research question builds on existing knowledge that Omaha and Iowa
contain pockets of segregated populations, both in terms of ethnicity,
finances, and priviledge. The research question poses that location
might have a significant effect on missed appointments. If a particular
location has a population with financial access to vehicles, or even
multiple vehicles, they might be more able to make their appointments.
Initially, we hope to identify that facility location affects missed
appointments. If we can show this is true, we can further explore what
about the facility location negatively affects client’s ability to make
their appointments. This research data specifically looks at the mental
health program, the largest for HFS. This research also breaks down no
shows by the type of therapist, knowing that different types of
therapists might handle different appointments, each of which might be
related to the likelihood of missing appointments. This research
quesiton does not break down the data by ethnicity, only because the
ethnicity counts were not large enough to be meaningful for each
facility and job title.

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

The purpose of the above graph is to highlight the stark different in
the y-axis for each facet. The y-axis shows no show percentage, which
captures the percentage of appointments that are missed, e.g., clients
do not show up for their appointments. The facet is facility, such as
the “Center Mall Office” and “Omaha (Blondo) Reporting Center.” The
graph shows significant differences in no show percentage by facility.
The range itself varies from around 5% to almost 30%, depending on the
facility. The plot also separates out job function since it has a
significant effect on no show percentage and tends to drop from
Therapist I to Therapist II.

The takeaway is that no show percentage is tightly related to the
facility. This means that the rate at which clients skip or miss
appointments is tightly coupled with the specific facility. This means
that some facilities show much higher rates of appointment misses than
others. For example, the Gendler HFS location should expect very high
appointment no shows, up to 25% contrasted with the Sarpy location that
averages fewer than 10% no shows.

HFS should seek to understand why location is closely coupled to missing
appointments. This might be related to particular locations being more
difficult to access, such as difficult traffic or limited parking. On
the other hand, each location might have very different populations,
whether via ethnicity or financial differences that provide different
levels of privilege, which in turn affect client’s ability to allocate
time for appointments. Further research should seek to understand what
about location or the clients that visit these locations affect missed
appointments.
