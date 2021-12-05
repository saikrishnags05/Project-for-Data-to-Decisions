## Contributor Statement

Chad, Sai and Rhonda each contributed to the project plan. Sai worked on
Research Question 1 Chad worked on Research Question 2 Rhonda worked on
Research Question 3 Chad proofread

The entire group is performing data exploration concerning location and
its relationship to other columns, such as the onboarding duration,
ethnicity, program name, and job title. Given this general direction of
interest, each of us explored a related research question. Fortunately,
based on the data exploration documentation, we are updating the
direction of our research. Specifically, how does onboarding duration
change when enhanced by interesting findings concerning ethnicity,
program name, and job title. Part of the progress demonstrated by this
assignment intertwines these aspects.

## Introduction

Heartland Family Service, which was founded in Omaha in 1875, served
more than 79,000 individuals and families last year through direct
services, education, and outreach from more than 15 facilities in east
central Nebraska and southwest Iowa. In the following focus areas, our
programs provide important human services to children, individuals, and
families:

• Housing, Safety, and Financial Stability

• Child & Family Well-Being

• Counseling & Prevention

Heartland Family Service’s objective is to enhance communities by
providing education, counseling, and support services to individuals and
families. Last year, Heartland Family Service, which was founded in
Omaha in 1875, served 60,309 individuals and families through direct
services, education, and outreach from more than 15 locations in east
central Nebraska and southwest Iowa. In the following target areas:
Child & Family Well-Being, Counseling & Prevention, and Housing, Safety,
& Financial Stability,their programs provide important human services to
the individuals and families that ultimately create the future of our
community.

# Cleaning Process

## RQ 1

The first reserach question explores datetime relationships per person.
Specifically, we look at the time delay between signup and the first
appointment, as well as appointment volumes by datetime for HFS.

### Reason

This will help in analyzing the time taken per enrolling; from the
signup to the appointment date. It will also give us insights concerning
demographic breakdowns per customer, e.g., their ethnic identity and
location. We will also explore the total time taken for each event. This
analysis will provide HFS data concerning their enrollment speed by
different customer facets.

The data cleaning results in a new subset of data based on the
attributes analyzed in the first research question. Important columns
within this data cleaning are listed below:

`facility`, `actual_date`, `event_name`, `date_entered`,
`approved_date`, `program_unit_description`, `zip`,
`state`,`ethnic_identity`

    library('tidyverse') 

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    library('ggplot2') # for sample plot if required
    library('dplyr') # to use pipelines '%>%' for data set

Read the files and Select the required columns

    HFS_data<-read.csv("HFS Service Data.csv") # read data set


    selected_columns<-c("program_name","facility","actual_date","event_name","date_entered",   
                        "approved_date","zip","state","age","ethnic_identity")
    HFS_data<-HFS_data %>%
      select(selected_columns)  # this command is used for selecting the required columns

    ## Note: Using an external vector in selections is ambiguous.
    ## ℹ Use `all_of(selected_columns)` instead of `selected_columns` to silence this message.
    ## ℹ See <https://tidyselect.r-lib.org/reference/faq-external-vector.html>.
    ## This message is displayed once per session.

As the sources Information is fetched out to csv on 25th August 2021.

By using this date i have created original data of the eventsm with the
help of as.Date() function in R

    HFS_data$AD<-as.Date(-(HFS_data$actual_date), origin = '2021-08-25')
    HFS_data$ED<-as.Date(-(HFS_data$date_entered), origin = '2021-08-25')
    HFS_data$AD_year<-(format(as.Date(HFS_data$ED), "%Y"))
    HFS_data$AD_ED<-(HFS_data$actual_date-HFS_data$date_entered)
    HFS_data$ED_APD<-(HFS_data$date_entered-HFS_data$approved_date)
    HFS_data$AD_APD<-(abs(HFS_data$actual_date-HFS_data$approved_date))

Omit all the **NA** values

    HFS_data<-na.omit(HFS_data)

Rename all the state name with it’s full form

    HFS_data$state<- as.character(HFS_data$state)
    HFS_data$state[HFS_data$state == "NE"] <- "nebrska"
    HFS_data$state[HFS_data$state == "IA"] <- "iowa"
    HFS_data$state[HFS_data$state == "SC"] <- "south carolina"
    HFS_data$state[HFS_data$state == "NC"] <- "north carolina"
    HFS_data$state[HFS_data$state == "CO"] <- "colorado"

# Iowa

In the below code i have filtered the original data with the state name
and aggregate the whole data based to get an average time taken for
completing the HFS process.

    HFS_data$state[HFS_data$state == "IA"] <- "iowa"
    IA<-subset(HFS_data,HFS_data$state=="iowa")#,HFS_data$program_name=='Mental Health')
    ag_ia<-aggregate(IA$AD_APD~IA$facility+IA$AD_year+IA$program_name,IA,mean)
    ag_ia$`IA$AD_APD`<-round(ag_ia$`IA$AD_APD`,0)
    names(ag_ia)[names(ag_ia) == "IA$facility"] <- "facility"     
    names(ag_ia)[names(ag_ia) == "IA$AD_year"] <-"AD_year"      
    names(ag_ia)[names(ag_ia) == "IA$program_name"] <-"program_name" 
    names(ag_ia)[names(ag_ia) == "IA$AD_APD"] <-"AD_APD"

In the Below code i have build a model to see if it have better
confidence in between the attributes in the data frame or not

    # Build the model

    model <- lm(AD_APD ~facility+AD_year, data = ag_ia)
    model

    ## 
    ## Call:
    ## lm(formula = AD_APD ~ facility + AD_year, data = ag_ia)
    ## 
    ## Coefficients:
    ##                                                (Intercept)  
    ##                                                     2.5038  
    ##                                 facilityCenter Mall Office  
    ##                                                     3.3721  
    ##                 facilityHeartland Family Service - Central  
    ##                                                     6.2717  
    ## facilityHeartland Family Service - Child and Family Center  
    ##                                                    -0.4007  
    ##                 facilityHeartland Family Service - Gendler  
    ##                                                     1.7091  
    ##                facilityHeartland Family Service - Glenwood  
    ##                                                    -2.7996  
    ##         facilityHeartland Family Service - Heartland Homes  
    ##                                                    10.2086  
    ##                   facilityHeartland Family Service - Lakin  
    ##                                                    -5.0006  
    ##                   facilityHeartland Family Service - Logan  
    ##                                                     7.8554  
    ##                   facilityHeartland Family Service - Sarpy  
    ##                                                     3.7283  
    ##             facilityKanesville Alternative Learning Center  
    ##                                                    -1.7283  
    ##                           facilityKirn Junior High  School  
    ##                                                    -2.8469  
    ##                               facilityKreft Primary School  
    ##                                                    -1.5871  
    ##                          facilityLewis Central High School  
    ##                                                     0.4415  
    ##                        facilityLewis Central Middle School  
    ##                                                    -1.2661  
    ##                                        facilityMicah House  
    ##                                                    -1.9297  
    ##     facilityNorth Omaha Intergenerational Campus (Service)  
    ##                                                    -2.6552  
    ##                       facilityThomas Jefferson High School  
    ##                                                     0.1272  
    ##                     facilityTitan Hill Intermediate School  
    ##                                                    -0.5585  
    ##                               facilityWilson Middle School  
    ##                                                    -0.6734  
    ##                                                AD_year2015  
    ##                                                    -0.5958  
    ##                                                AD_year2016  
    ##                                                     2.7291  
    ##                                                AD_year2017  
    ##                                                     1.5950  
    ##                                                AD_year2018  
    ##                                                     0.2244  
    ##                                                AD_year2019  
    ##                                                     1.2875  
    ##                                                AD_year2020  
    ##                                                     4.3986  
    ##                                                AD_year2021  
    ##                                                     0.7679

    new.ag_ia.avg.time <- data.frame(
      facility = c("Thomas Jefferson High School","Titan Hill Intermediate School", "Heartland Family Service - Heartland Homes")
      ,AD_year=c('2021','2020','2020')
      )

    predict(model, newdata = new.ag_ia.avg.time)

    ##         1         2         3 
    ##  3.398884  6.343865 17.111017

    predict(model, newdata = new.ag_ia.avg.time, interval = "confidence")

    ##         fit       lwr      upr
    ## 1  3.398884 -3.981651 10.77942
    ## 2  6.343865  0.627303 12.06043
    ## 3 17.111017  5.049678 29.17236

