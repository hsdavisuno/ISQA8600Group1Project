Data Cleaning Documentation
================
Heather S. Davis, Akhil Kodali, and Scott Bui
October 24, 2021

# Data Sources

Heartland Family Services. (2021). HFS Service Data \[Data file\].
Retrieved from
\[<https://unomaha.instructure.com/courses/50683/files/5256675?module_item_id=1562501>\]
(<https://unomaha.instructure.com/courses/50683/files/5256675?module_item_id=1562501>)

The primary data source we used for our data cleaning was the HFS
Service Data.csv file from the HFS resources section of Canvas. This
file was provided to our class by Clayton Juarez, Ph.D. of Heartland
Family Services (HFS) in Omaha, Nebraska. Dr. Juarez pulled the data
from a system HFS uses to track the mental health services it provides
to clients. The file contained 51 columns and 8746 rows. The primary
source contained column heading such as: gender, program\_name,
program\_type, and facility.

Heartland Family Services. (2021). SLA Data Columns 2021-08-25 \[Data
file\]. Retrieved from
\[<https://unomaha.instructure.com/courses/50683/files/5217061?module_item_id=1562500>\]
(<https://unomaha.instructure.com/courses/50683/files/5217061?module_item_id=1562500>)

The secondary data source we used for our data cleaning was the SLA Data
Columns 2021-08-25.xlsx file from the HFS resources section of Canvas.
This file was also provide to us by Dr. Juarez. The file contains 3
columns and 52 rows. It provides the field names, notes, and
descriptions about the fields. From the SLA Data Columns file, we were
able to discern helpful information such as the meaning of the codes
provided for race.

# Intellectual Policy Constraints

Permission for us to use this information was given to us by Heartland
Family Services. Even though the information has been cleaned of
personal information, we are still bound to observe data privacy
standards.

# Metadata

The source we had for Metadata was the SLA Data Columns document
described in the preceding Data Sources section. This document contained
short descriptions of the fields, often just writing a longer
description of the field name but sometimes listing out the meanings of
codes used in the columns.

# Data Issues

-   gender\_identity - In the gender identity column, we found both the
    value of female and woman and felt this was duplicate, especially
    since we did not see an equivalent with the value of man. Only man
    was used and not male.

-   simple\_race - In the simple race column, we found values that did
    not correspond to the numeric codes provided in the SLA Data Columns
    document.

# Data Remediation

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

# Code

## Loading the package for select()

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

## To load the data

``` r
Full_HFS_Data <- read.csv("HFS Service Data.csv")
```

## To select the coloumns that are required for our analysis

``` r
HFS_1 <- select(Full_HFS_Data, c(program_name, facility, job_title, event_name, is_approved, NormalWorkHours, total_duration_num, recordID, age, simple_race, gender_identity))
```

## To select the data that has is\_approved = 1

``` r
HFS_2 <- subset(HFS_1, is_approved == 1) 
```

## To omit the data that has simple\_race = 21,65,5,9,17,20,81

``` r
HFS_Data <- subset(HFS_2,  simple_race != 21 & simple_race != 65 &simple_race != 5 & simple_race != 9 & simple_race != 17 & simple_race != 20  & simple_race != 81 )
```

## To replace the numeric codes in simple\_race to actual words.

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

## To add modify the column gender\_identity so that all the values having value “Female” are converted to “Woman”

``` r
HFS_Data$gender_identity[HFS_Data$gender_identity == "Female"] <- "Woman"
```

# To modify the values containing “NA” in data to “Not Obtained”

# PS: We have NA in only “gender\_identity” column

``` r
HFS_Data[is.na(HFS_Data)] <- "NA"
```

## To verify data has modified according to our needs

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

    ## [1] "NA"                      "Woman"                  
    ## [3] "Not Obtained"            "Man"                    
    ## [5] "Client Declined to Give" "Two-Spirit"             
    ## [7] "Non-Binary"              "Other"

``` r
unique(HFS_Data$is_approved)
```

    ## [1] 1

## To view the modified data

``` r
str(HFS_Data)
```

    ## 'data.frame':    7989 obs. of  11 variables:
    ##  $ program_name      : chr  "Mental Health" "Mental Health" "Mental Health" "Mental Health" ...
    ##  $ facility          : chr  "Center Mall Office" "Center Mall Office" "Center Mall Office" "Center Mall Office" ...
    ##  $ job_title         : chr  "THERAPIST II" "THERAPIST II" "THERAPIST II" "THERAPIST II" ...
    ##  $ event_name        : chr  "Collateral Note" "Individual Therapy" "Individual Therapy" "Individual Therapy" ...
    ##  $ is_approved       : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ NormalWorkHours   : chr  "Yes" "Yes" "Yes" "Yes" ...
    ##  $ total_duration_num: int  2 61 0 60 90 4 2 62 125 0 ...
    ##  $ recordID          : int  338 338 338 338 338 338 338 338 338 338 ...
    ##  $ age               : int  26 25 25 25 25 25 26 25 25 25 ...
    ##  $ simple_race       : chr  "Black / African-American" "Black / African-American" "Black / African-American" "Black / African-American" ...
    ##  $ gender_identity   : chr  "NA" "NA" "NA" "NA" ...

# Contributor Statement

Heather Davis, Scott Bui, and Akhil Kodali reviewed the data, analyzed
it, and decided what needed to be cleaned from the overall data and
which columns to keep that would be of most use for our research. Akhil
Kodali wrote the R code for the data cleaning, and Heather Davis wrote
the first draft of the Data Cleaning Documentation document. Scott Bui
proofed the document, updated the ReadMe with a link, and submitted it
for the assignment.
