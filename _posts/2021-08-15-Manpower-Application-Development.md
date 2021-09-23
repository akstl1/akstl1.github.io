---
layout: post
title: Manpower Application Development
image: "/posts/Park2.jpg"
tags: [mySQL, KPI]
---
# Summary

To be fleshed out further

# Project Introduction & Impact

Manpower reporting is a critical indicator of potnetial progress on construction sites. Knowing how many personnel are on site, and what they are doing, can allow the team to understand all the work taking place and its associated production rate on site. This information can be used to determine if a project is on time, and allows the project team to follow up with subcontractors to increase manpower on site and recover time on the project.

Though manpower tracking is important, on many construction projects reporting isn't fully completed for one of several reasons:
1. Subcontractors do not submit daily manpower logs and details to the general contractor
2. The general contractor team does not have time to sort through all the paperwork

To remedy the above issues, in tandem with a co-worker, I created the frameworks for the visuals and reporting features in a new reporting app.  
 
I helped design a user-friendly interface that allowed end users to easily input data daily 
into a database. This was a critical component of the application because without user support 
and input, the app would not be used and there would be no data to analyze by my group.  
 
Once a successful data entry system was created, I helped create and modify a Report Approval 
module to review the subcontractor-input data. The module allowed staff to quickly view submitted 
data, discuss inaccuracies with subcontractors, and approve the final report submissions.

With this in place, I helped develop analytical tables for our project staff. The key table 
displayed approval status and total manpower values. The table allows project managers to view manpower trends on a project for any date range at a 
glance, a feature not available on previously used software. With these trends, project managers 
can now more easily do the following:

<!-- A mockup of this table is 
shown below. Each column represents a date, and each row represents the manpower of a given 
subcontractor per day. If a subcontractor submits a report, that day’s cell turns green for that 
contractor. If a subcontractor is supposed to be working but did not submit a report their cell will 
appear red. A cell appears white if no workers are needed that day.   -->

1. Determine which subcontractors are scheduled to be on site, and why scheduled 
contractors are not on site.
 
2. Contact subcontractors to inquire about insufficient manpower, so recovery plans can be put in place.
 
3. Estimate total hours worked on the project for safety record reporting, saving hours of monthly reporting work.
 
4. Determine which daily reports are not reviewed, confirm the accuracy of those reports, and approve them efficiently.

<!-- ---

To remedy this, our team developed a manpower reporting system with two key features to address the above issues:
1. Provided subcontractors a user-friendly interface to input their manpower and select tasks each team was working on
2. Provided summary sheets for the general contractor team, so they could review and approve manpower report entries

With this system in place, the team believed that each project team would save ~5 hours per week on manpower tracking, if not more. This allows construction teams to spend more time in the field proactively solving problems and managing the site.

After creating the above, the team wanted to create metrics to track progress and improvements in manpower reporting. 

---

With this basic system in place, the team came up with several metrics to track 


--- -->

<!-- For the latter half of 2019 and January 2020, I co-managed an initiative to 
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
can now more easily do the following: -->

<!-- Each column represents a date, and each row represents the manpower of a given 
subcontractor per day. If a subcontractor submits a report, that day’s cell turns green for that 
contractor. If a subcontractor is supposed to be working but did not submit a report their cell will 
appear red. A cell appears white if no workers are needed that day.   -->
 
<!-- 1. Determine which subcontractors are scheduled to be on site, and why scheduled 
contractors are not on site.
 
2. Contact subcontractors to inquire about insufficient manpower, so recovery plans can be put in place.
 
3. Estimate total hours worked on the project for safety record reporting, saving hours of monthly reporting work.
 
4. Determine which daily reports are not reviewed, confirm the accuracy of those reports, and approve them efficiently. -->

# Metric Selection

After creating the reporting system above, the team wanted to create performance measures 
to track how project teams were using the app. This would allow executive 
leadership to determine whether the app was successful as a whole and monitor individual 
project performance. I developed 5 metrics as part of this process. These metrics were as 
follows: 

1. Accuracy - measures whether subcontractors are submitting accurate reports; an 'accurate' report was deemed to be one that didn't require further editing/clarification after initial review. This is critical because if many entries required editing, then we are not saving as 
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

```sql
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
## Compliance

```sql