tried to get a predict value for over all data for Iowa

    # 1. Add predictions 
    pred.int <- predict(model, interval = "prediction")

    ## Warning in predict.lm(model, interval = "prediction"): predictions on current data refer to _future_ responses

    mydata <- cbind(ag_ia, pred.int)

Now i have created a plot to see how the predicted value is similar with

    # 2. Regression line + confidence intervals
    library("ggplot2")
    p <- ggplot(mydata, aes(y=fit,x=AD_APD,color=AD_year ))+ geom_point()+geom_smooth(method = "lm")+facet_wrap(~AD_year,scales = 'free')

    # 3. Add prediction intervals
    p + geom_line(aes(x = lwr), color = "red", linetype = "dashed")+
      geom_line(aes(x = upr), color = "red", linetype = "dashed")

    ## `geom_smooth()` using formula 'y ~ x'

![](ComprehensiveAnalysis_files/figure-markdown_strict/unnamed-chunk-10-1.png)
(optional) Now for just verification i am trying to get a Residuals of
the data that is present in Iowa

    aggregate_IA<-(aggregate(AD_APD~state+facility+AD_year,IA,mean))
    aov_group_IA <- aov(AD_APD~facility, data = aggregate_IA)
    summary_group_IA<-summary(aov_group_IA)
    summary_group_IA

    ##             Df Sum Sq Mean Sq F value  Pr(>F)   
    ## facility    19  321.2  16.905   2.304 0.00932 **
    ## Residuals   51  374.3   7.339                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

## RQ 2

### Job Position, Ethnicity, Location, and Dropped Appointments (Chad Crowe)

### RQ Overview

The third research question concerns exploring the job role of
therapists within HFS, with specific interest in appointment no shows.
When patients fail to appear for appointments, this costs HFS time and
costs the patient opportunity for therapy. We explore whether there
exist clear patterns that might contribute to patients missing visits,
such as a location or ethnicity effect. It might be the case that
particular facilities are less friendly in supporting a language, which
might effect the rate of dropped appointments.

The research also explores whether job title effects dropped
appointments. Job requirements might change from title to title that
might have an effect on dropped appointments. This research explores the
phenomenon.

Initial research also explores appointment duration. Based on the given
data, it is unknown whether appointment duration is fixed by insurance
or varies between patients. This research explores duration of
appointments across job position, ethnicity, location, and the rate of
dropped appointments too. While success is not determined by duration,
Dr. Juarez mentioned how HFS is very interested in exploring patterns
pertaining to the number of appointments and durations by each patient
since it affects the funding HFS receives.

### Datasets Used

The data explored in this research question include five columns:

-   Job Title (Therapists I, II, and III)

-   Ethnicity

-   Facility Location

-   Appointment Duration

-   Appointment No Shows

Each column will be explored in the following section. The section will
describe the number of rows & columns and provide sample headers. The
section will also include a description of the metadata, such as what
information is available for understanding and interpreting the data.
The section will also cover the rationale for remediating and cleaning
the data, such as handling empty data. It will also include a
description of the approach and the code required for replication.

### Description of Datasets

#### Job Title (Therapists I, II, and III)

    library('dplyr')
    library('tidyverse')
    library('moderndive')
    #data <- read.csv("/Users/ccrowe/github/isqa8600_ChadCrowe/programs/data/HFS Service Data.csv")
    data <- read.csv("HFS Service Data.csv")

The data contains 8745 rows. If we filter out NA values for job title
there are 8745. This means each row has a job title and there are no NA
values. Given that there is no missing data, there is no need to handle
missing data.

Below is a plot of available job titles:

    tibble_data <- as_tibble(data)
    # data header
    head(tibble_data$job_title)

    ## [1] "Clinical Supervisor" "THERAPIST II"        "THERAPIST II"       
    ## [4] "THERAPIST II"        "THERAPIST II"        "THERAPIST II"

    job_title_counts <- tibble_data %>% group_by(job_title) %>% count(sort=TRUE)
    ggplot(job_title_counts) + geom_point(mapping = aes(x = reorder(job_title,-n), y = n)) +
    ggtitle("Count of Rows for each Job Title") +
    xlab("Job Title") +
    ylab("Count of Job Title's Occurrence") +
     #ylim(0, 130) + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), legend.position = "none")

![](ComprehensiveAnalysis_files/figure-markdown_strict/job_title_all-1.png)

Most job titles have fewer than fifty instances. Job titles with many
instances include therapist, clinical supervisor, case managers, and
admin assists. Of those job titles, there are five types of therapists.
Given most of the primary job titles are therapists, the exploration of
job titles will focus on therapists. We filter the job titles to the
various therapist job positions.

    therapists = data %>% filter(data$job_title == "THERAPIST I" | data$job_title == "THERAPIST II" | data$job_title == "THERAPIST III" | data$job_title == "LEAD THERAPIST" | data$job_title == "Therapist")

If we filter out therapists there are only 7246 rows, so 1500 fewer
rows.

#### Plot - Histogram of Duration for All Therapist Job Titles

Firstly we filter the dataset for therapist job titles. Most of the data
concerns therapists, more than 80% of the dataset.

    data <- as_tibble(read.csv("HFS Service Data.csv"))
    therapists = data %>% filter(data$job_title == "THERAPIST I" | data$job_title == "THERAPIST II" | data$job_title == "THERAPIST III" | data$job_title == "LEAD THERAPIST")
    #nrow(therapists)# 1500 fewer rows

We want to explore the relationship of duration for each of the
therapist job titles. Therefore, we create a histogram of duration for
each therapist job title.

    hist(therapists$total_duration_num, breaks=50,xlim=c(0,150),main="Histogram of Duration for All Records\n With a Therapist Job Title", xlab="Duration (minutes)", ylab = "Count")

![](ComprehensiveAnalysis_files/figure-markdown_strict/therapists_vs_duration-1.png)

#### Ethnicity

    tibble_data <- as_tibble(data)
    ethnicity <- tibble_data %>% group_by(ethnic_identity) %>% count(sort=TRUE)
    # data header
    head(tibble_data$ethnic_identity)

    ## [1] "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino"
    ## [3] "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino"
    ## [5] "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino"

