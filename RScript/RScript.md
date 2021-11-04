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

Staff can hand-craft an integrated, multi-service, trauma-informed
strategy to help clients toward safety, well-being, and, ultimately,
self-sufficiency, thanks to their programs and services. A sliding
pricing scale is available for some of their counseling services.

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

### Rearch Questions Over View:

#### RQ 1

**My Research Question is to tell that how much time of delay it take to
takes for enrolling into the event, Entering into system and Approving
the enrollment.** **(Sai Krishna)**

Reason:- This will help in analyzing the time taking per enrolling from
starting to ending state. And what kind of people are enrolling on the
day of the even or before the event and total time taken for individual
event. This will help the HFS to improve the speed of enrollment and
also can increate the event of that type in that location.

### Data cleaning:

I have created a new data set based on attributes that I choice from
Dataset to analyze my desired Research Question. Attributes are
mentioned bellow.

`facility`, `actual_date`, `event_name`, `date_entered`,
`approved_date`, `program_unit_description`, `zip`,
`state`,`ethnic_identity`

#### In the new data set there 3 variables that talks about the dates in numeric form.

`actual_date` is about the actual data of the program admission.

`date_entered` is about when was the service entered (days after
enrollment or days before enrollment)

`approved_date` is about when was the service documentation approved by
a supervisor? (Days after enrollment)

#### Other Attributes:

`event_name` is about the name of the event.

`program_unit_description` is about the event description

`zip` Zip of the location

`state` Which location is the

`ethnic_identity` this is about the people of different groups who
attend the events

### Cleaning procidure :-

**step 1:-** Since i am using Data format so i have to find the
difference between each and every even that is been taken place. So, i
have used as.Date() function to get the actual date format.

**step 2:-** Apply step1 to all the date column to verify it in next
phase.

**step 3:-** Now create a new column add subtract the dates if we get
the values in negatives then it is enroled before the event,if 0 then
they resisted at the moment and if greater the 0 then it is after the
event.

**step 4:-** Later total we have 5 states in the data set which is
mentioned in short form

**step 5:-** Subset each and every State are in short form like
`IA`,`NE`,`CO`,`NC`,`SC`and later updated to full form of user
understanding `iowa`, `Nebrska` `colorado`,
`north carolina`,`south carolina`

and we can observe many different plots based on state and there zip
code.

    HFS_data<-read.csv("HFS Service Data.csv")
    #before cleasing the Na values
    #head(HFS_data)

Libraries used

    library('ggplot2')
    #library('')

As the Information for the sources of the data that is fatched out to
csv on **25th August 2021**.

By using this date i have created original data of the events. with the
help of **as.Date()** function in R

    HFS_data$AD<-as.Date(-(HFS_data$actual_date), origin = '2021-08-25')
    HFS_data$ED<-as.Date(-(HFS_data$date_entered), origin = '2021-08-25')
    HFS_data$Date_of_approved_date<-as.Date(-(HFS_data$approved_date), origin = '2021-08-25')

    HFS_data$AD_1<-(HFS_data$actual_date-HFS_data$date_entered)
    #str(HFS_data)

### Cleaning State Names:

We have a total of five states

    regions<-c(unique(HFS_data$state))
    regions

    ## [1] "IA" "NE" "CO" "NC" "SC"

Now we update all the states name into full form and them into the
dataset for easy understanding.

    HFS_data$state<- as.character(HFS_data$state)
    HFS_data$state[HFS_data$state == "NE"] <- "nebrska"
    HFS_data$state[HFS_data$state == "IA"] <- "iowa"
    HFS_data$state[HFS_data$state == "SC"] <- "south carolina"
    HFS_data$state[HFS_data$state == "NC"] <- "north carolina"
    HFS_data$state[HFS_data$state == "CO"] <- "colorado"

After Changing the Short form of the state:-

    c(unique(HFS_data$state))

    ## [1] "iowa"           "nebrska"        "colorado"       "north carolina"
    ## [5] "south carolina"

# Speliting the individual data accoring to the region:-

# IA(iowa):

    IA<- subset(HFS_data,state=='iowa')
    #str(IA)
    p <- ggplot(data = IA, aes(y =IA$AD, x =IA$AD_1,color=ethnic_identity ))
    p + geom_point()+geom_violin() +facet_wrap(IA$zip~IA$program_name,scales='free')