/*Query to find total scheduled submissions and total non-duplicate submissions received
Structure:
-count total daily report submissions per day by unique subcontractor numbers
-unique submissions are determined as follows
--a table is created to count submissions per day
--if 1 submission is made by a party with a given contract #, count will equal 1
--if there are multiple submissions by parties with the same contract #, then the count will be changed to equal 1. this removes counting multuiple submissions by a single party per day.

*/

/*
SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)
order by T1.YR, T1.WK
*/

/*======================================================================================================================================*/
/*======================================================================================================================================*/


/*Query to find total cumulative scheduled submissions and total non-duplicate submissions received
Structure:
-Join the code used above to find weekly accuracy % to itself on the following conditions
-the week/year of the value in the left table is greater or equal to that of the right table
-this join allows us to sum total reports to date for each week, and divide the number of reports
-that had no over-rides by the total number of reports. this is the cumulative compliance to date
*/

/*
 SELECT R1.WK, R1.YR,LEFT(100*Sum(R2.Total_Submissions_No_Duplicates)/Sum(R2.[Total Submissions Schduled]),5) AS Compliance_Percentage_Cumulative FROM 
(


SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)



) R1

JOIN


(




SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)




) R2

on ((R1.WK >= R2.WK) and (R1.YR = R2.YR)) OR (R1.YR>R2.YR)
GROUP BY R1.WK, R1.YR 
order by R1.YR, R1.WK
*/


/*======================================================================================================================================*/
/*======================================================================================================================================*/


/*Query to find total scheduled submissions and total non-duplicate submissions received. Joining Weekly and cumulative % tables to see both these values for a given week*/





Select R1.WK,R1.YR,R1.Compliance_Percentage_Weekly,R2.Compliance_Percentage_Cumulative FROM
/*Combining queries above. Now will have weekly and cumulative averages in the same table*/
(
SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)


) R1

JOIN

(

 SELECT R1.WK, R1.YR,LEFT(100*Sum(R2.Total_Submissions_No_Duplicates)/Sum(R2.[Total Submissions Schduled]),5) AS Compliance_Percentage_Cumulative FROM 
(


SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)



) R1

JOIN


(




SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)




) R2

on ((R1.WK >= R2.WK) and (R1.YR = R2.YR)) OR (R1.YR>R2.YR)
GROUP BY R1.WK, R1.YR 

) R2

on (R1.WK=R2.WK) and (R1.YR = R2.YR)
WHERE R1.YR > 1901
order by R1.YR, R1.WK
```

## Timeliness

```sql
/*Queries to determine whether contractors submitted reports within 48 hours of peforming work

structure:
-case statements sum report instances when the report timestamp is less than two days away from the date that activities were performed
-the above value is divided by the total number of reports that were submitted for that day
-results were grouped by week

SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity != '1900-01-01'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  

*/

/*=========================================================================================================*/

/*Cumulative Timeliness Query

-Join the code used above to find weekly accuracy % to itself on the following conditions
-the week/year of the value in the left table is greater or equal to that of the right table
-this join allows us to sum total reports to date for each week, and divide the number of reports
-that had no over-rides by the total number of reports. this is the cumulative timeliness to date



SELECT T1.WK,
	   T1.YR,
	   LEFT(100*Sum(T2.[Reports Sumbitted on Time])/Sum(T2.[Total Reports Submitted]),5) AS Cumulative_Report_Timeliness

FROM

(

SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity != '1900-01-01'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity != '1900-01-01'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  

) T2

on ((T1.WK >= T2.WK) and (T1.YR = T2.YR)) OR (T1.YR>T2.YR)
GROUP BY T1.WK, T1.Yr 
order by T1.WK, T1.Yr


*/

/*==========================================================================================================*/

/*
Final query joins the above two queries into one table, so that weekly and cumulative values can be seen side-by-side, and so all values can be exported to Tableau and graphed more easily.
*/
Select R1.WK,R1.YR,R1.On_Time_Percent_Weekly,R2.Cumulative_Report_Timeliness FROM
/*Combining queries above. Now will have weekly and cumulative averages in the same table*/
(

SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity != '1900-01-01'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  

) R1

JOIN

(

SELECT T1.WK,
	   T1.YR,
	   LEFT(100*Sum(T2.[Reports Sumbitted on Time])/Sum(T2.[Total Reports Submitted]),5) AS Cumulative_Report_Timeliness

FROM

(

SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity != '1900-01-01'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity != '1900-01-01'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  

) T2

on ((T1.WK >= T2.WK) and (T1.YR = T2.YR)) OR (T1.YR>T2.YR)
GROUP BY T1.WK, T1.Yr 

) R2

