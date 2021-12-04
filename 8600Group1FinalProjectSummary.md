# ISQA 8600 Final Project Summary – Mental Health Service Encounter Hours by Age, Gender Identity, and Race and Facility

# Group 1 – Akhil Kodali, Heather Davis, Scott Bui

## Introduction

Our research study analyzes the relationship of mental health service encounter hours by age, gender identity, and race at types of facilities run by a U.S. midwestern human services organization, Heartland Family Service (HFS). HFS is a private, non-profit, non-sectarian organization founded in 1875 that provides service in Iowa and Nebraska (Heartland Family Service, n.d., 2020 Annual Report). The mission of HFS is &quot;to strengthen individuals and families in our community through education, counseling, and support services&quot; (Heartland Family Service, n.d., Our Mission).

## Analysis Decision Targets or Support

We researched the relationship of age, gender identity, and race with the hours of mental health service provided to clients by HFS facilities segmented by facility type. Our research question was: Is there a correlation between demographics (age, gender identity and race) and average service visit duration time? What demographics have longer average service visit duration time requirements than others?

Our research target was to provide HFS with information about how mental health service encounter hours were provided across different client demographics by facility. We felt this research might help HFS support the following decisions:

- Plan targeted staff schedule and staff allocation – If certain demographics and/or facilities required more time for service encounters, this could be considered by the organization in its staff scheduling and allocation.
- Further research – Depending on how the facilities were meeting their planed service encounter hours, HFS could investigate differences between facilities to see if other types of interventions could be applied to alleviate situations where the facilities were exceeding the goals for service encounter hours.
- Consider targeted and focused diversity and inclusion and cultural staff training resources – If certain demographics and/or facilities required more time for service encounters, training efforts could be focused on the facilities that supported those demographics.

## Data Cleaning Choices and Relevance

This section describes the choices we made with our data and their relevance to the analysis target.

### Data Sources