![](RScript_files/figure-markdown_strict/unnamed-chunk-7-1.png) In the
above graph you can understand that most of the people who from Iowa
joining events are **Not Spanish/Hispanic/Latino** most of them are
opting Gambling where as other ethnic\_identity groups are attending the
events like **Mental Health** and **Substance Use** and most of them are
from Zip code 0. \#\#\# NE(nebraska)

    NE<- subset(HFS_data,state=='nebrska')
    #head(NE)
    p <- ggplot(data = NE, aes(y =NE$AD, x =NE$AD_1,color=ethnic_identity ))
    p + geom_point() +geom_violin()+facet_wrap(zip~program_name,scales='free')

![](RScript_files/figure-markdown_strict/unnamed-chunk-8-1.png) In this
graph we can under stand how the date \#\#\# CO(colorado)

    CO<- subset(HFS_data,state=='colorado')
    #head(CO)
    p <- ggplot(data = CO, aes(y =CO$AD, x =CO$AD_1,color=ethnic_identity ))
    p + geom_point() +geom_violin()+facet_wrap(~zip,scales='free')

![](RScript_files/figure-markdown_strict/unnamed-chunk-9-1.png)

### NC(north carolina)

    NC<- subset(HFS_data,state=='north carolina')
    #head(NC)
    p <- ggplot(data = NC, aes(y =NC$AD, x =NC$AD_1,color=ethnic_identity ))
    p + geom_point() +geom_violin()+facet_wrap(~zip,scales='free')

![](RScript_files/figure-markdown_strict/unnamed-chunk-10-1.png)

### SC(south carolina)

    SC<- subset(HFS_data,state=='south carolina')
    #head(SC)
    p <- ggplot(data = SC, aes(y =SC$AD, x =SC$AD_1,color=ethnic_identity ))
    p + geom_point() +geom_violin()+facet_wrap(~zip,scales='free')

![](RScript_files/figure-markdown_strict/unnamed-chunk-11-1.png)

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

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library('tidyverse')

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ tibble  3.1.4     ✓ purrr   0.3.4
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

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

![](RScript_files/figure-markdown_strict/job_title_all-1.png)

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

![](RScript_files/figure-markdown_strict/therapists_vs_duration-1.png)

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

![](RScript_files/figure-markdown_strict/ethnic_plot-1.png)

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

![](RScript_files/figure-markdown_strict/facility_breakdown-1.png)

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

![](RScript_files/figure-markdown_strict/location-1.png)

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

![](RScript_files/figure-markdown_strict/general_location-1.png)

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

![](RScript_files/figure-markdown_strict/noshow_breakdown-1.png)

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

![](RScript_files/figure-markdown_strict/appointments-1.png)

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

![](RScript_files/figure-markdown_strict/therapist_type_vs_duration-1.png)

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

![](RScript_files/figure-markdown_strict/job_title_vs_therapists-1.png)

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

![](RScript_files/figure-markdown_strict/unnamed-chunk-12-1.png)

This graph is extremely complicated but also informative. Coloring by
location is informative and tells us where ethnicities and job titles
congregate. I hid the legend since I am less concerned with the actual
location and more concerned with the general area.

# Plot: No show by Ethnicity

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

![](RScript_files/figure-markdown_strict/noshow_ethnicity-1.png)

This shows that the groups with the highest no-show rate across all
programs looks like it is the Mexican ethnic identity. It is worthwhile
to break down ethnic identity by the program to understand if this
relationship still holds. If it does, this research can explore how one
might decrease the no show rate across the groups experiencing the
highest rates of no-shows.

# Plot: Break down no show for ethnicity across program types

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

![](RScript_files/figure-markdown_strict/ethnicity_by_program_noshows-1.png)

We see a similar dropout rate for both “Non-Spanish/Hispanic/Latino and
Other Hispanic or Latino” On the other hand, we see a far higher, even
double, no show rate for those who are identified as the Mexican ethnic
identity, which only exists in the “Mental Health” program type. This
research should explore the Mexican ethnic identity within the “Mental
Health Program.”

