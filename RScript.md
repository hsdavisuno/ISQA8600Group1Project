R Script
================

**Team 1 - Akhil Kodali, Heather Davis, Scott Bui**



# Data Cleaning

Same data cleaning we performed before with one slight modification
identified in the “To modify the values containing”NA" to “Not
Obtained” and "To select the columns that are required for our analysis" sections.

## Loading the packages for select() and plots

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

``` r
library(ggplot2)
```

## To load the data

``` r
Full_HFS_Data <- read.csv("HFS Service Data.csv")
```

## To select the columns that are required for our analysis

``` r
HFS_1 <- select(Full_HFS_Data, c(program_name, facility, job_title,staff_name, actual_date, event_name, is_approved, NormalWorkHours, total_duration_num, recordID, state,age, simple_race,ethnic_identity, gender_identity))
```

This time we included the state column.

## To select the data that has is\_approved = 1

``` r
HFS_2 <- subset(HFS_1, is_approved == 1) 
```

Here is\_approved = 1 means the people who are approved for the service.
So, only approved data and patients who have visited the service are
taken into consideration as there is no point in analyzing the data for
which the patients have not been treated.

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

## To modify the column gender\_identity so that all the values having value “Female” are converted to “Woman”

``` r
HFS_Data$gender_identity[HFS_Data$gender_identity == "Female"] <- "Woman"
```

## To modify the values containing “NA” to "Not Obtained

``` r
HFS_Data[is.na(HFS_Data)] <- "Not Obtained"
```

## To verify data has been modified according to our needs

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
    ## [5] "Two-Spirit"              "Non-Binary"             
    ## [7] "Other"

``` r
unique(HFS_Data$is_approved)
```

    ## [1] 1

## To view the modified data

``` r
str(HFS_Data)
```

    ## 'data.frame':    7989 obs. of  15 variables:
    ##  $ program_name      : chr  "Mental Health" "Mental Health" "Mental Health" "Mental Health" ...
    ##  $ facility          : chr  "Center Mall Office" "Center Mall Office" "Center Mall Office" "Center Mall Office" ...
    ##  $ job_title         : chr  "THERAPIST II" "THERAPIST II" "THERAPIST II" "THERAPIST II" ...
    ##  $ staff_name        : chr  "Carlson, Kaitlin" "Carlson, Kaitlin" "Carlson, Kaitlin" "Carlson, Kaitlin" ...
    ##  $ actual_date       : chr  "857" "682" "710" "696" ...
    ##  $ event_name        : chr  "Collateral Note" "Individual Therapy" "Individual Therapy" "Individual Therapy" ...
    ##  $ is_approved       : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ NormalWorkHours   : chr  "Yes" "Yes" "Yes" "Yes" ...
    ##  $ total_duration_num: int  2 61 0 60 90 4 2 62 125 0 ...
    ##  $ recordID          : int  338 338 338 338 338 338 338 338 338 338 ...
    ##  $ state             : chr  "NE" "NE" "NE" "NE" ...
    ##  $ age               : int  26 25 25 25 25 25 26 25 25 25 ...
    ##  $ simple_race       : chr  "Black / African-American" "Black / African-American" "Black / African-American" "Black / African-American" ...
    ##  $ ethnic_identity   : chr  "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino" "Not Spanish/Hispanic/Latino" ...
    ##  $ gender_identity   : chr  "Not Obtained" "Not Obtained" "Not Obtained" "Not Obtained" ...



# Plots

## Hours and Facilities, Faceted by Gender Identity

``` r
ggplot(data = HFS_Data) + 
geom_point(mapping = aes(x = facility, y = total_duration_num)) + 
    theme(axis.text.x = element_text(angle=90, vjust=1, hjust=1))+
facet_grid(. ~gender_identity)
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/hours%20and%20facilities%20with%20gender%20id%20facets-1.png)<!-- -->

In this visualization, we are observing whether we could identify some
trends by faceting by gender identity. We were unable to see much in the
facilities because there were too many variables. There is one pattern
visible, where Two-Spirit appears to be at one facility. For the next
steps on clarifying this visual, I would like to remove the upper end
outliers in the Woman column so the dots are clearly visible. I would
also like to aggregate the facilities to see better on what is happening
across facility types and facility locations.

## Hours and Gender with Facility Color Coding

``` r
ggplot(data = HFS_Data) + 
geom_col(mapping = aes(x = gender_identity, y = total_duration_num, color = facility))
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/Heather.png)<!-- -->

In the normal view of this visualization, it can be observed that people
who identify as women make up more of the total duration hours than any
other gender identity. Like the previous plot, I would like to explore
removing the top end outliers in the woman column and consolidate the
facilities by type and location to see if it may yield more patterns.

## Hours and Gender Identity Only

``` r
ggplot(data = HFS_Data) + 
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))+
geom_col(mapping = aes(x = gender_identity, y = total_duration_num))
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/hours%20and%20gender%20id%20bar-1.png)<!-- -->

Like the previous plot, this bar chart indicates that people who
identify as women make up more of the total duration hours than any
other gender identity. There may not be a need to further modify this
plot for observation.

## Hours and Gender with Program Name Legend

``` r
ggplot(data = HFS_Data) + 
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))+
geom_col(mapping = aes(x = gender_identity, y = total_duration_num, color = program_name) )
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/hours%20and%20gender%20with%20program%20name%20legend-1.png)<!-- -->

This bar chart indicates that most of the program provides services for
mental health and substance use, with the majority of services provided
being mental health. There isn’t a need to drill this information any
differently, as there are no further interesting patterns to discover.

## Hours and Race faceted by different programs