- Heartland Family Services. (2021). HFS Service Data [Data file]. Retrieved from: [https://unomaha.instructure.com/courses/50683/files/5256675?module\_item\_id=1562501](https://unomaha.instructure.com/courses/50683/files/5256675?module_item_id=1562501)

    The primary data source we used for our data cleaning was the HFS Service Data.csv file from the HFS resources section of Canvas. This file was provided to our class by Dr. Clayton Juarez, Ph.D. of Heartland Family Services (HFS) in Omaha, Nebraska. Dr. Juarez pulled the data from a system HFS uses to track the mental health services it provides to clients. The file contained 51 columns and 8746 rows.

- Heartland Family Services. (2021). SLA Data Columns 2021-08-25 [Data file]. Retrieved from: [https://unomaha.instructure.com/courses/50683/files/5217061?module\_item\_id=1562500](https://unomaha.instructure.com/courses/50683/files/5217061?module_item_id=1562500)

    The secondary data source we used for our data cleaning was the SLA Data Columns 2021-08-25.xlsx file from the HFS resources section of Canvas. This file was also provided to us by Dr. Juarez. The file contains 3 columns and 52 rows. It provides the field names, notes, and descriptions about the fields.

### General Data Remediation

We performed the following actions to generally clean our data for our use:

- Removed columns we did not need for our research.
- We only kept the rows from the data that were approved by the supervisor.
- Although HFS provides services to individuals located in multiple states, we narrowed the focus of our research only to those located in Iowa and Nebraska, where the facilities are located.

### Mental Health

After analyzing the data by program types (Mental Health, Gambling, and Substance Use), we determined that HFS provides most of its service hours in this program area compared to the other areas. We removed some unwanted statistical outlier hours from Mental Health duration hours.

### Service Visit Duration

From the beginning of our project, we examined the relationships between age, gender identity, and race and service hours. At first, we examined the relationship of the three demographics to total duration, as this was a sum of the service encounter, planning, and travel time. We felt that focusing on total duration would encompass the amount of time the staff was being paid to provide the service, thereby making cost estimation easier. Over time, we decided to hone this measure in two ways:

- Narrow the focus from total duration to duration, which the SLA indicated as &quot;Duration of service encounter&quot; (Heartland Family Service, (2021), SLA Data Columns). By doing this, we moved from a cost-estimation focus to a focus on the actual time the client was receiving service, which we felt was more of a quality measure by removing the non-value-added time from the delivery of service.
- Narrow the focus of counting hours to average hours to better represent the sample.

### Facility

Because we were provided 25 different facilities and wanted to see if we could identify trends among facility types, we grouped them into the following categories:

- Center – We grouped this on its own because it did not appear to have a particular associated type in the label.
- HFS-Other – We grouped these together because they were all HFS-labelled locations.
- HFS-Gendler – We separated this location from the other HFS locations because it showed the highest total duration hours.
- Micah House – This location did not have an identifiable type for us to group it with.
- North Omaha Intergenerational Campus (NOIC) – This location did not have an identifiable type for us to group it with.
- Reporting Centers (RepCenter) – We grouped these together because they were all reporting center labelled locations.
- Sanctuary Houses (SancHouse) – We grouped these together because they were all sanctuary house labelled locations.
- School – We grouped these together because they were all school labelled locations.


### Gender Identity

We changed the value of female to woman to correspond to the value of man because male was not used for individuals who identified themselves as men. We also changed the value to Not Obtained to NA since we felt that Not Obtained and NA were very similar. In the gender identity column, we found both the value of female and woman and felt this was duplicate, especially since we did not see an equivalent with the value of man. Only man was used and not male.

### Race

We removed the rows in the Simple Race data that did not correlate with the values provided in the SLA Data Columns document for race, as we felt these were likely mis-keyed values. We also replaced the numeric codes for race with their label values to make them easier to understand. For example, the value 2 was replaced with Alaskan Native. The data indicating two or more races was such a small number of clients, it was removed from our data.

## Results and Interpretations

The following section discusses our results and interpretations, and is broken down into three plots by age, gender identity, and race.

### Plot 1 – Mental Health Service Encounter Hours by Age and Facility

According to the Centers for Disease Control and Prevention (CDC) and National Association of Chronic Disease Directors, in their report &quot;The State of Mental Health and Aging in America Issue Brief 1: What Do the Data Tell Us?&quot;, the percentage of adults aged 50 years or older who in the past 30 days experienced frequent mental distress in Nebraska was 7.24-8.52% and in Iowa was 0-7.23% (Centers for Disease Control and Prevention and National Association of Chronic Disease Directors, 2008, p. 5). Based on this data, we might expect to see a lower rate of mental health hours required for Iowa clients than Nebraska clients among older age groups.

Additionally, according to the National Institute of Mental Health (NIMH), young adults (18-25) with any mental illness and the percentage of those young adults that received mental health service was lower than those in older generations (National Institute of Mental Health, n.d.). Based on this data, we might expect to see young adults receiving a greater average amount of service encounter hours than other age groups.

The plot that follows shows the results of our analysis of average service visit duration time by age and facility and state.

![Plot of average service visit duration time by age and facility and state](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-16-1.png)

In those facilities that serve both Iowa and Nebraska older clients, we do not see a significant difference between the average duration of the service encounters. Overall, regardless of age group, Iowa clients appear to require more service time than those in Nebraska. We also do not see a significant increase in average duration of service encounters for younger clients. Perhaps, because younger clients are less likely to receive mental health services than other age groups, they are not present in the data. The only significance we took away from the data is that the average service encounter time is more closely associated with facility type than age – with the biggest difference between schools and reporting centers, followed by schools and sanctuary houses.

### Plot 2 – Mental Health Service Encounter Hours by Gender Identity and Facility

According to the article &quot;Gender and Mental Health&quot;, produced by the World Health Organization, U.S. women &quot;reported higher levels of distress than did men, and were more likely to perceive having an emotional problem than men who had a similar level of symptoms.&quot; (World Health Organization, 2002, p.3). Additionally, according to the National Institute of Mental Health (NIMH), the prevalence of AMI was &quot;higher among females (24.5%) than males (16.3%)&quot; and &quot;more females with AMI (49.7%) received mental health services than males with AMI (36.8%).&quot; (National Institute of Mental Health, n.d.). Non-binary individuals were not accounted for in the study. Based on this data, we might see a higher average service duration for women than men in our results.

The plot that follows shows the results of our analysis of average service visit duration time by gender identity and facility and state.

![Plot of service visit duration time by gender identity and facility and state](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-17-1.png)

When comparing average service duration for men and women, they do not vary more than 10 points, except at the reporting centers, where women have an over twenty-point difference. There is also a twenty-point difference between non-binary clients and women and even more significant difference between non-binary clients and men at the HFS-Gendler location. Non-binary clients did not represent a significant difference elsewhere, but they were not identified at all locations. Again, the most significant differences between clients seems to be based on facility type – with the biggest difference between schools and reporting centers, followed by schools and sanctuary houses.

### Plot 3 – Mental Health Service Encounter Hours by Race and Facility

According to the National Institute of Mental Health (NIMH) &quot;the prevalence of AMI was highest among the adults reporting two or more races (31.7%), followed by White adults (22.2%). The prevalence of AMI was lowest among Asian adults (14.4%).&quot; (National Institute of Mental Health, n.d.). The data indicating two or more races was such a small number of clients, it was removed from our data. So, we expected to see perhaps Caucasian being the longest average duration of service hours and Asian being the shortest.

The plot that follows shows the results of our analysis of average service visit duration time by race and facility and state.

![Plot of average service visit duration time by race and facility and state](https://github.com/hsdavisuno/ISQA8600Group1Project/blob/main/IntegratedCompleteProject/unnamed-chunk-18-1.png)

Looking at the data across races, Native Americans appear to receive the most average hours of service encounters, by a significant margin especially at the Center, HFS-Other, and reporting centers. Among Native American clients at the HRS-Other locations, Native Americans from Iowa receive more than double the average amount of service encounter hours than Nebraska Native Americans. There also appears to be a significant difference between Caucasians from Iowa and Nebraska at Micah house, but that might be somehow related to Micah house being in Iowa. Asians were only represented at the HFS-Other locations, and they use a slightly higher average than other races among the HFS-Other locations, except for Native Americans. Again, the most significant differences between clients seems to be based on facility type – with the biggest difference between schools and reporting centers, followed by schools and sanctuary houses.

## Conclusions

After researching the relationship of age, gender identity, and race with the hours of mental health service provided to clients by HFS facilities segmented by facility type, our research indicated very little variance in average service hours by age or gender. Most of the variance in average service encounter duration appears to be between facility types. Schools have the shortest duration and reporting centers have the highest. Sanctuary houses are just behind reporting centers. If this is out of alignment with HFS expectations or standards, the organization might:

- Perform further research into the underlying circumstances where facilities might require longer average duration service encounters
- Investigate if reporting centers and sanctuary houses might need more resources
- Plan staff allocation and schedules differently at those locations

There also appears to be a trend of higher average duration service encounter hours with Native American clients at Center, HFS-Other, and reporting center facility types. HFS might use this data to:

- Perform further research into the underlying circumstances where facilities might require longer average duration service encounters and look at the HFS-Other facilities to see if significant differences can be identified among them.
- Investigate if reporting centers and sanctuary houses might need more resources
- Plan staff allocation and schedules differently at those locations
- Consider allocating Native American cultural training to those locations is appropriate.


To a lesser degree, there might be a trend in gender identity that might be worth researching further. Men and women at the reporting centers have an over twenty-point difference. At HFS-Gendler, there is also a twenty-point difference between non-binary clients and women and even more significant difference between non-binary clients and men. 


## References

Centers for Disease Control and Prevention and National Association of Chronic Disease Directors. (2008). _The State of Mental Health and Aging in America Issue Brief 1: What Do the Data Tell Us?_ Atlanta, GA. Retrieved from: [https://www.cdc.gov/aging/pdf/mental\_health.pdf](https://www.cdc.gov/aging/pdf/mental_health.pdf)

Heartland Family Service (2020). _2020 Annual Report_. Retrieved from: [https://www.heartlandfamilyservice.org/wp-content/uploads/2021/06/2020HFS-Annual-Report-FINAL-low-res-for-email.pdf](https://www.heartlandfamilyservice.org/wp-content/uploads/2021/06/2020-HFS-Annual-Report-FINAL-low-res-for-email.pdf)

Heartland Family Service. (2021). _HFS Service Data_ [Data file]. Retrieved from: [https://unomaha.instructure.com/courses/50683/files/5256675?module\_item\_id=1562501](https://unomaha.instructure.com/courses/50683/files/5256675?module_item_id=1562501)

Heartland Family Service. (2021). _SLA Data Columns 2021-08-25_ [Data file]. Retrieved from: [https://unomaha.instructure.com/courses/50683/files/5217061?module\_item\_id=1562500](https://unomaha.instructure.com/courses/50683/files/5217061?module_item_id=1562500)

Heartland Family Service. (n.d.). _Locations_. Retrieved from [https://www.heartlandfamilyservice.org/locations/](https://www.heartlandfamilyservice.org/locations/)

Heartland Family Service. (n.d.). _Our Mission_. Retrieved from [https://www.heartlandfamilyservice.org/our-mission/](https://www.heartlandfamilyservice.org/our-mission/)

National Institute of Mental Health (NIMH). (n.d.). _Mental illness_. Retrieved from [https://www.nimh.nih.gov/health/statistics/mental-illness](https://www.nimh.nih.gov/health/statistics/mental-illness)

World Health Organization. _Gender and Mental Health_. Geneva, Switzerland. Department of Gender and Women&#39;s Health, 2002. Retrieved from: [https://www.who.int/gender/other\_health/genderMH.pdf](https://www.who.int/gender/other_health/genderMH.pdf)