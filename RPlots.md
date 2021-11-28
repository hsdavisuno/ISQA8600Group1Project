R Plots
================

**Team 1 - Akhil Kodali, Heather Davis, Scott Bui**

# Data Cleaning

Same data cleaning we performed before with one slight modification.

### Loading the required packages

``` r
library(dplyr)
library(ggplot2)
library(tidyverse)
library(tinytex)
library(rticles)
```

### To load the data

``` r
Full_HFS_Data <- read.csv("HFS Service Data.csv")
```

### To select the coloumns that are required for our analysis

``` r
HFS_1 <- select(Full_HFS_Data, c(program_name, facility, job_title, event_name, is_approved, NormalWorkHours,duration_num, total_duration_num, recordID, state, age, simple_race, gender_identity))
```

This time we included the state column.

### To select the data that has is\_approved = 1

``` r
HFS_2 <- subset(HFS_1, is_approved == 1) 
```

Here is\_approved = 1 means the people who are approved for the service.
So, only approved data and patients who have visited the service are
taken into consideration as there is no point in analyzing the data for
which the patients have not been treated.

### To omit the data that has simple\_race = 21,65,5,9,17,20,81

``` r
HFS_Data <- subset(HFS_2,  simple_race != 21 & simple_race != 65 &simple_race != 5 & simple_race != 9 & simple_race != 17 & simple_race != 20  & simple_race != 81 )
```

### To replace the numeric codes in simple\_race to actual words.

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

### To add modify the column gender_identity to move Female to Woman, and NA and Client Declined to Give to Not Obtained

``` r
HFS_Data$gender_identity[HFS_Data$gender_identity == "Female"] <- "Woman"
HFS_Data[is.na(HFS_Data)] <- "Not Obtained"
HFS_Data$gender_identity[HFS_Data$gender_identity == "Client Declined to Give"] <- "Not Obtained"
```

### To verify data has been modified according to our needs

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

    ## [1] "Not Obtained" "Woman"        "Man"          "Two-Spirit"   "Non-Binary"  
    ## [6] "Other"

``` r
unique(HFS_Data$is_approved)
```

    ## [1] 1

### To fit the different ages into age groups

``` r
HFS_Data <- HFS_Data %>% mutate(agegroup = case_when(age >= 63  & age <= 72 ~ '63-72',
                                                       age >= 54  & age <= 63 ~ '54-63',
                                                       age >= 45  & age <= 54 ~ '45-54',
                                                       age >= 37  & age <= 45 ~ '37-45',
                                                       age >= 28  & age <= 36 ~ '28-36',
                                                       age >= 19  & age <= 27 ~ '19-27',
                                                       age >= 10  & age <= 18 ~ '10-18',
                                                       age >= 0  & age <= 8 ~ '1-9'))
```

### To view the modified data

``` r
str(HFS_Data)
```

    ## 'data.frame':    7989 obs. of  14 variables:
    ##  $ program_name      : chr  "Mental Health" "Mental Health" "Mental Health" "Mental Health" ...
    ##  $ facility          : chr  "Center Mall Office" "Center Mall Office" "Center Mall Office" "Center Mall Office" ...
    ##  $ job_title         : chr  "THERAPIST II" "THERAPIST II" "THERAPIST II" "THERAPIST II" ...
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
    ##  $ agegroup          : chr  "19-27" "19-27" "19-27" "19-27" ...

### Additional Data cleaning for Gender Identity and Facility

``` r
HFS_Data$gender_identity[HFS_Data$gender_identity == "Other"] <- "Non-Binary"
HFS_Data$gender_identity[HFS_Data$gender_identity == "Two-Spirit"] <- "Non-Binary"
```

``` r
HFS_Data$facility[HFS_Data$facility == "Center Mall Office"] <- "Center"
HFS_Data$facility[HFS_Data$facility == "North Omaha Intergenerational Campus (Service)"] <- "NOIC"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Logan"] <- "HFS-Logan"
HFS_Data$facility[HFS_Data$facility == "Heartland Family Service - Gendler"] <- "HFS-Other"
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

## Plots

### Hours and Facilities, Faceted by Gender Identity

This plot represents the total duration of hours for visits at
facilities and faceted by gender identity. The plot can tell us that
women make up the majority of hours for this organization. We can also
see that the majority of hours are occurring at HFS-Other grouped
facilities and that non-binary clients are seen at HFS-Other and school
facilities. We can see that no men are being seen at Micah house and
Sanctuary House. To break down this plot further, I’d like to break down
just the HFS facilities.

``` r
remove_outliers <- HFS_Data %>%
filter (total_duration_num < 350)
```

``` r
ggplot(data = remove_outliers) + 
  geom_col(mapping = aes(x = facility, y = total_duration_num, fill=facility)) +
  theme(axis.text.x = element_text(angle=90, vjust=1, hjust=1)) +
  facet_grid(. ~gender_identity)
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/RPlots/plot%20hours%20and%20facilities%20with%20gender%20id%20facets-1.png)<!-- -->

### Hours and Gender Identity Only

This plot focuses on just total duration hours and gender identity. It
clearly shows that the majority provided by the facilities is for women
and those for whom the gender identity was not obtained. I’d like to
break this down further to show the detail on what not obtained means
and by what facilities to see if any patterns exist. Gender identity
identification can be very helpful in identifying the needs of the
organization and knowing more about their clients might be helpful to
them.