# Plot: Mexican Dropout Percentage by Job Title within the Mental Health Program

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

![](RScript_files/figure-markdown_strict/unnamed-chunk-14-1.png)

Now this looks AMAZING and like we’ve found something truly significant.
However, when we look at the data we find only eight instances of the
Mexican ethnic identity for Therapist II. It is possible this
relationship is significant but more data is needed to confirm this
phenomenon. Otherwise, no-show rate is mostly similar across all
ethnicities.

# Plot: Average No Show Percent for Each Therapist Job Titleby Ethnicity, Faceted by Program Name

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

![](RScript_files/figure-markdown_strict/noshows-1.png)

Noticeable takeaway is a general trend for more experienced job titles
to have a lower no-show percentage by graphed by ethnicity. You also
notice that some ethnicities are only treated by one level of therapist
by each program type. On average, mental health has lower no-show rates.

# Simple Linear Model of No Show Percentage vs Job Title + Ethnicity + Program

Since we’ve created some insightful plots, it’s worth getting a feeling
of how strong the relationship is between no-show percentage and the
other variables we’ve investigated.

    score_model <- lm(df$noshow_percent ~ df$job_title + df$ethnic_identity + df$program_name, data = df)
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

# Linear Model with Facility

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

#### RQ 3

### RQ Rhonda

Of the clients receiving services that identified as Hispanic or
Mexican, how many received services at which branch

### Dataset Used

### To see the names of Columns

gender, program\_name, program\_type, facility, job\_title, staff\_name,
actual\_date, event\_name, activity\_type, encounter\_with,
is\_client\_involved, is\_noshow, is\_locked, is\_billed, is\_paid,
date\_entered, user\_entered\_name, approved\_date,
approved\_staff\_name, submitted, is\_approved, is\_notapproved,
is\_notapproved\_subm, program\_unit\_description, sc\_code,
duration\_num, do\_not\_bill, do\_not\_pay, general\_location,
program\_modifier, program\_modifier\_code, NormalWorkHours,
duration\_other\_num, duration\_other, travel\_time\_num, travel\_time,
planning\_time\_num, planning\_time, total\_duration\_num,
total\_duration, reason\_for\_no\_show, is\_billable, zip, state, age,
duration, recordID, simple\_race, ethnic\_identity, gender\_identity,
sexual\_orientation, AD, ED, Date\_of\_approved\_date, AD\_1

### Deleted Columns that I didn’t need and named it HFS.Hispanic.cleaned

gender, program\_name, program\_type, facility, actual\_date,
activity\_type, user\_entered\_name, program\_unit\_description,
sc\_code, program\_modifier, duration, simple\_race, ethnic\_identity,
gender\_identity, sexual\_orientation

“gender” “program\_name” “program\_type”  
“facility” “actual\_date” “event\_name”  
“date\_entered” “program\_unit\_description” “general\_location”  
“age” “simple\_race” “ethnic\_identity”  
“gender\_identity” “sexual\_orientation”

\*\* I now have 14 columns & 8745 rows

    ncol(HFS.Ethnic_identity.cleaned)

    ## [1] 15

    nrow(HFS.Ethnic_identity.cleaned)

    ## [1] 8745

14

8745

\#\#\# apply count function count (df, ethnic\_identity) \#\#\# RESULTS
1 Mexican 148 2 Not Collected 165 3 Not Spanish/Hispanic/Latino 7820 4
Other Hispanic or Latino 471 5 Unknown 141

