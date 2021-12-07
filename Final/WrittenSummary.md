# Written Summary

By Sai Krishna Gaduputi Subbammagari, Chad Crowe, and Rhonda Silva

# Overview

## Abstract

Heartland Family Services(HFS) comprises multiple facilities between Iowa and Nebraska that exhibit different aspects of customer behavior related to 1. enrollment, missed appointments, and the ethnicity of those appointments. Our group takes a deep dive into facility information concerning their enrollment process, the rate of missed appointments, and ethnicity information. We find the enrollment delays, missed appointments, and Latino ethnicity varies dramatically, based on the facility. Our study makes recommendations based on these findings and how HFS can investigate these facility differences to improve the user experience and organization's efficiency.

## Research Questions

### Research Question #1
**Do significant delays exist between enrolling in an event, entering it
into the system, and its enrollment approval?**

### The Decisions our Analysis Targets Could Support

This research question will help the HFS to know the average time taken for a person to
complete the full enrollment process for any event. Moreover, this is organized by different
facilities and accounts for data over the last few years. This research question uncovers patterns related to enrollment delays.  HFS can use this information to take action at particular locations in order to improve the enrollment/approval proces.

### Research Question #2
**Do HFS facility locations have a significant effect on the number of
missed appointments?**

### The Decisions our Analysis Targets Could Support

This research question allows HFS to understand the effect that facility has on missed appointments. Missed appointments describe events where the person does not show up for their appointment. These missed appointments are costly for HFS since the user misses their treatment and the therapist cannot perform chargeable services. Therefore, missed appointments represent large costs, both for the clients and in terms of overall revenue. This research question explores the relationship between the facility and missed appointments.

There are multiple aspects of a facility that might affect the number of missed appointments. A location with limited parking or limited alternative transportation might affect a client's ability to reach the facility. Further, a location without sidewalks might that primarily serves persons without personal vehicles might negatively affect the ease of visiting the location; therefore, the target demographic might also affect the rate of missed appointments. The language might also be a barrier. Facilities that serve a Spanish-only speaking population but lack a sufficient amount of language support might result in a higher missed appointment rate for particular ethnicities.  

The research also explores whether job title affects dropped appointments. Job requirements might change from title to title, affecting dropped appointments. This research explores the phenomenon. It might be that more experienced therapists are better at scheduling appointments and auxiliary details to help the patient make the meeting. In contrast, a busier facility might result in more persons falling through the cracks and, therefore, missing appointments. This question provides details concerning the relationship between the therapist job title and the rate of missed appointments.

Knowledge about the relationship between missed appointments and the facility can support numerous decisions. Firstly, it might create awareness about problematic facilities with higher missed appointment rates. Secondly, we uncover an interesting relationship between job title and missed appointments. HFS can take this knowledge and delve into why these facilities and job titles have higher rates of missed appointments. Given the large cost of missed appointments, we feel that this research question can potentially impact HFS's efficiency and revenue, improving both its and patients' outcomes.

### Research Question #3
**Which facilities provide services to clients identifying as
Latino?**

### The Decisions our Analysis Targets Could Support

HFS deeply cares about the experiences of minority demographics, therefore, we explore which facilities serve groups with these demographics. We focus on minorities that might speak Spanish. Our goal is to identify which facilities receive and serve this demographic. We hope to provide feedback concerning which facilities require more Spanish-speaking staff. Our results might help HFS to understand where their minorities are served and can ensure they provide the necessary support for these minority groups.

# Data Cleaning Choices

We have created a new dataset to help us achieve the best results for our research question. The attributes studies are listed below:

`facility`, `actual_date`, `event_name`, `date_entered`,
`approved_date`, `program_unit_description`, `zip`,
`state`,`ethnic_identity`,
`Job Title (Therapists I, II, and III)`,`Appointment Duration`.
`Appointment No Shows`.

**Step 1:-** collect all the columns that are required and store them within a data frame.

**Step 2:-** We calculate the original date.

**Step 3:-** We decorate all events with a date, based on its offset from the original date.

**Step 4:-** We then add columns that capture the delays each person experiences for their enrollment and event.

**Step 5:-** In total we collected data for five states but focus in on NE and IA.