``` r
ggplot(data = HFS_Data) + 
geom_col(mapping = aes(x = gender_identity, y = total_duration_num, fill=gender_identity))
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/RPlots/hours%20and%20gender%20id%20bar-1.png)<!-- -->

### Removing some unwanted outliers

``` r
HFS_Data1 <- HFS_Data %>%
  filter (duration_num < 350 & duration_num > 10)
```

### Subsetting to get the required data

``` r
HFS_Data1 <- subset(HFS_Data1,program_name == 'Mental Health')
HFS_Data1 <- HFS_Data1 %>% filter(state=="NE" | state=="IA")
HFS_Data1 <- HFS_Data1 %>% mutate(ageg = case_when(age >= 55  & age <= 72 ~ '55-72',
                                                     age >= 37  & age <= 54 ~ '37-54',
                                                     age >= 19  & age <= 36 ~ '19-36',
                                                     age >= 0  & age <= 18 ~ '1-18'))
```

</br>

### The next three graphs depict the average number of hours of service required for clients from various demographics who visited various facilities and came from various states.

### Service hours spent on different age groups by facility and state

``` r
age<-aggregate(duration_num~ageg+facility+state,HFS_Data1,FUN=mean)
age$duration_num<-round(age$duration_num,digits=1)

ggplot(age,aes(x=ageg,y=duration_num,na.rm=TRUE,fill=state))+geom_bar(stat="identity",position="dodge")+facet_wrap(~facility,ncol=2)+geom_text(aes(label=duration_num), position=position_dodge(width=1), vjust=1,size=3)+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1)) +labs(y= "Average service hours spent", x = "Age Groups",title = "Service hours spent on different age groups by facility and state")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/RPlots/unnamed-chunk-5-1.png)<!-- -->

Age Groups: We can see from the above graph that the age group 37-54
takes the longest to get treated, followed by the age groups 19-36 and
55-72. Also, we can notice that NOIC and School facilities are
exclusively visited by people aged 1 to 18. Patients from both states
are treated at the facilities Center and HFS-other. Clients of various
ages come to Micah House from Iowa, but only those between the ages of
19 and 36 come from Nebraska.

### Service hours spent on different genders by facility and state

``` r
gender<-aggregate(duration_num~gender_identity+facility+state,HFS_Data1,FUN=mean)
gender$duration_num<-round(gender$duration_num,digits=1)

ggplot(gender,aes(x=gender_identity,y=duration_num,na.rm=TRUE,fill=state))+geom_bar(stat="identity",position="dodge")+
  facet_wrap(~facility,ncol=2) +geom_text(aes(label=duration_num), position=position_dodge(width=1), vjust=1,size=3)+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1)) +
  labs(y= "Average service hours spent", x = "Gender",title = "Service hours spent on different genders by facility and state")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/RPlots/unnamed-chunk-6-1.png)<!-- -->

Gender: The most treatment hours are required by women from Nebraska’s
“RepCntr” facility. If we look at the School facility, the least number
of hours necessary for treatment is for Man. We may also see that the
facilities HFS-Logan and School only have clients from Iowa, whereas
SancHouse and RepCntr only have clients from Nebraska. If we look at all
of the facilities, we can conclude that women require more or qual
treatment hours than men.

### Service hours spent on different races by facility and state

``` r
race<-aggregate(duration_num~simple_race+facility+state,HFS_Data1,FUN=mean)
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

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/RPlots/unnamed-chunk-7-1.png)<!-- -->

Race: If we compare all of the facilities, Native Americans receive the
most hours on average, as shown in the graph above. For the RepCntr
facility, the largest average service hours are spent on American
Natives. There aren’t many Asian customers who benefit greatly from this
service. When compared to all of the facilities, Caucasian clients
require the least amount of service hours.

Conclusion for the above 3 graphs: Regardless of demographics, a closer
examination of the above three plots reveals that clients visiting the
same institutions receive identical treatment hours. People that come to
HFS-Logan, for example, have the same treatment hours regardless of
their demographics. This information may aid HFS in determining whether
facilities require additional investment in order to reduce employee
service hours and client treatment hours. This research also helps HFS
determine which demographic client should be assigned to which member of
staff or facility in order to improve program efficiency.

### Hours and Age with Program Name Facet

``` r
DurationAge <- ggplot(data=HFS_Data, aes(x=age, y=total_duration_num))+
geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))+
 facet_grid(.~program_name)+
  labs(title="Duration Time Utlized for Services in Different Ages")+
  xlab("Ages")+
  ylab("Total Duration Utilized")
options(scipen = 999)

DurationAge
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/RPlots/Hours%20and%20Age%20with%20Program%20Name%20Facet-1.png)<!-- -->

Mental health seems to be the highest and most populated health concern
for all age groups. We can take away those ages under 15 has the highest
spike utilizing help and services. Coming in second for the mental
health category are years 28-30. Substance use has its highest marks in
the group’s late 20’s specifically ages 26-30. Gambling shows very
little to be known and therefore is a huge concern for a particular
pattern of age groups. Our leading concern will have to Mental health
and what more can be done.

## Contributor Statement

Heather Davis, Scott Bui, and Akhil Kodali each created plots for their
research questions. Heather Davis wrote the first draft of the markdown
document and Akhil added the remaining code and plots to it. Akhil
uploaded the markdown document to the GitHub. Heather Davis proofed the
document, updated the ReadMe with a link, and submitted it for the
assignment.