There are no NAs for the ethnic\_identity column. The ethnic identities
are categorized as Mexian, Hispanic/Latino, and not
Spanish/Hispanic/Latino. Ninety-percent of the data (7820 rows) are not
Spanish, Hispanic or Latino. The following plot shows the diparity of
counts within the ethic\_identity column.

    ggplot(ethnicity) + geom_point(mapping = aes(x = reorder(ethnic_identity,-n), y = n)) +
    ggtitle("Count of Rows for each Ethnicity") +
    xlab("Ethnicity") +
    ylab("Count of Ethnicity") +
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), legend.position = "none")

![](ComprehensiveAnalysis_files/figure-markdown_strict/ethnic_plot-1.png)

Given that most categories have fewer than two-hundred persons, one
simlification is to create a binary column for Not
Spanish/Hispanic/Latino and Spanish/Hispanic/Lantino. We’ll filter out
unknown since it contains no ethnic identity information. Otherwise,
there are no NAs or missing data in this column so there’s no need to
handle or filter out NAs.

    two_ethnicities <- tibble_data %>% mutate(is_minority = ethnic_identity != "Not Spanish/Hispanic/Latino") %>% filter(ethnic_identity != "Unknown")
    two_ethnicities %>% group_by(is_minority) %>% count()

    ## # A tibble: 2 × 2
    ## # Groups:   is_minority [2]
    ##   is_minority     n
    ##   <lgl>       <int>
    ## 1 FALSE        7820
    ## 2 TRUE          784

When we filter by the identified ethinicities and filter out the unknown
category we get almost 800 rows of ethnicities HFS tracks.

#### Facility Location

Below we can see a breakdown of records per facility. We group by
facility and sort by the facilities with the most usage. This will help
us understand the usage of HFS facilities within the dataset.

    tibble_data <- as_tibble(data)
    # data header
    head(tibble_data$facility)

    ## [1] "Heartland Family Service - Logan" "Center Mall Office"              
    ## [3] "Center Mall Office"               "Center Mall Office"              
    ## [5] "Center Mall Office"               "Center Mall Office"

    grouped_facility <- tibble_data %>% group_by(facility) %>% count(sort=TRUE)
    #ordered <- transform(grouped_facility, variable=reorder(facility, n) ) 
    ggplot(grouped_facility) + geom_point(mapping = aes(x = reorder(facility,-n), y = n)) +
    ggtitle("Count of Records by Facility") +
    xlab("Facility") +
    ylab("Count of Records at a Facility") +
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), legend.position = "none")

![](ComprehensiveAnalysis_files/figure-markdown_strict/facility_breakdown-1.png)

    # check for NAs
    tibble_data %>% filter(facility == NA) %>% count()

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1     0

From the graph we see nine main facilities with more than two-hundred
records. There are three facilities with more than one-thousand rows. We
want to avoid aggregating smaller facilities together since each
facility might be very different. For now, we will leave the smaller
facilities in the data. Later on, we might remove facilities with very
few users. No rows are NA so there is no need to handle NAs or missing
data in this column.

#### Plot: Count of each Zip Code for Therapists

This plot aims to get an idea of zip code and whether it will be helpful
in any location analysis. In the below graph, we group by zip code, plot
the count, and sort descending.

    counts <- therapists %>% group_by(zip) %>% count(sort = TRUE)
    plot(counts$n, main = "Plot of Record Count per Zip",xlab="Zip Index (Descending)", ylab="Count of Records per Zip")

![](ComprehensiveAnalysis_files/figure-markdown_strict/location-1.png)

The graph shows only two main zip-codes, 681, 680, and 0, which is not
very informative. The other zip codes had fewer than fifty records.

We’ll use the facility’s name now that we’ve decided not to use general
location or zip code. We’ll also keep exploring job title, duration, and
ethnicity.

#### Plot of General Location for Therapist Work

There are a few columns I’m interested in using, such as general
location and the zip code. In the below plots, I’ll explore these
columns.

    counts <- therapists %>% group_by(general_location) %>% count(sort = TRUE)
    plot(counts$n, main = "Plot of Record Count per Location",xlab="Location Index (Descending)", ylab="Count of Records per Location")

![](ComprehensiveAnalysis_files/figure-markdown_strict/general_location-1.png)

The data exploration looked into general location data. We thought we
might find insights about where treatment tends to occur. We firstly
group by location, get the count and plot the counts. We see there are
seven main locations with more than two hundred records. The rest of the
locations contain 25 or fewer records. For the sake of data readability,
we will filter out these sparse locations for understanding larger
trends. It is worth noting that most of the data is not
location-specific, including the telehealth video, phone, and where
there is no location. This data might not be useful for understanding
location trends but lets us know that many services are outside the HFS
office.

A lot of the general location data is telehealth - video. I assume this
means location does not matter.

#### Appointment No Shows

The column is\_noshow is interesting because these are costly events for
both HFS and for the potential benefactor. No\_shows consume HFS
appointment time and the person loses out on an opportunity for therapy.

    tibble_data <- as_tibble(data)
    # data header
    head(tibble_data$is_noshow)

    ## [1] FALSE FALSE FALSE  TRUE FALSE FALSE

    grouped_no_show <- tibble_data %>% group_by(is_noshow) %>% count(sort=TRUE)
    ggplot(grouped_no_show) + geom_point(mapping = aes(x = reorder(is_noshow,-n), y = n)) +
    ggtitle("Count of Rows for Show vs NoShow") +
    xlab("Appointment Show or No Show") +
    ylab("Count of Category") +
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), legend.position = "none")

![](ComprehensiveAnalysis_files/figure-markdown_strict/noshow_breakdown-1.png)

    # check for NAs
    tibble_data %>% filter(is_noshow == NA) %>% count()

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1     0

We see that 15% of all rows are no shows. 15% seems like a surprisingly
high number of appointment no shows for any organization. This metric is
worth looking into further. There are no NAs in the column or values we
want to filter.

#### Number of Appointments per Person

HFS has voiced an interest in the number of appointments and total
duration spent per patient. While duration length or the number of
appointments does not connotate to organizational success, they are
metrics that HFS reports to funders.

    tibble_data <- as_tibble(data)
    # data header
    head(tibble_data$recordID)

    ## [1] 298 338 338 338 338 338

    record_counts <- tibble_data %>% group_by(recordID) %>% count(sort=TRUE) %>% filter(n > 2)
    ggplot(record_counts) + geom_point(mapping = aes(x = reorder(recordID,-n), y = n)) +
    ggtitle("Plot of Repeated Record Count per Person") +
    xlab("Person's RecordId") +
    ylab("Count of Person's Appointments") +
      theme(axis.text.x=element_blank(),
            axis.ticks.x=element_blank())

![](ComprehensiveAnalysis_files/figure-markdown_strict/appointments-1.png)

There are only 460 records with more than one appointment with HFS,
which is only 5% of all HFS records. From this we learn that almost all
appointments are single-time appointments. Considering the few number of
records with multiple appointments, it might not be worth looking
further into the factors affecting duration or the number of
appointments.