\#\#\#Clean NA & “Not Collected” \#\#\#Find missing values
`r`is.na(HFS.Ethnic\_identity.cleaned)\`

### Count missing values

1572 \#\#\# Result is 2,608

\#\#\#Delete Rows that have “Not Collected” in variable Ethnic\_Identity

    HFS.Ethnic_Identity.cleaned = HFS.Ethnic_Identity[HFS.Ethnic_Identity$ethnic_identity  != "Not Collected",]

    ncol(HFS.Ethnic_Identity.cleaned);nrow(HFS.Ethnic_Identity.cleaned)

    ## [1] 55

    ## [1] 8580

\[1\] 10 \[1\] 8580

\#\#\#Replace “Not Spanish/Hispanic/Latino” with “Not Latino”
`r`HFS.Ethnic\_Identity.cleaned*e**t**h**n**i**c*<sub>*i*</sub>*d**e**n**t**i**t**y*\[*H**F**S*.*E**t**h**n**i**c*<sub>*I*</sub>*d**e**n**t**i**t**y*.*c**l**e**a**n**e**d*ethnic\_identity==
“Not Spanish/Hispanic/Latino”\]&lt;- “Not Latino”\`

\#\#\#Replace “Other Hispanic or Latino” with “Other Latino”

### New Count of Ethnic Identity

1 Mexican 148 2 Not Latino 7820 3 Other Latino 471 4 Unknown 141

1.  **One scatter plot with three variables, properly labeled; choose
    your representation of the third variable based on what’s best for
    representing the data.**  

-   Three used variables are:
    -   ethnic\_identity
    -   facility  
    -   Adding a third variable in geom point using color=program\_name

<!-- -->

    ggplot(data = HFS.Ethnic_identity.cleaned, aes(x =ethnic_identity , y =program_unit_description , colour=program_name)) +geom_point(size = 3)+
         labs(title = "Scatter plot with three variables", y = "Unit Description of the Program", x = "Ethnicity")+ 
      theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))

![](RScript_files/figure-markdown_strict/unnamed-chunk-17-1.png)
[](https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/Data%20Cleaning%20Documentation/GIT_Data-Cleaning_files/figure-gfm/Scatter_Plot.jpeg)
The above scatter plot is composed of three variables: ethnic\_identity,
program\_unit description and program\_name. from this plot , we can say
that people have mostly participated in the mental health program. The
second major program is for the substance use program. The plot also
describes that NON HISPANIC clients are most likely to be treated in the
programs.

1.  **One faceted plot of two variables, properly labeled.**  

-   Two used variables are:
    -   ethnic\_identity
    -   facility

    <table>
    <thead>
    <tr class="header">
    <th><a href="https://github.com/saikrishnags05/Project-for-Data-to-Decisions/blob/master/Data%20Cleaning%20Documentation/GIT_Data-Cleaning_files/figure-gfm/Faceted.jpeg">Faceted Chart</a></th>
    </tr>
    </thead>
    <tbody>
    </tbody>
    </table>

    ggplot(data = HFS.Ethnic\_Identity.cleaned,
    aes(ethnic\_identity,facility)) +

-   geom_line(color = "steelblue", size = 1) +

-   geom_point(color = "steelblue") +

-   labs(title = "Faceted plot of two variables Ethnic Identity vs Facility", x = "ethnic_identity", y = "facility") +

-   facet_wrap(.~program_name)

I have divided the plot using the ethnic\_identity of the clients. This
plot uses two variables Ethnic Identity and Facility. We can say from
the plot that due to uncollected data or unknown/missing values there
are more separations in the plot. However , with the data in hand, the
created plot shows the differences of programs being served from the
various facilities accross various ethnicities.

1.  **One bar chart, properly labeled.**  

-   Used variable is:
    -   ethnic Identity

<!-- -->

    ggplot(data = HFS.Ethnic_Identity.cleaned)+
        geom_bar(mapping = aes(x=ethnic_identity),colour="white",fill="blue")+
        labs(title = "One bar chart of Ethnic Identity", y = "Count of Clients of Each Ethnicity", x = "Client Ethnicity")

![](RScript_files/figure-markdown_strict/unnamed-chunk-18-1.png) | ![Bar
Chart](https://raw.githubusercontent.com/saikrishnags05/Project-for-Data-to-Decisions/master/Data%20Cleaning%20Documentation/GIT_Data-Cleaning_files/figure-gfm/Bar_Chart.jpeg)
| - This Bar chart is composed of only the Ethnicity of the Client of
HFS. It shows how many clients are in each ethnicity group. The charts
shows that most clients of HFS are Not Latino. Also we can observe that
out of almost 8000 clients approximately 400 clients are Other Latino or
Unknown

# Thing explored:

We have explored many different types of attributes that are required to
solve the 3 research questions and also we analysed the data that we
have now can also get other results.Further We can subset each and every
Character data and get additional information.

# Results

All the Data is perfectly cleaned and analysed. We can get clear and
beautiful plots or results from the cleaned data.
