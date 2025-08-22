---
layout: post
title: Fantasy Football Analytics
image: "/posts/leaguelogo.png"
tags: [Python, Power BI, Analytics]
---

# Part 1 - Introduction
The Fantasy League industry has been booming since the early 2000s, jumping from an estimated 15 million players in 2003 to over 65 million in recent years. ESPN's Fantasy Football League hosted over 13 million players in its program in 2024 alone according to its internal analytics. Last year I took part in a fantasy league myself, and it sparked me to make my own analytics based on available league data. The league itself was a standard PPR setup, with 12 total participants.

After some research, I discovered that ESPN has an API which I could use to extract data efficiently. Using Python, I was able to ETL data into Power BI and create visuals/analysis on our league's draft, match results, team performance, and playoff performance.

## ---- Results ----
A few insights I gleaned from the loaded data included:

Draft order had little correlation to in-season success. The 9-11 slots in our 12 person league all ended up making deep playoff runs, with the #11 slot winning it all in the end. Though the top picks did snag several high-performing players, there was also significant variability that stunted the potential of these teams - for example, consensus #1 pick C. McCaffrey getting injured at the start of the season and missing most of the year. Additionally, the team managers that went far in the playoffs tended to have more football knowledge and were more actively swapping their players week-to-week.

ESPN did a fairly good job at predicting matchup results. 75% of matches ended with their predicted win/loss result. Even with this impressive prediction rate, there was a lot of variability within the actual match differentials. ESPN's average margin of victory was different from actual results by an average of 24 points, with 50% of the data being between the first and third quartiles of 8 and 38 points. 

You can view the final BI dashboard below, or via this link: <a href="https://app.powerbi.com/view?r=eyJrIjoiZWQ5NDliZjgtNjdjOC00MzU2LThmMDYtNmMwZWYzNmNiNGFmIiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9">Dashboard Link</a>

<p align="center"><iframe title="Fantasy Football 2024 Report PBIR" width="900" height="560.25" align="center" src="https://app.powerbi.com/view?r=eyJrIjoiZWQ5NDliZjgtNjdjOC00MzU2LThmMDYtNmMwZWYzNmNiNGFmIiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9" frameborder="0" allowFullScreen="true"></iframe></p>

Note the code for this project can also be viewed in this Github repository: <a href="https://github.com/akstl1/jonesfantasyfootball">Project Repo Link</a>

## ---- Data Source and Notes ----
Data source: The data for this project was collected via ESPN's API, and I used the following wiki to access its documentation https://github.com/cwendt94/espn-api/wiki

The API provides access to league information, provided you have your league's ESPN, SW, and S2 IDs. Once you have access to these IDs, you have can access the following:
- Team Class: Provides information on a fantasy team including its name, manager, record, points for/against, standing, and playoff chances
- Player Class: Provides informaiton about a given player in a team's lineup such as their position, status, and points scored
- Box Player Class: Provides more specific information about a player at a given time
- Box Score Class: Provides information on the teams involved in a matchup, their lineups and projected scores
- Pick Class: Provides information about each pick in the draft
- Settings Class: Provides information about league-specific setings
- Matchup Class: Provides information about the final score and lineup for a given matchup
- Activity Class: provides details on trades and drops/adds during the season

For this project, I used each class except the Activity class for my analysis since no trades were initated by any team.

## ---- Import Libraries for Project ----
```python
from espn_api.football import League
import pandas as pd
import numpy as np
from dotenv import load_dotenv
import os
load_dotenv()
from espn_api_functions import curr_week, hundredyardgame, threehundredyardpassinggame, draft, powerRankingsSingleWeek, powerRankingsMultiWeek, standingsSingleWeek, standingsMultiWeek, matchupsSingleWeek, matchupsMultiWeek, playerStatsSingleWeek, playerStatsMultiWeek, fantasy_teams

league = League(league_id=os.getenv("LEAGUE_ID"), year=int(os.getenv("YEAR")), espn_s2=os.getenv("S2"),swid=os.getenv("SWID"))
curr_week=league.current_week
```

# Part 2 - Data Requirements & Ingestion

When outlining the report I wanted to build, I focused on 6 key concepts: power rankings, draft, scoreboard/standings, matchup details, individual team performance and final playoff bracket.

## ---- League Power Rankings ----
Power Rankings are an estimate of a team's strength and forecast going forward. To help understand how teams in the league are projected to perform in future weeks and how a team's stength has changed over time, I wanted to plot initial and current power rankings for each team.

## ----League Draft ----
The draft is a key component of any league. In addition to being a fun way to kick off the league year, typically during the draft a large proportion of high-value players are chosen by the participating teams. In our 12-person league, each team got 17 picks for a total of 204 player selections. To visualize how the draft progressed and get a sense of each player's strategy, I chose to visualize both each team's individual choices and player type breakdown as well as providing a matrix of the full draft board.

## ----Weekly Scoreboard & Standings ----
To get an overview of how each week is progressing, I wanted to display how each matchup turned out and how that impacted the overall league standings for the week.

## ----Matchup Details & Box Scores ----
Beyond a scoreboard overview, I wanted to provide analysis on each matchup so I could review key players and statistics and determine whether any lineup changes may be needed in the future. To do so, I created visuals to show each team's top performers, key scoring measurements, performance in each point category and a player detail bookmark to see a better breakdown of individual performance.

## ---- Individual Team Performance ----
Outside the context of an individual matchup, it's important to undrestand a team's season-to-date performance and get context on how a team is performing compared to ESPN expectations and other teams. This was also an opportunity to provide more narrative analysis on a team's match results to date.

## ---- Final League Playoff Bracket ----
One of the most anticipated parts of a season are the playoffs. Most leagues offer players a reward for having deep playoff runs, so I wanted to make sure to capture each playoff matchup and highlight the victor of the league for the year.

## ---- Ingestion and Transform of Data ----
To perform the ETL processes required to make the above visuals, I leveraged ESPN's APIs and Python to create a set of functions. These functions could be used to get stats regarding the league draft, individual week results, and results for multiple weeks in case a bulk add of data is required. With the set of 13 functions I created, I made it possible to ingest data for any number of league weeks within a few seconds as Excel files.

The code for these functions can be viewed in the espn_api_functions.py file within the above linked Github repo.

After generating files with the above functions, I loaded each table into Power BI and performed some final data manipulations using BI's M Language. These final transforms allowed me to create new columns, tables based off the original Python code, and join tables together to get a better breakdown of my desired attributes.

Once done transforming my data, I used Power BI's modelling features to link tables together like a relational database so that I could effectively build visuals and measures for the report.

# Part 3 - Potential Future Work

In the future, it would be interesting to conduct more analysis to build on the current model.

For example, since there were no trades during this league year there wasn't any analysis to be done there. If trades are initiated next year, it would be interesting to create a trade report and analyze approvals/denials. Additionally, I think it would be insightful to see whether there is any impact on a team's frequency of adding/dropping players on their overall performance.

# Part 4 - Conclusion

Playing in Fantasy Leagues has become a staple of many peoples' lives over the last 2 decades, and with the rise in leagues there is a wealth of data available to be analyzed from league hosts like ESPN. After joining a league last year, I wanted to visualize the performance of our teams and learned how to efficiently extract league data with Python via ESPN's API. Using custom-built funcitons, I developed a method to extract data and create a Power BI dashboard/analysis on our league's draft, match results, team performance, and playoff performance.