on (R1.WK=R2.WK) and (R1.YR = R2.YR)
WHERE R1.YR > 1901
order by R1.YR, R1.WK
```

## Timely Compliance

```sql

/*Query to find total  submissions thjat were on time and submitted by unique subs*/

/* Compliance query copy - see Compliance Query original file for writeup

SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM
(
SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM
(

SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber

) R1
group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)

) T1

JOIN

(

SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)

 ) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)
order by T1.YR, T1.WK
*/



/*========================================================================================================================================================================================*/
/*========================================================================================================================================================================================*/
/*========================================================================================================================================================================================*/
/*========================================================================================================================================================================================*/

/*timeliness query copy - see original Timeliness query code for writeup
SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity > '2019-06-11'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)  
*/

/*========================================================================================================================================================================================*/
/*========================================================================================================================================================================================*/
/*========================================================================================================================================================================================*/
/*========================================================================================================================================================================================*/

SELECT R1.WK, R1.YR,R2.[Reports Sumbitted on Time], R1.[Total Submissions Schduled],LEFT(100* R2.[Reports Sumbitted on Time]/CAST(R1.[Total Submissions Schduled] AS DECIMAL),5) As TImely_Compliance_Weekly

FROM

(SELECT T1.WK, T1.YR, T1. Total_Submissions_No_Duplicates, T2.[Total Submissions Schduled],LEFT(100* T1.Total_Submissions_No_Duplicates/CAST(T2.[Total Submissions Schduled] AS DECIMAL),5) AS Compliance_Percentage_Weekly
FROM

(SELECT DATEPART(week,R1.DateActivity) AS WK, DATEPART(year,R1.DateActivity) as YR, count(*) AS Total_Submissions_No_Duplicates
FROM

(SELECT R1.DateActivity, R1.ContractNumber, case when count(*)>=1 then 1 else 0 end AS Total_Submissions
FROM [RDODS].[DDPM].[DDRMaster] R1
group by R1.DateActivity, R1.ContractNumber) R1

group by DATEPART(week,R1.DateActivity), DATEPART(year,R1.DateActivity)) T1

JOIN

(SELECT 
  DATEPART(YEAR, DATE) AS YR, 
  DATEPART(wk, DATE) AS WK, 
  1.0*count(*) AS [Total Submissions Schduled]
  FROM [RDODS].[DDPM].[DDRDailySummaryAndCompliance]
 WHERE  
	((ScheduledToBeOnSiteFlag is Not Null)
	and Date < GETDATE() 
	And CalendarDayType = 1 
	AND Date > '2019-06-11')
	OR (WorkersOnSiteCount is Not Null and Date < GETDATE() AND Date > '2019-06-11')
 GROUP BY DATEPART(wk, DATE), DATEPART(YEAR, DATE)) T2

on (T1.WK = T2.WK) and (T1.YR=T2.YR)) R1

JOIN

(SELECT 
  DATEPART(YEAR, DateActivity) AS YR, 
  DATEPART(wk, DateActivity) AS WK, 	   
  1.0* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end) AS [Reports Sumbitted on Time],
  1.0* count (*) AS [Total Reports Submitted],
  LEFT(100* sum (case when DATEDIFF(day, ReportDate, CONVERT(date, DateCreated)) <= 2 then 1 else 0 end)/NULLIF(CAST(1.0* count (*) AS DECIMAL),0),5) AS On_Time_Percent_Weekly
FROM [RDODS].[DDPM].[DDRMaster]
WHERE ContractNumber != '11111' 
  AND DateActivity > '2019-06-11'
  AND DateCreated >= DateActivity
  AND ContractNumber != 'CLARK'
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)) R2

on (R1.WK = R2.WK) AND (R1.YR = R2.YR)
order by R1.YR, R1.WK
```

## Validation

```sql

/*Query to determine how many reports Clark has approved (valiated)

structure:
-sum all instances where ClarkApproved values is not null as approved reports
-sum all records with a recorded date to find total submitted reports
-divide the above values to find the percentage of reports approved

*/

