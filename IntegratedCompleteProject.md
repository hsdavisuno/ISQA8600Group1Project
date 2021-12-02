Integrated Complete Project
================

</br>

## RQ

**Is there any correlation between demographics(Age, Gender Identity and
Ethnic Identity) and the quantity of mental health care received? What
demographics have longer service time requirements than others?**

**We’re interested in seeing if there’s a link between demographics and
mental health care. There are also many studies that claim there is a
link between different demographics and mental health care, which piqued
our curiosity in conducting this research. The studies that found a link
between demographics and mental health are listed below:** </br> **1) [The
State of Mental Health and Aging in
America](https://www.cdc.gov/aging/pdf/mental_health.pdf)** </br> **2)
[Gender and
Mental](https://www.who.int/gender/other_health/genderMH.pdf)** </br>
**3) [Race, Ethnicity and Mental
Health](https://www.asanet.org/sites/default/files/savvy/images/members/docs/pdf/special/jhsb/jhsb_44_3_takeuchi.pdf)**
</br> **Knowing if there is a correlation can help HFS shorten the
service time it takes for specific populations by sending them to expert
staff.**

</br>

## DATA CLEANING



### Data Sources

-   Heartland Family Services. (2021). HFS Service Data
    *D**a**t**a**f**i**l**e*
    . Retrieved from:
    <https://unomaha.instructure.com/courses/50683/files/5256675?module_item_id=1562501>

    The primary data source we used for our data cleaning was the HFS
    Service Data.csv file from the HFS resources section of Canvas. This
    file was provided to our class by Clayton Juarez, Ph.D.of Heartland
    Family Services (HFS) in Omaha, Nebraska. Dr. Juarez pulled the data
    from a system HFS uses to track the mental health services it
    provides to clients. The file contained 51 columns and 8746 rows.
    The primary source contained column heading such as: gender,
    program\_name, program\_type, and facility.

-   Heartland Family Services. (2021). SLA Data Columns 2021-08-25
    *D**a**t**a**f**i**l**e*
    . Retrieved from:
    <https://unomaha.instructure.com/courses/50683/files/5217061?module_item_id=1562500>

    The secondary data source we used for our data cleaning was the SLA
    Data Columns 2021-08-25.xlsx file from the HFS resources section of
    Canvas. This file was also provide to us by Dr. Juarez. The file
    contains 3 columns and 52 rows. It provides the field names, notes,
    and descriptions about the fields. From the SLA Data Columns file,
    we were able to discern helpful information such as the meaning of
    the codes provided for race.

</br>

### Intellectual Policy Constraints

Permission for us to use this information was given to us by Heartland
Family Services. Even though the information has been cleaned of
personal information, we are still bound to observe data privacy
standards.

</br>

### Metadata

The source we had for Metadata was the SLA Data Columns document
described in the preceding Data Sources section. This document contained
short descriptions of the fields, often just writing a longer
description of the field name but sometimes listing out the meanings of
codes used in the columns.

</br>

### Data Issues

-   gender\_identity - In the gender identity column, we found both the
    value of female and woman and felt this was duplicate, especially
    since we did not see an equivalent with the value of man. Only man
    was used and not male.

-   simple\_race - In the simple race column, we found values that did
    not correspond to the numeric codes provided in the SLA Data Columns
    document.

</br>

### Data Remediation

-   Removed rows we did not need for our research.

-   is\_approved - We only kept the rows from the data that had a value
    of 1 from this column. These were records that were approved by the
    supervisor.

-   simple\_race - We removed the rows in the data that did not
    correlate with the values provided in the SLA Data Columns
    document.We also replaced the numeric codes for race with their
    label values to make them easier to understand. For example, the
    value 2 was replaced with Alaskan Native.

-   gender\_identity - We changed the value of female to woman to
    correspond to the value of man because male was not used for
    individuals who identified themselves as men. We also changed the
    value to Not Obtained to NA since we felt they were very similar.

</br>

### Code

**1. Loading the required packages**

``` r
library(dplyr)
library(ggplot2)
library(tidyverse)
library(tinytex)
library(rticles)
```

**2. To load the data**

``` r
Full_HFS_Data <- read.csv("HFS Service Data.csv")
```

**3. To select the columns that are required for our analysis**

``` r
HFS_1 <- select(Full_HFS_Data, c(program_name, facility, job_title,actual_date, event_name, is_approved, NormalWorkHours,duration_num, total_duration_num, recordID,state, age, simple_race, gender_identity))
```

**4. To select the data that has is\_approved = 1**

``` r
HFS_2 <- subset(HFS_1, is_approved == 1) 
```

Here is\_approved = 1 means the people who are approved for the service.
So, only approved data and patients who have visited the service are
taken into consideration as there is no point in analyzing the data for
which the patients have not been treated.

**5. To omit the data that has simple\_race = 21,65,5,9,17,20,81**

``` r
HFS_Data <- subset(HFS_2,  simple_race != 21 & simple_race != 65 &simple_race != 5 & simple_race != 9 & simple_race != 17 & simple_race != 20  & simple_race != 81 )
```

The simple race having numbers 21, 65, 5, 9, 17, 20, 81 is incorrect
data. So, we are removing the false data here.

**6. To replace the numeric codes in simple\_race to actual words.**

``` r
HFS_Data$simple_race[HFS_Data$simple_race ==1 ] <- "Caucasian"
HFS_Data$simple_race[HFS_Data$simple_race ==2 ] <- "Alaskan Native"
HFS_Data$simple_race[HFS_Data$simple_race ==4 ] <- "American Indian/Native American"
HFS_Data$simple_race[HFS_Data$simple_race ==8 ] <- "Asian"
HFS_Data$simple_race[HFS_Data$simple_race ==16 ] <- "Black / African-American"
HFS_Data$simple_race[HFS_Data$simple_race ==32] <- "Hawaiian Native / Pacific Islander"
HFS_Data$simple_race[HFS_Data$simple_race ==64] <- "Two or more races"
HFS_Data$simple_race[HFS_Data$simple_race ==128 ] <- "Other"
HFS_Data$simple_race[HFS_Data$simple_race ==0 ] <- "Unknown or Not Collected"
```

We are here replacing the numeric numbers with the exact words so that
it will be good for analysis.

**7. To modify the column gender\_identity**

``` r
HFS_Data$gender_identity[HFS_Data$gender_identity == "Female"] <- "Woman"
HFS_Data$gender_identity[HFS_Data$gender_identity == "Other"] <- "Non-Binary"
HFS_Data$gender_identity[HFS_Data$gender_identity == "Two-Spirit"] <- "Non-Binary"
```

We have modified the values “Female” to “Woman” because they both mean
the same. We have modified the values “Other” and “Two-Spirit” to
“Non-Binary” as two spirit is both male and female spirit which is non
binary and also if they are not man or woman they are non-binary.

**8. To modify the values containing “NA” in data to “Not Obtained”**

``` r
HFS_Data[is.na(HFS_Data)] <- "Not Obtained"
```

**9. To modify the actual\_date column**

``` r
HFS_Data$actual_date <- as.numeric(HFS_Data$actual_date)
HFS_Data <- HFS_Data %>% mutate(actual_date = case_when(actual_date >= 3287  & actual_date <= 3652 ~ 'Year10',
                                                        actual_date >= 2923  & actual_date <= 3287 ~ 'Year9',
                                                        actual_date >= 2557  & actual_date <= 2922 ~ 'Year8',
                                                        actual_date >= 2192  & actual_date <= 2556 ~ 'Year7',
                                                        actual_date >= 1827  & actual_date <= 2191 ~ 'Year6',
                                                        actual_date >= 1462  & actual_date <= 1826 ~ 'Year5',
                                                        actual_date >= 1096  & actual_date <= 1461 ~ 'Year4',
                                                        actual_date >= 731  & actual_date <= 1095 ~ 'Year3',
                                                        actual_date >= 366  & actual_date <= 730 ~ 'Year2',
                                                        actual_date >= -7  & actual_date <= 365   ~ 'Year1'))
```

We’ve filtered the days with respect to years from the beginning to the
end so that evaluating the results is simple.

**10. To verify data has modified according to our needs**

``` r
sort(unique(HFS_Data$simple_race))
```

    ## [1] "American Indian/Native American"    "Asian"                             
    ## [3] "Black / African-American"           "Caucasian"                         
    ## [5] "Hawaiian Native / Pacific Islander" "Other"                             
    ## [7] "Two or more races"                  "Unknown or Not Collected"

``` r
unique(HFS_Data$gender_identity)
```

    ## [1] "Not Obtained"            "Woman"                  
    ## [3] "Man"                     "Client Declined to Give"
    ## [5] "Non-Binary"

``` r
unique(HFS_Data$is_approved)
```

    ## [1] 1

**11. To view the modified data**

``` r
str(HFS_Data)
```

    ## 'data.frame':    7989 obs. of  14 variables:
    ##  $ program_name      : chr  "Mental Health" "Mental Health" "Mental Health" "Mental Health" ...
    ##  $ facility          : chr  "Center Mall Office" "Center Mall Office" "Center Mall Office" "Center Mall Office" ...
    ##  $ job_title         : chr  "THERAPIST II" "THERAPIST II" "THERAPIST II" "THERAPIST II" ...
    ##  $ actual_date       : chr  "Year3" "Year2" "Year2" "Year2" ...
    ##  $ event_name        : chr  "Collateral Note" "Individual Therapy" "Individual Therapy" "Individual Therapy" ...
    ##  $ is_approved       : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ NormalWorkHours   : chr  "Yes" "Yes" "Yes" "Yes" ...
    ##  $ duration_num      : int  2 51 0 50 80 4 2 52 115 0 ...
    ##  $ total_duration_num: int  2 61 0 60 90 4 2 62 125 0 ...
    ##  $ recordID          : int  338 338 338 338 338 338 338 338 338 338 ...
    ##  $ state             : chr  "NE" "NE" "NE" "NE" ...
    ##  $ age               : int  26 25 25 25 25 25 26 25 25 25 ...
    ##  $ simple_race       : chr  "Black / African-American" "Black / African-American" "Black / African-American" "Black / African-American" ...
    ##  $ gender_identity   : chr  "Not Obtained" "Not Obtained" "Not Obtained" "Not Obtained" ...

</br> </br>

## R Script



### The reason for selecting only Mental Health from the three services provided.

``` r
nofvisits <- data.frame(count(HFS_Data,actual_date,program_name,facility))
ggplot(data = nofvisits,aes(x=actual_date,y=n,na.rm=TRUE))+geom_bar(stat="identity")+facet_wrap(~program_name)+ 
ggtitle("Total visits by years from the start of service for different program names")+
theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1)) 
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-12-1.png)<!-- -->

We can observe from the graph above that visits to mental health
programs are increased every year when compared to other programs. Other
programs’ duration hours vary dramatically as well. As a result, we
chose to focus our research solely on mental health, ignoring substance
use and gambling.

### Box Plots of different programs w.r.t duration

``` r
ggplot(HFS_Data, aes(program_name,duration_num,by1=recordID,fill=program_name)) + 
  geom_boxplot(aes(program_name), alpha=0.3) +
  xlab("Programs offered") +ylab("Duration")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-13-1.png)<!-- -->

``` r
  theme(legend.position = "none")
```

    ## List of 1
    ##  $ legend.position: chr "none"
    ##  - attr(*, "class")= chr [1:2] "theme" "gg"
    ##  - attr(*, "complete")= logi FALSE
    ##  - attr(*, "validate")= logi TRUE

</br>

### Removing the unwanted outliers

``` r
HFS_Data <- HFS_Data %>%
  filter (duration_num < 350 & duration_num > 10)
```

We have removed the outliers which we encountered in the boxplot above.

</br>

### Additional Data cleaning for Facility

``` r
HFS_Data$facility[HFS_Data$facility == "Center Mall Office"] <- "Center"
HFS_Data$facility[HFS_Data$facility == "North Omaha Intergenerational Campus (Service)"] <- "NOIC"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Logan"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Gendler"] <- "HFS-Gendler"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Child and Family Center"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Central"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Family Works Nebraska"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Sarpy"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Glenwood"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Heartland Homes"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Lakin"] <- "HFS-Other"
HFS_Data$facility[HFS_Data$facility == "Omaha (Spring) Reporting Center"] <- "RepCntr"
HFS_Data$facility[HFS_Data$facility == "Omaha (Blondo) Reporting Center"] <- "RepCntr"
HFS_Data$facility[HFS_Data$facility == "Bellevue Reporting Center"] <- "RepCntr"
HFS_Data$facility[HFS_Data$facility == "Sanctuary House"] <- "SancHouse"
HFS_Data$facility[HFS_Data$facility == "Lewis Central High School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Lewis Central Middle School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Kirn Junior High  School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Wilson Middle School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Thomas Jefferson High School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Kreft Primary School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Abraham Lincoln High School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Titan Hill Intermediate School"] <- "School"
HFS_Data$facility[HFS_Data$facility == "Kanesville Alternative Learning Center"] <- "School"
```

We have reassigned the names of facilities for better analysis of data.
We have grouped all the facilities having school into School, reporting
canter as RepCntr, HFS as HFS-Other and HFS\_Gendler. Here We have
separated only the HFS-Gendler from other HFS places as HFS\_Gendler is
having more visits.

</br>

### Reason for choosing only IOWA and Nebraska

**As these services are mostly concentrated in Iowa and Nebraska and
also the people from from NE and IA are most frequently enrolling for
the services offered by HFS.**

</br>

### Subsetting to get the required data

``` r
HFS_Data <- subset(HFS_Data,program_name == 'Mental Health')
HFS_Data <- HFS_Data %>% filter(state=="NE" | state=="IA")
HFS_Data <- HFS_Data %>% mutate(ageg = case_when(age >= 55  & age <= 72 ~ '55-72',
                                                     age >= 37  & age <= 54 ~ '37-54',
                                                     age >= 19  & age <= 36 ~ '19-36',
                                                     age >= 0  & age <= 18 ~ '1-18'))
```

</br>

</br>

## R Plots



### The next three plots depict the average number of hours of service required for clients from various demographics who visited various facilities and came from various states:

</br>

**Here we have divided the ages, gender\_identity and race into groups
so that the analysis can be done perfectly.**

</br>

-   #### Service hours spent on different age groups by facility and state

``` r
age<-aggregate(duration_num~ageg+facility+state,HFS_Data,FUN=mean)
age$duration_num<-round(age$duration_num,digits=1)

ggplot(age,aes(x=ageg,y=duration_num,na.rm=TRUE,fill=state))+geom_bar(stat="identity",position="dodge")+facet_wrap(~facility,ncol=2)+geom_text(aes(label=duration_num), position=position_dodge(width=1), vjust=1,size=3)+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1)) +labs(y= "Average service hours spent", x = "Age Groups",title = "Service hours spent on different age groups by facility and state")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-16-1.png)<!-- -->

**Age Groups:** </br> We can see from the above graph that the age group
37-54 takes the longest to get treated, followed by the age groups 19-36
and 55-72. Also, we can notice that NOIC and School facilities are
exclusively visited by people aged 1 to 18. Patients from both states
are treated at the facilities Center and HFS-other. Clients of various
ages come to Micah House from Iowa, but only those between the ages of
19 and 36 come from Nebraska.

</br>

-   #### Service hours spent on different genders by facility and state

``` r
gender<-aggregate(duration_num~gender_identity+facility+state,HFS_Data,FUN=mean)
gender$duration_num<-round(gender$duration_num,digits=1)

ggplot(gender,aes(x=gender_identity,y=duration_num,na.rm=TRUE,fill=state))+geom_bar(stat="identity",position="dodge")+
  facet_wrap(~facility,ncol=2) +geom_text(aes(label=duration_num), position=position_dodge(width=1), vjust=1,size=3)+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1)) +
  labs(y= "Average service hours spent", x = "Gender",title = "Service hours spent on different genders by facility and state")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-17-1.png)<!-- -->