#### Plot: Total Duration for each Therapist Job Title, Faceted by Program Name

The next goal is to visualize the relationship between job title,
duration, ethnicity, and the program. We’ll show a few plots below that
begin to explore this relationship. Firstly, we will look at job title
vs. duration across programs. Next, we’ll include ethnicity as a color,
but it doesn’t tell us much about ethnicity.

    ggplot(data = therapists) + geom_point(mapping = aes(x = job_title, y = total_duration_num, color=ethnic_identity)) +  facet_wrap(~ program_name, nrow = 2) +
    ggtitle("Total Appointment Duration for Each Therapist Job Title\nFacet by Program Name") +
    xlab("Job Title") +
    ylab("Duration (minutes)") + 
      ylim(0, 130) + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

    ## Warning: Removed 458 rows containing missing values (geom_point).

![](ComprehensiveAnalysis_files/figure-markdown_strict/therapist_type_vs_duration-1.png)

The primary observation is the distribution of appointments across both
program types and job titles. Therapist III only exists in their Mental
Health program. Gambling only has Therapist I and II. Gambling duration
is much lower and Mental Health appointment duration has a higher range.
Most importantly, we see differences in the duration range, variance,
and range by job title for each program. The implication of different
durations across job titles might indicate that “job title” affects the
duration of appointments or the type of appointments assigned to each
therapist job title.

It isn’t easy from the graph’s colors to distinguish ethnicity. Using
shape instead of color is even more challenging to read.

#### Plot: Duration vs. Job Title, Faceted by Program Name

    # therapist 1 is far less effective.  therapist is faster, possibly a different title?
    ggplot(data = therapists) + 
      stat_summary(
        mapping = aes(x = job_title, y = total_duration_num),
        #fun.min = min,
        #fun.max = max,
        fun = median
      ) + facet_grid(~ program_name) +
    ggtitle("Duration vs Job Title, Facet by Program Type") +
    xlab("Job Title") +
    ylab("Duration (minutes)") + 
      ylim(0, 130) + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

    ## Warning: Removed 458 rows containing non-finite values (stat_summary).

    ## Warning: Removed 2 rows containing missing values (geom_segment).

    ## Warning: Removed 4 rows containing missing values (geom_segment).

    ## Warning: Removed 3 rows containing missing values (geom_segment).

![](ComprehensiveAnalysis_files/figure-markdown_strict/job_title_vs_therapists-1.png)

    # we do get an equal or decrease in the mean time by experience.  What about the variance?  
    # therapist 3 are only seen in mental health
    # in substance abuse duration goes up from therapist 1 to 2, though it is lower than therapist and lead therapist.

#### Plot: Total Appointment Duration For Each Therapist Job Title, Faceted by Ethnicity, Colored for Location

    # but total_duration goes down with increase experience
    # I wonder if clients are more likely to show up in the future based on the previous experience who rendered the service.
    # durations do total together!
    # BEAUTIFUL, it shows the efficiency across types
    ggplot(data = therapists) + geom_point(mapping = aes(x = job_title, y = duration_num, color=facility)) + facet_wrap(~ ethnic_identity) +
    ggtitle("Total Appointment Duration for Each Therapist Job Title,\nFacet by Ethnicity, colored for Location") +
    xlab("Job Title") +
    ylab("Duration (minutes)") +
     ylim(0, 130) + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), legend.position = "none")

    ## Warning: Removed 2 rows containing missing values (geom_point).

![](ComprehensiveAnalysis_files/figure-markdown_strict/unnamed-chunk-12-1.png)

This graph is extremely complicated but also informative. Coloring by
location is informative and tells us where ethnicities and job titles
congregate. I hid the legend since I am less concerned with the actual
location and more concerned with the general area.

#### Plot: No show by Ethnicity

    noshow_stats <- therapists %>% group_by(ethnic_identity) %>% count(is_noshow) %>% filter(n > 10)
    noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
    shows <- noshow_stats %>% filter(is_noshow == "TRUE")
    noshow_stats_consolidated <- inner_join(shows,noshows,by=c("ethnic_identity"), suffix = c("no", "yes"))
    noshow_stats_consolidated <- select(noshow_stats_consolidated, -c(is_noshowno, is_noshowyes))
    noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
    df <- noshow_percent

    ggplot(data = df) + geom_point(mapping = aes(x = ethnic_identity, y = noshow_percent)) + coord_flip() +
    ggtitle("Average No Show Percent\n by Ethnicity") +
    xlab("Ethnicity") +
    ylab("Average Percentage of No Shows")

![](ComprehensiveAnalysis_files/figure-markdown_strict/noshow_ethnicity-1.png)

This shows that the groups with the highest no-show rate across all
programs looks like it is the Mexican ethnic identity. It is worthwhile
to break down ethnic identity by the program to understand if this
relationship still holds. If it does, this research can explore how one
might decrease the no show rate across the groups experiencing the
highest rates of no-shows.

#### Plot: Break down no show for ethnicity across program types

    noshow_stats <- therapists %>% group_by(ethnic_identity, program_name) %>% count(is_noshow) %>% filter(n > 10)
    noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
    shows <- noshow_stats %>% filter(is_noshow == "TRUE")
    noshow_stats_consolidated <- inner_join(shows,noshows,by=c("ethnic_identity","program_name"), suffix = c("no", "yes"))
    noshow_stats_consolidated <- select(noshow_stats_consolidated, -c(is_noshowno, is_noshowyes))
    noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
    df <- noshow_percent

    ggplot(data = df) + geom_point(mapping = aes(x = ethnic_identity, y = noshow_percent)) +  facet_wrap(~ program_name, nrow = 2) +
    ggtitle("No Show Percentage for Each Ethnicity\nFacet by Program Name") +
    xlab("Job Title") +
    ylab("Duration (minutes)") + 
      ylim(10, 25) + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

![](ComprehensiveAnalysis_files/figure-markdown_strict/ethnicity_by_program_noshows-1.png)

We see a similar dropout rate for both “Non-Spanish/Hispanic/Latino and
Other Hispanic or Latino” On the other hand, we see a far higher, even
double, no show rate for those who are identified as the Mexican ethnic
identity, which only exists in the “Mental Health” program type. This
research should explore the Mexican ethnic identity within the “Mental
Health Program.”

#### Plot: Mexican Dropout Percentage by Job Title within the Mental Health Program

We can then go one step further and see if we see a difference in drop
out, within the “Mental Health” program for the “Mexican” ethnic
identity.

    mexican_ethnic_identity <- therapists %>% filter(program_name == "Mental Health" & ethnic_identity == "Mexican") %>% group_by(ethnic_identity,job_title)  %>% count(is_noshow)
    noshow_stats <- mexican_ethnic_identity
    noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
    shows <- noshow_stats %>% filter(is_noshow == "TRUE")
    noshow_stats_consolidated <- inner_join(shows,noshows,by=c("ethnic_identity","job_title"), suffix = c("no", "yes"))
    noshow_stats_consolidated <- select(noshow_stats_consolidated, -c(is_noshowno, is_noshowyes))
    noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
    df <- noshow_percent

    ggplot(data = df) + geom_point(mapping = aes(x = job_title, y = noshow_percent)) +
    ggtitle("No Show Percentage for the Mexican Ethnic Identity\nfor the Mental Health Program by Therapist Job Title") +
    xlab("Job Title") +
    ylab("Duration (minutes)") + 
      ylim(20, 55) + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