**Step 6:-** We filter out NA rows, such as NA values for job title entries. This removes all rows with missing data.

**Step 7:-** We then group by columns of interest, such as facility, ethnic_identity, and job_title. This leaves us with 1500 fewer rows, 7246 rows in total.

**Step 8:-** We group ethnicity into Spanish-specific and non-Spanish ethnicity for our analysis.  More details are discussed immediately below.

**ethnicity**

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

**Step 10** We then discovered that the variable "ethnic\_identity" had
the following classes *Mexican *Not Collected *Not
Spanish/Hispanic/Latino *Other Hispanic or Latino \*Unknown

We omitted the rows that had "Not collected" or "Unknown" since this
information will not help with the interpretation of data.

We then changed "Mexican" to "Latino" and "Other Hispanic or Latino" to
"Latino" we changed "Not Spanish/Hispanic/Latino" to "Not Latino" This
leaves the data with 2 classes for the variable titled
"ethnic\_identity" Latino and Not Latino

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

In the below code, we have filtered the original date by state and facility and processed the data to show the average delays to complete the HFS enrollment process.

Firstly, we'll show our model that demonstrates that year and state is a large predictor of overall enrollment delays.

### Iowa

The below plot shows our ability to predict the enrollment delay by year and state.
![](WrittenSummary_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

(optional) Now for just verification, i am trying to get a Residuals of
the data that is present in Iowa

    ##             Df Sum Sq Mean Sq F value  Pr(>F)   
    ## facility    19  321.2  16.905   2.304 0.00932 **
    ## Residuals   51  374.3   7.339                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

### Nebraska

![](WrittenSummary_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## facility    14   3810  272.14  18.781 1.44e-11 ***
    ## AD_year      8    353   44.08   3.042   0.0116 *  
    ## Residuals   32    464   14.49                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


If we observe the graph, we can tell that the overall behavior of enrollment
process w.r.t the time taken per person to enroll for an event from
past **8 years** in the state of **Iowa**.

-   There are few facilities where it is taking more time to enroll for
    a person compared to the previous year.

-   We can notice that the enrolling time is faster in schools.

-   From the plot, I can tell that all the enrollments have been late for
    the past two years. There may be multiple reasons.

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
contain pockets of segregated populations, both ethnicity,
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
the y-axis for each facet. The y-axis shows the no-show percentage, which
captures the percentage of missed appointments, e.g., clients
do not show up for their appointments. The facet is the facility, such as
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

Most Latino clients received services at the North Omaha Campus and at the HFS-Central location. HFS should staff these two facilities with bi-lingual (Spanish) therapists and receptionists. HFS could possibly benefit by having the bilingual staff at the Sarpy Office as well so that there are 3 branches throughout the metro area to serve Latino clients.

![](WrittenSummary_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

# Conclusion

Our research takes a deep dive into the inner workings of HFS facilities. We looked at appointment delays, missed appointments, and the ethnicities at each facility. We found large discrepancies in appointment delays, missed appointments, and the number of persons with a Latino ethnicity among locations. These results are detailed via the plots in the above section. Firstly, while we find that appointment delays vary widely among facilities, some facilities, such as schools, have a faster registration and approval process, whereas counties take much longer. We also saw that the overall enrollment delay tends to decrease over time. We found that missed appointments vary by multiple factors based on location in a similar vein. For example, the Sarpy location might have a ten-percent missed appointment rate, but the Blondo location has a nearly thirty-percent rate of missed appointments. In our opinion, this unveils a massive discrepancy in the user experience and facility operations. However, we are not sure why this delay occurs. We suspect the delay might be a function of facility accessibility, such as traffic, parking, or alternative transportation options. Thirdly, we analyzed the number of Latinos by location. We found that three locations stand out as serving this demographic and suggest each of these locations has bilingual staff to support their experience at HFS. Based on these findings, we believe that HFS can better understand the differences in user experience by the facility and improve the user experience throughout Nebraska and Iowa locations.

# Reference

-   <https://www.heartlandfamilyservice.org/>

-   <https://addictiontreatmentmagazine.com/rehab/heartland-family-service-omaha/>

-   <https://www.heartlandfamilyservice.org/our-mission/>