**Gender:** </br> The most treatment hours are required by women from
Nebraska’s “RepCntr” facility. If we look at the School facility, the
least number of hours necessary for treatment is for Man. We may also
see that the facilities School only have clients from Iowa, whereas 
SancHouse and RepCntr only have clients from Nebraska. If
we look at all of the facilities, we can conclude that women require
more or equal treatment hours than men.

</br>

-   #### Service hours spent on different races by facility and state

``` r
race<-aggregate(duration_num~simple_race+facility+state,HFS_Data,FUN=mean)
race <- race %>% filter(simple_race=="Asian" | simple_race=="American Indian/Native American" | 
                          simple_race=="Black / African-American" | simple_race=="Caucasian" | simple_race=="Hawaiian Native / Pacific Islander")
race$simple_race[race$simple_race == "American Indian/Native American"] <- "Native American"
race$simple_race[race$simple_race == "Hawaiian Native / Pacific Islander"] <- "Hawaiian Native"
race$duration_num<-round(race$duration_num,digits=1)

ggplot(race,aes(x=simple_race,y=duration_num,na.rm=TRUE,fill=state))+geom_bar(stat="identity",position="dodge")+
  facet_wrap(~facility,ncol=2)+geom_text(aes(label=duration_num), position=position_dodge(width=1), vjust=1,size=3)+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1)) +
    labs(y= "Average service hours spent", x = "Race",title = "Service hours spent on different races by facility and state")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-18-1.png)<!-- -->

**Race:** </br> If we compare all of the facilities, Native Americans
receive the most hours on average, as shown in the graph above. For the
RepCntr facility, the largest average service hours are spent on
American Natives. There aren’t many Asian customers who benefit greatly
from this service. When compared to all of the facilities, Caucasian
clients require the least amount of service hours.

**Conclusion from the above 3 graphs:** </br> Regardless of
demographics, a closer examination of the above three plots reveals that
clients visiting the same institutions receive identical treatment
hours. People that come to School, for example, have the same
treatment hours regardless of their demographics. This information may
aid HFS in determining whether facilities require additional investment
in order to reduce employee service hours and client treatment hours.
This research also helps HFS determine which demographic client should
be assigned to which member of staff or facility in order to improve
program efficiency.