![](ComprehensiveAnalysis_files/figure-markdown_strict/unnamed-chunk-14-1.png)

Now this looks AMAZING and like we’ve found something truly significant.
However, when we look at the data we find only eight instances of the
Mexican ethnic identity for Therapist II. It is possible this
relationship is significant but more data is needed to confirm this
phenomenon. Otherwise, no-show rate is mostly similar across all
ethnicities.

#### Plot: Average No Show Percent for Each Therapist Job Titleby Ethnicity, Faceted by Program Name

There is now an opportunity to look at no-show rate across therapist job
title, program name, and ethnicity. The graph separates the data into
facets for each program type and graphs no show percentage for each
ethnicity, colored for therapist job title.

    noshow_stats <- therapists %>% group_by(job_title) %>% count(is_noshow)
    noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
    shows <- noshow_stats %>% filter(is_noshow == "TRUE")
    noshow_stats_consolidated <- inner_join(shows,noshows,by=c("job_title"), suffix = c("no", "yes")) 
    noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
    noshow_percent

    ## # A tibble: 4 × 6
    ## # Groups:   job_title [4]
    ##   job_title      is_noshowno   nno is_noshowyes  nyes noshow_percent
    ##   <chr>          <lgl>       <int> <lgl>        <int>          <dbl>
    ## 1 LEAD THERAPIST TRUE           48 FALSE          284          14.5 
    ## 2 THERAPIST I    TRUE          649 FALSE         4301          13.1 
    ## 3 THERAPIST II   TRUE          280 FALSE         1162          19.4 
    ## 4 THERAPIST III  TRUE           13 FALSE          363           3.46

    # All shows and no-shows are under
    noshow_stats <- therapists %>% group_by(job_title, ethnic_identity, program_name) %>% count(is_noshow) %>% filter(n > 10)
    noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
    shows <- noshow_stats %>% filter(is_noshow == "TRUE")
    noshow_stats_consolidated <- inner_join(shows,noshows,by=c("job_title","ethnic_identity","program_name"), suffix = c("no", "yes"))
    noshow_stats_consolidated <- select(noshow_stats_consolidated, -c(is_noshowno, is_noshowyes))
    noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
    df <- noshow_percent

    ggplot(data = df) + geom_point(mapping = aes(x = ethnic_identity, y = noshow_percent, color = job_title)) + 
    facet_wrap(~ program_name, nrow = 2) +
    ggtitle("Average No Show Percent for Each Therapist Job Title\n by Ethnicity, Faceted by Program Name") +
    xlab("Ethnicity") +
    ylab("Average Percentage of No Shows\n for Each Therapist Job Title") + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

![](ComprehensiveAnalysis_files/figure-markdown_strict/noshows-1.png)

Noticeable takeaway is a general trend for more experienced job titles
to have a lower no-show percentage by graphed by ethnicity. You also
notice that some ethnicities are only treated by one level of therapist
by each program type. On average, mental health has lower no-show rates.

#### Simple Linear Model of No Show Percentage vs Job Title + Ethnicity + Program

Since we’ve created some insightful plots, it’s worth getting a feeling
of how strong the relationship is between no-show percentage and the
other variables we’ve investigated. Moreover, the residuals and standard
residuals are somewhat constant.

    score_model <- lm(df$noshow_percent ~ df$job_title + df$ethnic_identity + df$program_name, data = df)
    plot(score_model)

    ## Warning: not plotting observations with leverage one:
    ##   3, 4, 11

![](ComprehensiveAnalysis_files/figure-markdown_strict/linear_model_assumptions-1.png)![](ComprehensiveAnalysis_files/figure-markdown_strict/linear_model_assumptions-2.png)![](ComprehensiveAnalysis_files/figure-markdown_strict/linear_model_assumptions-3.png)![](ComprehensiveAnalysis_files/figure-markdown_strict/linear_model_assumptions-4.png)

    shapiro.test(df$noshow_percent)

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  df$noshow_percent
    ## W = 0.98644, p-value = 0.9915

From the four plots, it seems the distribution might be a good candidate
for a linear model. Most of the data is normal in the qqplot and all the
lines have a manageable leverage. Moreover, a Shapiro-Wilk’s test shows
the data is not significantly different from normal.

R allows us to create a linear model and investigate the properties
expected within a linear model.

    summary(score_model)

    ## 
    ## Call:
    ## lm(formula = df$noshow_percent ~ df$job_title + df$ethnic_identity + 
    ##     df$program_name, data = df)
    ## 
    ## Residuals:
    ##          1          2          3          4          5          6          7 
    ##  3.276e+00 -3.276e+00  1.110e-16  5.662e-15 -1.645e+00  1.645e+00 -4.401e+00 
    ##          8          9         10         11 
    ##  4.401e+00  2.770e+00 -2.770e+00 -3.331e-16 
    ## 
    ## Coefficients:
    ##                                               Estimate Std. Error t value
    ## (Intercept)                                     32.790     11.172   2.935
    ## df$job_titleTHERAPIST I                          1.620      5.195   0.312
    ## df$job_titleTHERAPIST II                         5.194      5.195   1.000
    ## df$job_titleTHERAPIST III                       -6.796      6.622  -1.026
    ## df$ethnic_identityNot Spanish/Hispanic/Latino   -9.410      6.622  -1.421
    ## df$ethnic_identityOther Hispanic or Latino      -4.413      6.622  -0.666
    ## df$program_nameMental Health                   -12.891      6.622  -1.947
    ## df$program_nameSubstance Use                    -2.819      6.622  -0.426
    ##                                               Pr(>|t|)  
    ## (Intercept)                                     0.0608 .
    ## df$job_titleTHERAPIST I                         0.7755  
    ## df$job_titleTHERAPIST II                        0.3911  
    ## df$job_titleTHERAPIST III                       0.3803  
    ## df$ethnic_identityNot Spanish/Hispanic/Latino   0.2504  
    ## df$ethnic_identityOther Hispanic or Latino      0.5528  
    ## df$program_nameMental Health                    0.1468  
    ## df$program_nameSubstance Use                    0.6990  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.195 on 3 degrees of freedom
    ## Multiple R-squared:  0.8677, Adjusted R-squared:  0.5588 
    ## F-statistic:  2.81 on 7 and 3 DF,  p-value: 0.2132

Most notable is no significant relationship between the no-show
percentage with other variables. However, both the mental health program
and most common ethnicity contained more extreme values. It gives us
feedback that no-show percentage is likely more complicated than job
title alone. However, I noticed that some facilities had much higher
no-show rates. I bet if I include facility it might change a lot in the
model.

#### Linear Model with Facility

