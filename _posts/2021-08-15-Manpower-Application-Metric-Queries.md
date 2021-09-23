---
layout: post
title: Manpower Application Metric Queries
image: "/posts/Park2.jpg"
tags: [mySQL, KPI]
---
# Summary


# Project Introduction & Impact

Manpower reporting is a critical indicator of potnetial progress on construction sites. Knowing how many personnel are on site, and what they are doing, can allow the team to understand all the work taking place and its associated production rate on site. This information can be used to determine if a project is on time, and allows the project team to follow up with subcontractors to increase manpower on site and recover time on the project.

Though manpower tracking is important, on many construction projects reporting isn't fully completed for one of several reasons:
1. Subcontractors do not submit daily manpower logs and details to the general contractor
2. The general contractor team does not have time to sort through all the paperwork

To remedy the above issues, in tandem with a co-worker, I created the frameworks for the visuals and reporting features in a new reporting application.  
 
I helped design a user-friendly interface that allowed end users to easily input data daily 
into a database. This was a critical component of the application because without user support 
and input, the app would not be used and there would be no data to analyze by my group.  
 
Once a successful data entry system was created, I helped create a Report Approval 
module to clean the subcontractor-input data. The module allowed staff to view submitted 
data, edit as needed to ensure accuracy, and approve the final report submissions. Without this 
feature, inaccurate data could be used in final analytics and skew manpower metrics. 

With this in place, I helped develop analytical tables for our project staff. The key table I 
developed displayed approval status and summary manpower values. A mockup of this table is 
shown below. Each column represents a date, and each row represents the manpower of a given 
subcontractor per day. If a subcontractor submits a report, that day’s cell turns green for that 
contractor. If a subcontractor is supposed to be working but did not submit a report their cell will 
appear red. A cell appears white if no workers are needed that day.  
 
The table allows project managers to view manpower trends on a project for any date range at a 
glance, a feature not available on previously used software. With these trends, project managers 
can now more easily do the following:

1. Determine which subcontractors are scheduled to be on site, and why scheduled 
contractors are not on site. This helps managers determine whether a project is on-time, 
and create mitigation plans as needed to recover any lost time. 
 
2. Contact subcontractors to inquire about insufficient manpower. Having a concise table of 
manpower allows project managers to quickly determine whether any subcontractors 
have critically low manpower that could slow down progress. 
 
3. Estimate total hours worked on the project for safety record reporting. Prior to the 
creation of this table, managers could spend hours or days performing similar estimates 
and paperwork for official, mandated records. 
 
4. Determine which daily reports are not approved, and ensure reports subsequently get 
approved. Without approval, executive leadership can’t be sure of the report’s accuracy. 




---

To remedy this, our team developed a manpower reporting system with two key features to address the above issues:
1. Provided subcontractors a user-friendly interface to input their manpower and select tasks each team was working on
2. Provided summary sheets for the general contractor team, so they could review and approve manpower report entries

With this system in place, the team believed that each project team would save ~5 hours per week on manpower tracking, if not more. This allows construction teams to spend more time in the field proactively solving problems and managing the site.

After creating the above, the team wanted to create metrics to track progress and improvements in manpower reporting. 

---

With this basic system in place, the team came up with several metrics to track 


---

For the latter half of 2019 and January 2020, I co-managed an initiative to 
improve daily manpower reporting on construction sites. The focus of the team was to create an 
app that allows subcontractors to: 

1. Enter how many people were working that day 
2. Describe which activities were worked on that day 
 
In tandem with a co-worker, I created the frameworks for the visuals and reporting features in 
the application.  
 
To start, I helped design a user-friendly interface that allowed end users to easily input data daily 
into a database. This was a critical component of the application because without user support 
and input, the app would not be used and there would be no data to analyze by my group.  
 
Once a successful data entry system was created, I helped create and modify a Report Approval 
module to review the subcontractor-input data. The module allowed staff to quickly view submitted 
data, discuss inaccuracies with subcontractors, and approve the final report submissions.