/*SELECT DATEPART(wk, DateActivity) AS WK, DATEPART(YEAR, DateActivity) AS YR, 
	   sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	   sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted, 
	   CASE when sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)>0 then (sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)/CAST(sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS DECIMAL)) Else Null end AS Weekly_Approved_Percent
FROM [RDODS].[DDPM].[DDRMaster]
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)
ORDER BY YR, WK   */



/*======================================================================================================================================================== */


/*Cumulative Validation Query
Structure:
-Join the code used above to find weekly accuracy % to itself on the following conditions
-the week/year of the value in the left table is greater or equal to that of the right table
-this join allows us to sum total reports to date for each week, and divide the number of reports
-that had no over-rides by the total number of reports. this is the cumulative validation  to date
*/
/* SELECT T1.WK, T1.YR, LEFT(100*Sum(T2.Approved_Reports)/nullif(CAST(Sum(T2.Total_Submitted) AS DECIMAL),0),5) AS Cumulative_Approved_Percent

FROM 

(

SELECT DATEPART(wk, DateActivity) AS WK, DATEPART(YEAR, DateActivity) AS YR, 
	   sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	   sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted, 
	   CASE when sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)>0 then (sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)/CAST(sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS DECIMAL)) Else Null end AS Weekly_Approved_Percent
FROM [RDODS].[DDPM].[DDRMaster]
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)

) T1

JOIN

(

SELECT DATEPART(wk, DateActivity) AS WK, DATEPART(YEAR, DateActivity) AS YR, 
	   sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	   sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted, 
	   CASE when sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)>0 then (sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)/CAST(sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS DECIMAL)) Else Null end AS Weekly_Approved_Percent
FROM [RDODS].[DDPM].[DDRMaster]
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)

) T2

ON ((T1.WK >= T2.WK) AND (T1.YR = T2.YR)) OR (T1.YR >T2.YR)
GROUP BY T1.WK, T1.Yr 
order by T1.WK, T1.Yr    */





/*========================================================================================================================================= */

/*Final query structure
-above two queries are joined on week and year
-this allows us to see weekly and cumulative stats side-by-side
-also allows the team to export a single query to Tableau for further visualization

*/

Select R1.WK,R1.Yr,R1.Weekly_Approved_Percent,R2.Cumulative_Approved_Percent FROM
/*Combining queries above. Now will have weekly and cumulative averages in the same table*/
(

SELECT DATEPART(wk, DateActivity) AS WK, DATEPART(YEAR, DateActivity) AS YR, 
	   sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	   sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted, 
	   100*CASE when sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)>0 then (sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)/CAST(sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS DECIMAL)) Else Null end AS Weekly_Approved_Percent
FROM [RDODS].[DDPM].[DDRMaster]
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)

) R1

JOIN

(

SELECT T1.WK, T1.YR, LEFT(100*Sum(T2.Approved_Reports)/nullif(CAST(Sum(T2.Total_Submitted) AS DECIMAL),0),5) AS Cumulative_Approved_Percent

FROM 

(

SELECT DATEPART(wk, DateActivity) AS WK, DATEPART(YEAR, DateActivity) AS YR, 
	   sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	   sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted, 
	   CASE when sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)>0 then (sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)/CAST(sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS DECIMAL)) Else Null end AS Weekly_Approved_Percent
FROM [RDODS].[DDPM].[DDRMaster]
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)

) T1

JOIN

(

SELECT DATEPART(wk, DateActivity) AS WK, DATEPART(YEAR, DateActivity) AS YR, 
	   sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	   sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted, 
	   CASE when sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)>0 then (sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end)/CAST(sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS DECIMAL)) Else Null end AS Weekly_Approved_Percent
FROM [RDODS].[DDPM].[DDRMaster]
GROUP BY DATEPART(wk, DateActivity), DATEPART(YEAR, DateActivity)

) T2

ON ((T1.WK >= T2.WK) AND (T1.YR = T2.YR)) OR (T1.YR >T2.YR)
GROUP BY T1.WK, T1.Yr 

) R2

on (R1.WK=R2.WK) and (R1.Yr = R2.YR)
WHERE R1.YR > 1901
order by R1.YR, R1.WK






/* ==============================================================================================================*/

/*
SELECT sum(case when ClarkApproved IS NOT Null AND  DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Approved_Reports,
	sum(case when DATEPART(YEAR, DateActivity) > 1901 then 1 else 0 end) AS Total_Submitted
FROM [RDODS].[DDPM].[DDRMaster]    */

```

# Conclusion