This time we heavily filter to the main few facility locations and
include it in the linear model. Again, I only want a few main facilities
since too many locations will heavily bias the model toward
significance.

    main_facilities <- unique(therapists %>% group_by(facility) %>% count(is_noshow) %>% filter(n > 100))

    noshow_stats <- therapists %>% group_by(job_title, ethnic_identity, program_name, facility) %>% count(is_noshow)
    noshows <- noshow_stats %>% filter(is_noshow == "FALSE")
    shows <- noshow_stats %>% filter(is_noshow == "TRUE")
    noshow_stats_consolidated <- inner_join(shows,noshows,by=c("job_title","ethnic_identity","program_name","facility"), suffix = c("no", "yes"))
    noshow_stats_consolidated <- select(noshow_stats_consolidated, -c(is_noshowno, is_noshowyes))
    noshow_percent <- noshow_stats_consolidated %>% add_column(noshow_percent = .$nno / (.$nyes + .$nno) * 100)
    df <- noshow_percent
    # filter by major facilities
    df <- inner_join(df,main_facilities,by=c("facility"), suffix = c("no", "yes"))

Above, we already showed that noshow\_percent is not statistically
different from normal and the assumptions of normality are generally
held by the data. Below, we show the relationship between
noshow\_percent and other important varaibles like job title, ethnicity,
and facility.

    facility.model <- lm(df$noshow_percent ~ df$job_title + df$program_name + df$ethnic_identity + df$facility, data = df)
    summary(facility.model)

    ## 
    ## Call:
    ## lm(formula = df$noshow_percent ~ df$job_title + df$program_name + 
    ##     df$ethnic_identity + df$facility, data = df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -38.257  -4.866   0.000   2.013  45.101 
    ## 
    ## Coefficients:
    ##                                                               Estimate
    ## (Intercept)                                                     8.7273
    ## df$job_titleTHERAPIST I                                         7.2459
    ## df$job_titleTHERAPIST II                                        1.8148
    ## df$job_titleTHERAPIST III                                       0.6175
    ## df$program_nameMental Health                                   -0.6466
    ## df$program_nameSubstance Use                                    3.2795
    ## df$ethnic_identityNot Collected                               -16.9080
    ## df$ethnic_identityNot Spanish/Hispanic/Latino                  -8.8750
    ## df$ethnic_identityOther Hispanic or Latino                     -6.8720
    ## df$ethnic_identityUnknown                                       1.3476
    ## df$facilityCenter Mall Office                                   9.3548
    ## df$facilityHeartland Family Service - Central                  11.5814
    ## df$facilityHeartland Family Service - Child and Family Center   5.7646
    ## df$facilityHeartland Family Service - Gendler                  17.9018
    ## df$facilityHeartland Family Service - Sarpy                     5.1971
    ## df$facilityKreft Primary School                                -0.4687
    ## df$facilityLewis Central High School                            2.8817
    ## df$facilityMicah House                                         -3.0425
    ## df$facilityOmaha (Blondo) Reporting Center                     32.0213
    ## df$facilityOmaha (Spring) Reporting Center                     11.3578
    ## df$facilityWilson Middle School                                -2.1038
    ##                                                               Std. Error
    ## (Intercept)                                                      20.1963
    ## df$job_titleTHERAPIST I                                           6.3294
    ## df$job_titleTHERAPIST II                                          6.3801
    ## df$job_titleTHERAPIST III                                         9.1231
    ## df$program_nameMental Health                                     10.2171
    ## df$program_nameSubstance Use                                     10.0607
    ## df$ethnic_identityNot Collected                                  10.8717
    ## df$ethnic_identityNot Spanish/Hispanic/Latino                     6.2997
    ## df$ethnic_identityOther Hispanic or Latino                        6.4991
    ## df$ethnic_identityUnknown                                         9.3480
    ## df$facilityCenter Mall Office                                    14.1559
    ## df$facilityHeartland Family Service - Central                    13.4532
    ## df$facilityHeartland Family Service - Child and Family Center    16.0703
    ## df$facilityHeartland Family Service - Gendler                    13.6211
    ## df$facilityHeartland Family Service - Sarpy                      14.6105
    ## df$facilityKreft Primary School                                  18.0161
    ## df$facilityLewis Central High School                             18.0161
    ## df$facilityMicah House                                           18.0161
    ## df$facilityOmaha (Blondo) Reporting Center                       14.0348
    ## df$facilityOmaha (Spring) Reporting Center                       14.8505
    ## df$facilityWilson Middle School                                  18.0161
    ##                                                               t value Pr(>|t|)
    ## (Intercept)                                                     0.432   0.6675
    ## df$job_titleTHERAPIST I                                         1.145   0.2579
    ## df$job_titleTHERAPIST II                                        0.284   0.7773
    ## df$job_titleTHERAPIST III                                       0.068   0.9463
    ## df$program_nameMental Health                                   -0.063   0.9498
    ## df$program_nameSubstance Use                                    0.326   0.7458
    ## df$ethnic_identityNot Collected                                -1.555   0.1263
    ## df$ethnic_identityNot Spanish/Hispanic/Latino                  -1.409   0.1652
    ## df$ethnic_identityOther Hispanic or Latino                     -1.057   0.2955
    ## df$ethnic_identityUnknown                                       0.144   0.8860
    ## df$facilityCenter Mall Office                                   0.661   0.5118
    ## df$facilityHeartland Family Service - Central                   0.861   0.3935
    ## df$facilityHeartland Family Service - Child and Family Center   0.359   0.7214
    ## df$facilityHeartland Family Service - Gendler                   1.314   0.1949
    ## df$facilityHeartland Family Service - Sarpy                     0.356   0.7236
    ## df$facilityKreft Primary School                                -0.026   0.9794
    ## df$facilityLewis Central High School                            0.160   0.8736
    ## df$facilityMicah House                                         -0.169   0.8666
    ## df$facilityOmaha (Blondo) Reporting Center                      2.282   0.0269
    ## df$facilityOmaha (Spring) Reporting Center                      0.765   0.4481
    ## df$facilityWilson Middle School                                -0.117   0.9075
    ##                                                                
    ## (Intercept)                                                    
    ## df$job_titleTHERAPIST I                                        
    ## df$job_titleTHERAPIST II                                       
    ## df$job_titleTHERAPIST III                                      
    ## df$program_nameMental Health                                   
    ## df$program_nameSubstance Use                                   
    ## df$ethnic_identityNot Collected                                
    ## df$ethnic_identityNot Spanish/Hispanic/Latino                  
    ## df$ethnic_identityOther Hispanic or Latino                     
    ## df$ethnic_identityUnknown                                      
    ## df$facilityCenter Mall Office                                  
    ## df$facilityHeartland Family Service - Central                  
    ## df$facilityHeartland Family Service - Child and Family Center  
    ## df$facilityHeartland Family Service - Gendler                  
    ## df$facilityHeartland Family Service - Sarpy                    
    ## df$facilityKreft Primary School                                
    ## df$facilityLewis Central High School                           
    ## df$facilityMicah House                                         
    ## df$facilityOmaha (Blondo) Reporting Center                    *
    ## df$facilityOmaha (Spring) Reporting Center                     
    ## df$facilityWilson Middle School                                
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.74 on 49 degrees of freedom
    ## Multiple R-squared:  0.4615, Adjusted R-squared:  0.2418 
    ## F-statistic:   2.1 on 20 and 49 DF,  p-value: 0.01788

What I’m most interested in is the relationship between the main
variables by facility. We then find a relationship between Therapist I,
ethnicity, and a few locations, which might be insightful since one
might expect more no-shows with a newer therapist. Also, we might expect
that certain locations are less able to handle certain ethnicities, such
as a lack of support for Spanish. However, depending on the facility
filter I use the stats change, which makes me think that some of the
significance is due to the sheer number of facilities. Further work
should categorize facilities based on size or primary ethnicities and
try to remodel with fewer variables.