With this in place, I helped develop analytical tables for our project staff. The key table 
displayed approval status and total manpower values. The table allows project managers to view manpower trends on a project for any date range at a 
glance, a feature not available on previously used software. With these trends, project managers 
can now more easily do the following:

<!-- Each column represents a date, and each row represents the manpower of a given 
subcontractor per day. If a subcontractor submits a report, that day’s cell turns green for that 
contractor. If a subcontractor is supposed to be working but did not submit a report their cell will 
appear red. A cell appears white if no workers are needed that day.   -->
 
1. Determine which subcontractors are scheduled to be on site, and why scheduled 
contractors are not on site.
 
2. Contact subcontractors to inquire about insufficient manpower, so recovery plans can be put in place.
 
3. Estimate total hours worked on the project for safety record reporting, saving hours of monthly reporting work.
 
4. Determine which daily reports are not reviewed, confirm the accuracy of those reports, and approve them efficiently.

# Metric Selection

After creating the reporting system above, the team wanted to create performance measures 
to track how project teams were using the app. This would allow executive 
leadership to determine whether the app was successful as a whole and monitor individual 
project performance. I developed 5 metrics as part of this process. These metrics were as 
follows: 

1. Accuracy - measures whether subcontractors are submitting accurate reports, judged by 
determining whether our personnel have edited any of the information submitted by 
subs. This is critical because if our team has to edit many entries, then we are not saving as 
much time as anticipated by using the new system.  
 
2. Compliance - measures the number of subcontractors that are submitting reports, as a 
percentage of subs that are expected to be working. This is critical because if subs are not 
using the new app, then the time spent developing it may have been wasted and it may 
not be worth Clark's time and money to maintain and continue to build out the new app. 
 
3. Timeliness - measures the percentage of subcontractors that submit a daily report within 
48 hours of the day activities took place. As time goes on, subs are more likely to forget 
the details of what happened on previous days and having this statistic can help teams 
ensure reports are submitted on time and are hopefully more comprehensive as a result. 
 
4. Timely Compliance - measure combines #2 and #3. This helps teams determine the 
number of subs submitting on time as a percentage of how many total reports they are 
expecting. This is a more complete measure of compliance. 
 
5. Validation - measures the percent of reports the Clark team has approved. If reports are 
not approved, they also have likely not been reviewed by Clark. This is critical because if 
Clark is not validating daily submissions, then inaccurate or incomplete daily reports may 
not be check. 
 

# Metric Calculation

## Accuracy

