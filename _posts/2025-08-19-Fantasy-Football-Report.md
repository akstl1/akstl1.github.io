---
layout: post
title: Fantasy Football Analytics
image: "/posts/leaguelogo.png"
tags: [Python, Power BI, APIs, Analytics]
---

# Part 1 - Introduction
The Fantasy League industry has been booming since the early 2000s, jumping from an estimated 15 million players in 2003 to over 65 million in recent years. ESPN's Fantasy Football League hosted over 13 million players in its 2024 program alone according to its internal analytics. Last year I took part in an ESPN 12-person PPR fantasy football league, and it sparked me to develop some analytics based on available league data.

Initially, I set out to manually record our league's data for this project. After realizing that this would require a daunting amount of effort and conducting some research, I discovered that ESPN has an API which I could use to extract data much more efficiently. Using Python, I was able to ETL data into Power BI and create visuals/analysis on our league's draft, match results, team performance, and playoff bracket.

Overall, this project was meaningful to me because:
- I got to combine my technical interests of utilizing Python data techniques and Power BI visualization knowledge with my lifelong interest of sports
- I was able to refresh my ability to look up documentation for, and query, APIs effectively
- I finally got to test out Gregor Brunner's <a href="https://en.brunner.bi/measurekiller">Measure Killer application</a>, which helped me clean up my data model at the end of the project

## ---- Results ----
A few insights I gleaned from the processed data included:

Draft order had little correlation to in-season success. The 9-11 slots in our 12 person league all ended up making deep playoff runs, with the #11 slot winning it all in the end. Though the top picks did snag several high-performing players, there was also significant variability that stunted the potential of these teams - for example, consensus #1 pick Christian McCaffrey suffering an Achilles Sprainat the start of the season and a PCL sprain thereafter causing him to miss most of the year. Additionally, the team managers that went far in the playoffs tended to have more football knowledge and were more actively swapping their players week-to-week.

Secondly, ESPN did a fairly good job at predicting matchup outcomes. 75% of matches ended with their predicted win/loss result. Even with this impressive prediction rate, there was a lot of variability within the actual match point differentials. ESPN's average margin of victory was different from actual results by an average of 24 points, with 50% of the differentials being between 8 and 38 points. As an aside, the largest margin of victory was an astounding 102 points - where ESPN had predicted an 11 pt differential.



You can view the final BI dashboard below, or via this link: <a href="https://app.powerbi.com/view?r=eyJrIjoiZWQ5NDliZjgtNjdjOC00MzU2LThmMDYtNmMwZWYzNmNiNGFmIiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9">Dashboard Link</a>

The code for this project can also be viewed in this Github repository: <a href="https://github.com/akstl1/jonesfantasyfootball">Project Repo Link</a>

<p align="center"><iframe title="Fantasy Football 2024 Report PBIR" width="900" height="560.25" align="center" src="https://app.powerbi.com/view?r=eyJrIjoiZWQ5NDliZjgtNjdjOC00MzU2LThmMDYtNmMwZWYzNmNiNGFmIiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9" frameborder="0" allowFullScreen="true"></iframe></p>

## ---- Data Source and Notes ----
Data source: The data for this project was collected via ESPN's API, and I used the <a href="https://github.com/cwendt94/espn-api/wiki">wiki linked here</a> to access its documentation.

The API provides access to league information, provided you have your league's ESPN, SW, and S2 IDs. Once you have access to these IDs, you have can access the following:
- Team Class: Provides information on a fantasy team including its name, manager, record, points for/against, standing, and playoff chances
- Player Class: Provides informaiton about a given player in a team's lineup such as their position, status, and points scored
- Box Player Class: Provides more specific information about a player at a given time
- Box Score Class: Provides information on the teams involved in a matchup, their lineups and projected scores
- Pick Class: Provides information about each pick in the draft
- Settings Class: Provides information about league-specific setings
- Matchup Class: Provides information about the final score and lineup for a given matchup
- Activity Class: provides details on trades and drops/adds during the season

For this project, I used each class except the Activity class since no trades were initated by any team.

# Part 2 - Data Requirements & Ingestion

When outlining the report I wanted to build, I focused on 6 key concepts: power rankings, draft, scoreboard/standings, matchup details, individual team performance and final playoff bracket.

## ---- League Power Rankings ----
Power Rankings are an estimate of a team's strength and forecast going forward. To help understand how teams in the league are projected to perform in future weeks and how a team's stength has changed over time, I wanted to plot initial and current power rankings for each team.

## ----League Draft ----
The draft is a key component of any league. In addition to being a fun way to kick off the league year, typically during the draft a large proportion of high-value players are chosen by the participating teams. In our 12-person league, each team got 17 picks for a total of 204 player selections. To visualize how the draft progressed and get a sense of each player's strategy, I chose to visualize both each team's individual choices and player type breakdown and built a matrix of the full draft board.

## ----Weekly Scoreboard & Standings ----
To get an overview of how each week progressed, I wanted to display how each matchup turned out and how that impacted the overall league standings for the week.

## ----Matchup Details & Box Scores ----
Beyond a scoreboard overview, I wanted to provide analysis on each matchup so I could review key players and statistics and determine whether any lineup changes may be needed in the future. To do so, I created visuals to show each team's top performers, key scoring measurements, performance in each point category and a player detail bookmark to see a better breakdown of individual performance.

## ---- Individual Team Performance ----
Outside the context of an individual matchup, it's important to undrestand a team's season-to-date performance and get insight on how a team is performing compared to ESPN expectations and other teams. This was also an opportunity to provide more narrative analysis on a team's match results to date.

## ---- Final League Playoff Bracket ----
One of the most anticipated parts of the season are the playoffs. Most leagues offer players a reward for having deep playoff runs, so I wanted to make sure to capture each playoff matchup and highlight the league victor.

## ---- Ingestion and Transform of Data ----
To perform the ETL processes required to make the above visuals, I leveraged ESPN's APIs and Python to create functions could get stats regarding the league draft, individual week results, and multi-week results in case a bulk data export is required. With the set of 13 functions I created, I made it possible to ingest data for any number of league weeks within a few seconds as Excel files.

The code for these functions can be viewed in the espn_api_functions.py file within the above linked Github repo.

After generating data files with the above functions, I loaded each table into Power BI and performed final data manipulations using Power BI's M Language. These final transforms allowed me to create new columns, new tables based off the original Python code, and join tables together to get a better breakdown of my desired attributes.

Once done transforming my data, I used Power BI's modelling features to link tables together like a relational database so that I could effectively build visuals and measures in DAX for the report.

# Part 3 - Potential Future Work

In the future, it would be interesting to conduct more analysis to build on the current model.

For example, since there were no trades during this league year there wasn't any analysis to be done there. If trades are initiated next year, it would be interesting to create a trade report and analyze approvals/denials. Additionally, I think it would be insightful to see whether there is any impact on a team's frequency of adding/dropping players on their overall performance.