### Integrating RQ 1 and RQ2

#### Looking at Day Lapse from Signup to Appointment by Facility

Sai did a lot of useful work concerning the date on which appointment
data is entered. I thought it would be interesting to explore whether
this data looks the same across all the primary facilities for HFS. If
the data varies, maybe we can glean differences in the busyness of
facilities due to needing to create appointments further in the future.
From these observations, we might be able to better understand which
facilities are more difficult for customers to use.

    average_lapse <- HFS_data %>% select(facility, AD) %>% group_by(facility) %>% drop_na() %>% summarise(mean = mean(AD)) %>% arrange(mean)

    filter_to_large_facilities <- inner_join(average_lapse,main_facilities,by=c("facility"), suffix = c(".1", ".2"))

    plot(filter_to_large_facilities$mean,xlab = "Facility, Ordered by Mean", ylab = "Day Lapse from Data Entry to Appointment", main = "Plot of Day Lapse to Appointments by Facility")

![](ComprehensiveAnalysis_files/figure-markdown_strict/appointment_entry_lapse_by_facility-1.png)

From the above plot we can see that HFS is fairly efficient at getting
customers registered and into appointments for all of their facilities
that handled at least 100 customers in the dataset. Given the discovered
efficiency for HFS when processing new persons, it might be not be worth
digging deeper into this lapse between signup to appointment day.

#### Looking at Timestamp Volumes per Facility, no Facet

If we look across all facilities, we can see how the HFS volumes
increase over time. This is particularly noticeable in 2021, where HFS
volume substantially increased.

    facility_timestamp_count <- HFS_data %>% select(facility, AD) %>% group_by(facility, AD) %>% drop_na() %>% count(sort=TRUE) %>% arrange(n)

    ggplot(facility_timestamp_count, aes(y =facility_timestamp_count$AD, x =facility_timestamp_count$n)) + 
      geom_point() +
    ggtitle("Daily Facility Volume per Day") +
    xlab("Date Time") +
    ylab("Count of Appointments per Day at the Facility") + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

![](ComprehensiveAnalysis_files/figure-markdown_strict/timestamp_appointment_entry_lapse_by_facility-1.png)

#### Looking at Timestamp Volumes by Facility, Facet by Facility

    facility_timestamp_count <- HFS_data %>% select(facility, AD) %>% group_by(facility, AD) %>% drop_na() %>% count(sort=TRUE) %>% arrange(n)

    filter_to_large_facilities <- inner_join(facility_timestamp_count,main_facilities,by=c("facility"), suffix = c(".1", ".2"))

    sorted <- filter_to_large_facilities %>% group_by(facility) %>% summarise(mean = mean(n.2)) %>% arrange(mean)

    with_averages <- inner_join(sorted,filter_to_large_facilities,by=c("facility"), suffix = c(".1", ".2"))

    ggplot(filter_to_large_facilities, aes(y =filter_to_large_facilities$AD, x=filter_to_large_facilities$n.1)) + 
      geom_point() +
    ggtitle("Facility Volume per Day, Facet by Facility") +
    xlab("Date Time") +
    ylab("Count of Appointments per Day at the Facility") + 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))+
      geom_violin()+
      facet_wrap(~filter_to_large_facilities$facility, labeller = labeller(groupwrap = label_wrap_gen(5)))

![](ComprehensiveAnalysis_files/figure-markdown_strict/timestamp_appointment_entry_lapse_by_facility_facet-1.png)

The goal of the above graph is to visualize how volume has changed over
time at the primary facilities for HFS. With this knowledge, our
research can better understand what HFS growth has looked like at its
primary facilities over the past few years. As is expected, most
facilities have a constant or slight increase in volume over time.
However, Some facilities like HFS - C. Have had substantial growth,
maybe 5x growth since 2020. However, most facilities show a slow growth
in daily appointments over time.

Per future analysis, from this information it might be useful to
calculate facility capacity as a function of therapist count per
facility.

#### RQ 3

### RQ Rhonda

Of the clients receiving services that identified as Hispanic or
Mexican, how many received services at which branch

### Dataset Used

    HFS_data<-read.csv("HFS Service Data.csv")
    HFS.Ethnic_Identity<-HFS_data

    str(HFS.Ethnic_Identity)

    ## 'data.frame':    8745 obs. of  51 variables:
    ##  $ gender                  : chr  "Male" "Female" "Female" "Female" ...
    ##  $ program_name            : chr  "Mental Health" "Mental Health" "Mental Health" "Mental Health" ...
    ##  $ program_type            : chr  "Counseling and Prevention" "Counseling and Prevention" "Counseling and Prevention" "Counseling and Prevention" ...
    ##  $ facility                : chr  "Heartland Family Service - Logan" "Center Mall Office" "Center Mall Office" "Center Mall Office" ...
    ##  $ job_title               : chr  "Clinical Supervisor" "THERAPIST II" "THERAPIST II" "THERAPIST II" ...
    ##  $ staff_name              : chr  "Poore, Lindsay" "Carlson, Kaitlin" "Carlson, Kaitlin" "Carlson, Kaitlin" ...
    ##  $ actual_date             : int  961 857 682 710 696 772 794 864 661 737 ...
    ##  $ event_name              : chr  "Daily Living Assessment DLA 20" "Collateral Note" "Individual Therapy" "Individual Therapy" ...
    ##  $ activity_type           : chr  "" "Phone" "" "" ...
    ##  $ encounter_with          : chr  "" "Client" "" "" ...
    ##  $ is_client_involved      : logi  TRUE TRUE TRUE TRUE TRUE TRUE ...
    ##  $ is_noshow               : logi  FALSE FALSE FALSE TRUE FALSE FALSE ...
    ##  $ is_locked               : logi  FALSE TRUE TRUE TRUE TRUE TRUE ...
    ##  $ is_billed               : logi  FALSE FALSE TRUE FALSE TRUE FALSE ...
    ##  $ is_paid                 : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ date_entered            : int  961 857 683 710 696 773 795 864 661 739 ...
    ##  $ user_entered_name       : chr  "Poore, Lindsay" "Carlson, Kaitlin V." "Carlson, Kaitlin V." "Carlson, Kaitlin V." ...
    ##  $ approved_date           : int  NA 857 689 714 697 773 795 864 662 739 ...
    ##  $ approved_staff_name     : chr  "" "Carlson, Kaitlin V." "Stanek, Sean" "Stanek, Sean" ...
    ##  $ submitted               : chr  "" "Approved" "Approved" "Approved" ...
    ##  $ is_approved             : int  0 1 1 1 1 1 1 1 1 1 ...
    ##  $ is_notapproved          : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ is_notapproved_subm     : int  1 0 0 0 0 0 0 0 0 0 ...
    ##  $ program_unit_description: chr  "Behavioral Health IA -  Mental Health" "Behavioral Health NE - Mental Health" "Behavioral Health NE - Mental Health" "Behavioral Health NE - Mental Health" ...
    ##  $ sc_code                 : chr  "1311-16" "1311-05" "1311-05" "1311-05" ...
    ##  $ duration_num            : int  0 2 51 0 50 80 4 2 52 115 ...
    ##  $ do_not_bill             : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ do_not_pay              : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ general_location        : chr  "" "Homeless Shelter" "Home" "Telehealth - Phone" ...
    ##  $ program_modifier        : chr  "No Modifier - IA" "Heartland Housing Navigation" "Heartland Housing Navigation" "Heartland Housing Navigation" ...
    ##  $ program_modifier_code   : chr  "NMODI" "HHN" "HHN" "HHN" ...
    ##  $ NormalWorkHours         : chr  "Yes" "Yes" "Yes" "Yes" ...
    ##  $ duration_other_num      : int  0 0 10 0 10 10 0 0 10 10 ...
    ##  $ duration_other          : chr  "0:00" "0:00" "0:10" "0:00" ...
    ##  $ travel_time_num         : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ travel_time             : chr  "0:00" "0:00" "0:00" "0:00" ...
    ##  $ planning_time_num       : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ planning_time           : chr  "0:00" "0:00" "0:00" "0:00" ...
    ##  $ total_duration_num      : int  0 2 61 0 60 90 4 2 62 125 ...
    ##  $ total_duration          : chr  "0:00" "0:02" "1:01" "0:00" ...
    ##  $ reason_for_no_show      : chr  "" "" "" "Client No Show - No Call" ...
    ##  $ is_billable             : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ zip                     : int  0 681 681 681 681 681 681 681 681 681 ...
    ##  $ state                   : chr  "IA" "NE" "NE" "NE" ...
    ##  $ age                     : int  12 26 25 25 25 25 25 26 25 25 ...
    ##  $ duration                : chr  "0:00" "0:02" "0:51" "0:00" ...
    ##  $ recordID                : int  298 338 338 338 338 338 338 338 338 338 ...
    ##  $ simple_race             : int  8 16 16 16 16 16 16 16 16 16 ...
    ##  $ ethnic_identity         : chr  "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino" ...
    ##  $ gender_identity         : chr  "Not Obtained" NA NA NA ...
    ##  $ sexual_orientation      : chr  "Not Obtained" NA NA NA ...

