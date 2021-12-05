# Written Summary

By Sai Krishna Gaduputi Subbammagari, Chad Crowe, and Rhonda Silva

# Overview

## Abstract

HFS comprises multiple facilities between Iowa and Nebraska that exhibit different aspects of customer behavior related to 1. enrollment, missed appointments, and the ethnicity of those appointments. Our group takes a deep dive into facility information concerning their enrollment process, the rate of missed appointments, and ethnicity information. We find the enrollment delays, missed appointments, and Latino ethnicity varies dramatically, based on the facility. Our study makes recommendations based on these findings and how HFS can investigate these facility differences to improve the user experience and organization's efficiency.

## Research Questions

### Research Question #1
Do significant delays exist between enrolling in an event, entering
into the system, and the enrollment approval?

### The Decisions your Analysis Targets Could Help Support

It will help the HFS to know the average time taken for a person to
complete the full process for any event that is organized in different
facility till date. So that If they find any delay in enrolling, then
they can take action on the particular location and improve the
approving process.

### Research Question #2
Do HFS facility locations have a significant effect on the number of
missed appointments?

### The Decisions your Analysis Targets Could Help Support

There might be unknown but significant reasons why particular locations
have higher rates of missing appointments, such as issues with the facility
accessibility or local financial burdens like access to transportation.

The second research question concerns exploring the job role of
therapists within HFS, with a specific interest in appointment no-shows.
When patients fail to appear for appointments, this costs HFS time and
costs the patient opportunity for therapy. We explore whether clear patterns might contribute to patients missing visits,
such as a location or ethnicity effect. It might be the case that
particular facilities are less friendly in supporting a language, affecting the rate of dropped appointments.

The research also explores whether job title affects dropped
appointments. Job requirements might change from title to title, which might affect dropped appointments. This research explores the
phenomenon.

Initial research also explores appointment duration. Based on the given
data, it is unknown whether appointment duration is fixed by insurance
or varies between patients. This research explores the duration of
appointments across job position, ethnicity, location, and the rate of
dropped appointments too. While success is not determined by duration,
Dr. Juarez mentioned how HFS is very interested in exploring patterns
about the number of appointments and durations by each patient
since it affects the funding HFS receives.

### Research Question #3
Which facilities provide services to clients identifying as
Latino?**

### The Decisions your Analysis Targets Could Help Support

Which facilities provide services to Latino ethnicity? Would the Clients
prefer to have communication in Spanish? Do these facilities provide
Spanish-speaking therapists?

# Data Cleaning Choices

We have created a new dataset that can help us achieve our research question's best results. Attributes are mentioned below.

`facility`, `actual_date`, `event_name`, `date_entered`,
`approved_date`, `program_unit_description`, `zip`,
`state`,`ethnic_identity`,
`Job Title (Therapists I, II, and III)`,`Appointment Duration`.
`Appointment No Shows`.

**step 1:-** collect all the columns that are required and store them in
a data frame **HFS\_data**

**step 2:-** Since we are using Data format, we have to find the
original date. So, we have used as.Date() function to get the actual
date format.

**step 3:-** Apply step2 to all the date columns to verify it in next
phase.

**step 4:-** Now create a new column. Now apply to add and subtract
methods on the dates if we get the values in negatives, then it is
enrolled before the event, if 0 then they resisted at the moment, and if
greater the 0 than it is after the event.

**step 5:-** Total we have five states in the data set which is mentioned
in short form