``` r
DurationRace <- ggplot(data=HFS_Data, aes(x=simple_race, y=total_duration_num))+
  facet_grid(.~program_name)+
geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle=60, vjust=, hjust=1))
options(scipen = 999)
DurationRace
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/Hours%20and%20Race%20with%20Program%20Name%20Facet-1.png)<!-- -->

From analysis, the group with the highest duration time utilized for the
service of mental health and substance abuse are the Caucasian group.
The second highest in both programs is Black/African-American. There is
very limited help that is needed in regards of Gambling in all race
groups.

## Hours and Age faceted by different programs

``` r
DurationAge <- ggplot(data=HFS_Data, aes(x=age, y=total_duration_num))+
geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))+
 facet_grid(.~program_name)
options(scipen = 999)

DurationAge
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/Hours%20and%20Age%20with%20Program%20Name%20Facet-1.png)<!-- -->

Mental health seems to be the highest and most populated health concern
for all age groups. We can take away that ages 7-10 has the highest
spike utilizing help and services. Coming in second for the mental
health category are years 28-30. Substance use has it’s highest marks in
the group’s late 20’s specifically ages 26-30.

## Count comparison of three program groups with different events

``` r
testing2 <- ggplot(data=HFS_Data, aes(x = event_name)) +
  geom_bar(fill = "#112446") +
      theme(axis.text.x = element_text(angle=70, vjust=1, hjust=1))+
facet_grid(.~program_name)
testing2
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/unnamed-chunk-1-1.png)<!-- -->

This exploration attempt was to see which event was being utilized the
most by which program. Gambling will be removed because there isn’t much
data in that category, so we’ll need to adjust the plot to remove that.
The mental health category will be the main focus. Substance abuse has
some information, until variables are cleaned up there cannot be a solid
observation with the graph alone. For the next iteration of this
visualization, a cleaner graph will be used to tell a better story, by
omitting the unneeded information.

## Box Plots of different programs w.r.t total duration

``` r
ggplot(HFS_Data, aes(program_name,total_duration_num,by1=recordID,fill=program_name)) + 
  geom_boxplot(aes(program_name), alpha=0.3) +
  xlab("Programs offered") +ylab("Total Duration")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/unnamed-chunk-2-1.png)<!-- -->

``` r
  theme(legend.position = "none")
```

    ## List of 1
    ##  $ legend.position: chr "none"
    ##  - attr(*, "class")= chr [1:2] "theme" "gg"
    ##  - attr(*, "complete")= logi FALSE
    ##  - attr(*, "validate")= logi TRUE

The above graph shows that Gambling has a very low average compared with
the Mental Health and Substance Use. Substance Use and Mental Health
have close to the same average number of hours for treatment. There are
some outliers in Mental Health, which explains why some patients are
taking more time compared to other programs. From this observation, we
can state that Gambling’s distribution is skewed high right. Mental
Health shows a highly left skewed distribution and Substance Use shows a
normal distribution of data.

## To determine the total number of hours spent by various age groups in all states

``` r
HFS_Data <- HFS_Data %>% mutate(agegroup = case_when(age >= 63  & age <= 72 ~ '63-72',
                                                     age >= 54  & age <= 63 ~ '54-63',
                                                     age >= 45  & age <= 54 ~ '45-54',
                                                     age >= 37  & age <= 45 ~ '37-45',
                                                     age >= 28  & age <= 36 ~ '28-36',
                                                     age >= 19  & age <= 27 ~ '19-27',
                                                     age >= 10  & age <= 18 ~ '10-18',
                                                     age >= 0  & age <= 8 ~ '1-9'))



ggplot(data = HFS_Data) +   
  
  geom_bar(stat = "identity",mapping = aes(x = agegroup, y = total_duration_num/60, fill =state)) +   
  
  facet_wrap( ~program_name,ncol=1) + labs(y= "Total duration in hours", x = "Different Age Groups")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/unnamed-chunk-3-1.png)<!-- -->

This bar chart appears interesting when we compare Iowa and Nebraska
with Mental Health and Gambling. The people in Iowa are taking more time
to normalize for Substance Use and the people in Nebraska are taking
more time for Mental Health. Next time, this plot can be drawn using
another parameter record\_id, as this may help enable return of actual
information based on individuals, rather than comparing all.

## Total visits by years from the start of service for different program names by different facilities

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



nofvisits <- data.frame(count(HFS_Data,actual_date,program_name,facility))
ggplot(data = nofvisits,aes(x=actual_date,y=n,fill=facility,na.rm=TRUE))+geom_bar(stat="identity")+facet_wrap(~program_name) 
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/Akhil.png)<!-- -->

In this plot, we can see that the total number of visits are decreasing,
which can be considered a good sign as the visits are decreasing year by
year. In the next task for the extension of this, we can find out if it
is the people or the visits that are decreasing using record_id.

## Number of staff over the years

``` r
da<-HFS_Data %>%
  count(actual_date,staff_name,job_title)

da2<-as.data.frame(table(da$actual_date,da$job_title))

ggplot(data=da2, aes(x=Var1,y=Freq))+geom_bar(stat="identity")+labs(y= "Total Resources", x = "Age Groups")
```

![](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/Rscript/unnamed-chunk-5-1.png)<!-- -->

From the above plot we can observe that the working staff also kept on
decreasing for years through which we can think that HFS is working more
towards saving their funds.



# Contributor Statement

Heather Davis, Scott Bui, and Akhil Kodali each created plots. Heather
Davis wrote the first draft of the markdown document, Scott Bui added
the remaining code and Akhil uploaded the plots to it. Heather Davis proofed the document,
updated the ReadMe with a link, and submitted it for the assignment.