```mySQL
/*query to determine accuracy - how many entries had clark overrides */
/*Query structure: 
-sum case when Clark has overridden any aspect of the daily report
-sum total reports submitted
-divide overidden reports by total submitted reports

SELECT sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		CONVERT(DECIMAL(4,2),100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL)) As Percent_Accurate
FROM [RDODS].[DTL] */

/*==================================================================================================*/

/*query to determine accuracy by week - how many entries had clark overrides */
/*Query structure: 
-sum cases when Clark has not over-ridden data input by subcontractors, grouped by week
-count the total number of submitted reports
-divide the above measures to find the reports that were accurately

SELECT DATEPART(ww,DDRMaster.DateActivity) AS Week,DATEPART(yyyy,DDRMaster.DateActivity) as Yr,sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		LEFT(100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL),6) As Weekly_Report_Accuracy
FROM [RDODS].[DDPM].[DDRDetail]
INNER JOIN [RDODS].[DDPM].[DDRMASTER] ON DDRDetail.MasterID = DDRMaster.ID
GROUP BY  DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity)
ORDER BY DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity);*/

/*========================================================================================================================*/


/*Cumulative Report Accuracy Query
Query structure:
-Join the code used above to find weekly accuracy % to itself on the following conditions
-the week/year of the value in the left table is greater or equal to that of the right table
-this join allows us to sum total reports to date for each week, and divide the number of reports
-that had no over-rides by the total number of reports. this is the cumulative accuracy to date

 SELECT T1.Week, T1.Yr, LEFT(100*Sum(T2.Accurate_Reports)/Sum(T2.Total_Submitted_Reports),5) AS Cumulative_Report_Accuracy FROM 
(SELECT DATEPART(ww,DDRMaster.DateActivity) AS Week,DATEPART(yyyy,DDRMaster.DateActivity) as Yr,sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		LEFT(100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL),6) As Percent_Accurate
FROM [RDODS].[DDPM].[DDRDetail]
INNER JOIN [RDODS].[DDPM].[DDRMASTER] ON DDRDetail.MasterID = DDRMaster.ID
GROUP BY  DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity)) T1

JOIN


(

SELECT DATEPART(ww,DDRMaster.DateActivity) AS Week,DATEPART(yyyy,DDRMaster.DateActivity) as Yr,sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		LEFT(100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL),6) As Percent_Accurate
FROM [RDODS].[DDPM].[DDRDetail]
INNER JOIN [RDODS].[DDPM].[DDRMASTER] ON DDRDetail.MasterID = DDRMaster.ID
GROUP BY  DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity)) T2

on (T1.Week >= T2.Week) and (T1.Yr = T2.Yr)
GROUP BY T1.Week, T1.Yr 
order by T1.Week, T1.Yr
*/




/*===========================================================================================================================*/

/*Final query structure:
-query below joins the two queries shaded above
-this creates a table that shows accuracy for a given week and cumulative accuracy to date for that week
*/

Select R1.Week,R1.Yr,R1.Weekly_Report_Accuracy,R2.Cumulative_Report_Accuracy FROM
/*Combining queries above. Now will have weekly and cumulative averages in the same table*/
(

SELECT DATEPART(ww,DDRMaster.DateActivity) AS Week,DATEPART(yyyy,DDRMaster.DateActivity) as Yr,sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		LEFT(100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL),6) As Weekly_Report_Accuracy
FROM [RDODS].[DDPM].[DDRDetail]
INNER JOIN [RDODS].[DDPM].[DDRMASTER] ON DDRDetail.MasterID = DDRMaster.ID
GROUP BY  DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity)

) R1

JOIN

(

  SELECT T1.Week, T1.Yr, LEFT(100*Sum(T2.Accurate_Reports)/Sum(T2.Total_Submitted_Reports),5) AS Cumulative_Report_Accuracy FROM 
(SELECT DATEPART(ww,DDRMaster.DateActivity) AS Week,DATEPART(yyyy,DDRMaster.DateActivity) as Yr,sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		LEFT(100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL),6) As Percent_Accurate
FROM [RDODS].[DDPM].[DDRDetail]
INNER JOIN [RDODS].[DDPM].[DDRMASTER] ON DDRDetail.MasterID = DDRMaster.ID
GROUP BY  DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity)) T1

JOIN


(

SELECT DATEPART(ww,DDRMaster.DateActivity) AS Week,DATEPART(yyyy,DDRMaster.DateActivity) as Yr,sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) AS Accurate_Reports,
		CAST(COUNT(*) AS DECIMAL) As Total_Submitted_Reports,
		LEFT(100*sum(case when ClarkOverride_ActivityDescription is Null 
		AND ClarkOverride_ActivityID is Null 
		AND ClarkOverride_Notes is Null 
		AND ClarkOverride_ShiftLength is Null 
		AND ClarkOverride_TotalHours is Null 
		AND ClarkOverride_Workers is Null
		then 1 else 0 end) / CAST(COUNT(*) AS DECIMAL),6) As Percent_Accurate
FROM [RDODS].[DDPM].[DDRDetail]
INNER JOIN [RDODS].[DDPM].[DDRMASTER] ON DDRDetail.MasterID = DDRMaster.ID
GROUP BY  DATEPART(ww,DDRMaster.DateActivity),DATEPART(yyyy,DDRMaster.DateActivity)) T2

on ((T1.Week >= T2.Week) and (T1.Yr = T2.Yr)) OR (T1.YR>T2.YR)
GROUP BY T1.Week, T1.Yr 

) R2

on (R1.Week=R2.Week) and (R1.Yr = R2.Yr)
WHERE R1.Yr > 1901
order by R1.Yr, R1.Week
```
# Conclusion