**step 6:-** Every State in short form like `IA`,`NE`,`CO`,`NC`,`SC'and
later updated to the full form of user understanding `iowa`,
`Nebrska`,`colorado`, `north carolina`,`south carolina` and we can
observe many different plots based on state and there zip code.

**step 7:-** The data contains rows. If we filter out NA values for job
title, there are 8158. This means each row has a job title and 
no NA values. There is no missing data, and there is no need to
handle missing data.

**step 8:-**

Most job titles have fewer than fifty instances. Job titles with many
instances include therapist, clinical supervisor, case managers, and
admin assists. Of those job titles, there are five types of therapists.
Given that most of the primary job titles are therapists, exploring
job titles will focus on therapists. We filter the job titles to the
various therapist job positions.

If we filter out therapists, there are only 7246 rows, so 1500 fewer
rows.

-   **ethnicity**

<!-- -->

    ## [1] "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino"
    ## [3] "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino"
    ## [5] "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino"

We see that 15% of all rows are no-shows. 15% seems like a surprisingly
high number of appointment no-shows for any organization. This metric is
worth looking into further. There are no NAs in the column or values we
want to filter.

**Step 9** Delete Rows that have "Not Collected" in variable
Ethnic\_Identity.

**Step 10** I then discovered that the variable "ethnic\_identity" had
the following classes *Mexican *Not Collected *Not
Spanish/Hispanic/Latino *Other Hispanic or Latino \*Unknown

I omitted the rows that had "Not collected" or "Unknown" since this
information will not help with the interpretation of data.

I then changed "Mexican" to "Latino" and "Other Hispanic or Latino" to
"Latino" I changed "Not Spanish/Hispanic/Latino" to "Not Latino" This
leaves the data with 2 classes for the variable titled
"ethnic\_identity" Latino Not Latino

    ##  [1] "program_name"             "facility"                
    ##  [3] "actual_date"              "event_name"              
    ##  [5] "date_entered"             "approved_date"           
    ##  [7] "zip"                      "state"                   
    ##  [9] "age"                      "ethnic_identity"         
    ## [11] "is_noshow"                "job_title"               
    ## [13] "program_type"             "program_unit_description"
    ## [15] "AD"                       "ED"                      
    ## [17] "AD_year"                  "AD_ED"                   
    ## [19] "ED_APD"                   "AD_APD"

## Overview Summary of Answers for our Research Questions

## Research Question 1

In the below code, I have filtered the original data with the state name
and aggregate the whole data based to get an average time taken for
completing the HFS process.

In the Below code, I have built a model to see if it have a better
confidence in between the attributes in the data frame or not tried to get a predict value for over all data for Iowa

### Iowa

Now I have created a plot to see how the predicted value is similar with
![](WrittenSummary_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

(optional) Now for just verification, i am trying to get a Residuals of
the data that is present in Iowa

    ##             Df Sum Sq Mean Sq F value  Pr(>F)   
    ## facility    19  321.2  16.905   2.304 0.00932 **
    ## Residuals   51  374.3   7.339                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

### Nebrska

![](WrittenSummary_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## facility    14   3810  272.14  18.781 1.44e-11 ***
    ## AD_year      8    353   44.08   3.042   0.0116 *  
    ## Residuals   32    464   14.49                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


If we observe the graph we can tell that overall behavior of enrollment
process w.r.t the time taken for per person to enroll for an event from
past **8 years** in the state of **Iowa**.

-   There are few facilities where it is taking more time to enroll for
    a person compared to the previous year.

-   We can notice that in the enrolling time is so fast in schools.

-   From the plot, I can tell that all the enrollments have been late for
    In the past two years, there may be multiple reasons.

**example: -** lockdown because Covid-19 which stopped the process

![](WrittenSummary_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

![](WrittenSummary_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

If we observe the graph, we can tell that overall enrollment
process behavior w.r.t the time is taken for per person to enroll for an event from
past **9 years** in the state of **Nebraska**. \* We found out that
overall enrollment time is decreased compared to previous years.

-   In some facilities, the enrollment process is the same.

-   Some did not conduct any events for the next few years.

-   Overall, there is no delay in the registration process compared to
    previous years.
    
# Research Question 2

HFS facility locations have a significant effect on the number of missed
appointments.

The research question builds on existing knowledge that Omaha and Iowa
contain pockets of segregated populations, both in terms of ethnicity,
finances, and privilege. The research question poses that location
might significantly affect missed appointments. If a particular
location has a population with financial access to vehicles or even
multiple vehicles, they might be more able to make their appointments.
Initially, we hope to identify that facility location affects missed
appointments. If we can show this is true, we can further explore how the facility location negatively affects the client's ability to make
their appointments. This research data looks explicitly at the mental
health program, the largest for HFS. This research also breaks down no-shows by the type of therapist, knowing that different 
therapists might handle different appointments, each of which might be
related to the likelihood of missing appointments. This research
question does not break down the data by ethnicity, only because the
ethnicity counts were not large enough to be meaningful for each
facility and job title.

#### Plot: No show percentage by facility, ethnicity, and type of therapist

![](WrittenSummary_files/figure-gfm/facility-1.png)<!-- -->

The purpose of the above graph is to highlight the stark difference in
the y-axis for each facet. The y-axis shows no-show percentage, which
captures the percentage of missed appointments, e.g., clients
do not show up for their appointments. The facet is facility, such as
the "Center Mall Office" and "Omaha (Blondo) Reporting Center." The
graph shows significant differences in no-show percentage by the facility.
Depending on the facility, the range itself varies from around 5% to almost 30%. The plot also separates out job function since it has a
significant effect on no show percentage and tends to drop from
Therapist I to Therapist II.

The takeaway is that the no-show percentage is tightly related to the
facility. This means that the rate at which clients skip or miss
appointments is tightly coupled with the specific facility. This means
that some facilities show much higher rates of appointment misses than
others. For example, the Gendler HFS location should expect very high
appointment no-shows, up to 25%, contrasted with the Sarpy location that averages fewer than 10% no-shows.

HFS should seek to understand why location is closely coupled to missing
appointments. This might be related to particular locations being more
difficult to access, such as difficult traffic or limited parking. On
the other hand, each location might have very different populations,
whether via ethnicity or financial differences, that provide different
levels of privilege, affecting the client's ability to allocate
time for appointments. Further research should seek to understand what
about location or the clients that visit these locations affect missed
appointments.

# Research Question 3

The data cleaning results in a new subset of 5 variables based on the
attributes analyzed in the first research question. Important columns
within this data cleaning are listed below: program\_unit\_desc
ethnic\_identity age facility program\_type

This BoxPlot identifies that most Latinos receive services for Mental
Health Programs and are between the ages of 18 and 50.
![](WrittenSummary_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

\#\#Bar chart of Ethnicity & Facility

This BarChart shows us that the majority of Latinos served to attend the
North Omaha Campus and the Heartland Family Service-Central location.

![](WrittenSummary_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

# Conclusion

Our research takes a deep dive into the inner workings of HFS facilities. We looked at appointment delays, missed appointments, and the ethnicities at each facility. We found large discrepancies in appointment delays, missed appointments, and the number of persons with a Latino ethnicity among locations. These results are detailed via the plots in the above section. Firstly, while we find that appointment delays vary widely among facilities, some facilities, such as schools, have a faster registration and approval process, wheras counties take much longer. We also saw that the overall enrollment delay tends to decrease over time. We found that missed appointments vary by multiple factors based on location in a similar vein.  The Sarpy location might have a ten-percent missed appointment rate, but the Blondo location has a nearly thirty-percent rate of missed appointments. In our opinion, this unveils a massive discrepancy in the user experience and facility operations. However, we are not such why this delay occurs. We suspect the delay might be a function of facility accessibility, such as traffic, parking, or alternative transportation options. Thirdly, we analyzed the number of Latinos by location. We found that three locations stand out as serving this demographic and suggest each of these locations has bilingual staff to support their experience at HFS. Based on these findings, we believe that HFS can better understand the differences in user experience by the facility and improve the user experience throughout Nebraska and Iowa locations.

# Reference

-   <https://www.heartlandfamilyservice.org/>

-   <https://addictiontreatmentmagazine.com/rehab/heartland-family-service-omaha/>

-   <https://www.heartlandfamilyservice.org/our-mission/>