### To see the names of Columns

    names(HFS.Ethnic_Identity)

    ##  [1] "gender"                   "program_name"            
    ##  [3] "program_type"             "facility"                
    ##  [5] "job_title"                "staff_name"              
    ##  [7] "actual_date"              "event_name"              
    ##  [9] "activity_type"            "encounter_with"          
    ## [11] "is_client_involved"       "is_noshow"               
    ## [13] "is_locked"                "is_billed"               
    ## [15] "is_paid"                  "date_entered"            
    ## [17] "user_entered_name"        "approved_date"           
    ## [19] "approved_staff_name"      "submitted"               
    ## [21] "is_approved"              "is_notapproved"          
    ## [23] "is_notapproved_subm"      "program_unit_description"
    ## [25] "sc_code"                  "duration_num"            
    ## [27] "do_not_bill"              "do_not_pay"              
    ## [29] "general_location"         "program_modifier"        
    ## [31] "program_modifier_code"    "NormalWorkHours"         
    ## [33] "duration_other_num"       "duration_other"          
    ## [35] "travel_time_num"          "travel_time"             
    ## [37] "planning_time_num"        "planning_time"           
    ## [39] "total_duration_num"       "total_duration"          
    ## [41] "reason_for_no_show"       "is_billable"             
    ## [43] "zip"                      "state"                   
    ## [45] "age"                      "duration"                
    ## [47] "recordID"                 "simple_race"             
    ## [49] "ethnic_identity"          "gender_identity"         
    ## [51] "sexual_orientation"

### Deleted Columns that I didn’t need and named it HFS.Hispanic.cleaned

    HFS.Ethnicity2 <- HFS.Ethnic_Identity %>% select(c(1:4,7,9,17,24,25,30,46,48,49:51))

    names(HFS.Ethnicity2)

    ##  [1] "gender"                   "program_name"            
    ##  [3] "program_type"             "facility"                
    ##  [5] "actual_date"              "activity_type"           
    ##  [7] "user_entered_name"        "program_unit_description"
    ##  [9] "sc_code"                  "program_modifier"        
    ## [11] "duration"                 "simple_race"             
    ## [13] "ethnic_identity"          "gender_identity"         
    ## [15] "sexual_orientation"

“program\_name”  
“facility”  
“program\_unit\_description”  
“age” “ethnic\_identity”

\*\* I now have 5 Variables (columns) & 8745 rows

\#\#\# apply count function count (df, ethnic\_identity) \#\#\# RESULTS
1 Mexican 148 2 Not Collected 165 3 Not Spanish/Hispanic/Latino 7820 4
Other Hispanic or Latino 471 5 Unknown 141

\#\#\#Clean NA & “Not Collected” \#\#\#Find missing values

    #is.na(HFS.Ethnic_identity.cleaned) # too long to show

\#\#\#Delete Rows that have “Not Collected” in variable Ethnic\_Identity

    HFS.Ethnicity2 = HFS.Ethnic_Identity[HFS.Ethnic_Identity$ethnic_identity  != "Not Collected",]

\#\#\#Replace “Not Spanish/Hispanic/Latino” with “Not Latino”

    HFS.Ethnicity2$ethnic_identity[HFS.Ethnicity2$ethnic_identity== "Not Spanish/Hispanic/Latino"]<- "Not Latino"

\#\#\#Replace “Other Hispanic or Latino” with “Latino”

    HFS.Ethnicity2$ethnic_identity[HFS.Ethnicity2$ethnic_identity== "Other Hispanic or Latino"]<- "Latino"

# Bar chart of Ethnicity & Facility

This BarChart shows us that the majority of Latinos served attend the
North Omaha Campus and the Heartland Family Service-Central location.

    ggplot(HFS.Ethnicity2) +
      aes(x = facility, fill = ethnic_identity)+
      labs(fill = "Ethnicity")+
      geom_bar() +
      scale_fill_hue(direction = 1) +
      coord_flip() +
      theme_minimal() +
      theme(axis.title.x = element_text(size = 9L))

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/Final%20Git%20Repository%20Data%20to%20Decisions/BarChartStackedFacility_Ethnicity.jpeg)<!-- -->

This BoxPlot identifies that most Latinos receive services for Mental
Health Programs and are between the ages of 18 and 50.

    ggplot(HFS.Ethnicity2) +
      aes(x = program_unit_description, y = age, fill = ethnic_identity) +
      geom_boxplot(width=0.5, lwd=1) +
      scale_color_hue(direction = 1) +
      ylab("Age") + xlab("Program Unit")+
      labs(fill = "Ethnicity") +
      labs (title = "Ethnicity by Age & Program Unit") +
      coord_flip() +
      theme_minimal()

![](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/Final%20Git%20Repository%20Data%20to%20Decisions/BoxPlot-updated.jpeg)<!-- -->

# Thing explored:

We have explored many different types of attributes that are required to
solve the 3 research questions and also we analysed the data that we
have now can also get other results.Further We can subset each and every
Character data and get additional information.

# Results

All the Data is perfectly cleaned and analysed. We can get clear and
beautiful plots or results from the cleaned data.
