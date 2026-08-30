---
layout: post
title: Briarport Livability Analysis
image: "/posts/livability.png"
tags: [Power BI, Analytics]
---

# Part 1 - Introduction

In January 2026 I took part in the Power BI Data Viz Championships, sponsored by Microsoft. The contest consisted of three rounds, with the sponsors providing starter data files and parameters for final submissions. Each round was independent, and finalists from each round would earn a chance to compete in a final championship round.

This report was created during the competition's first round, with the challenge being to compare livability in a fictional city, Briarport, against eleven peer cities over the past decade using five livability metrics: housing affordability, transit, emissions, green space, and overall livability.

More specifically, the analysis' core purpose was to assist Briarport's city planners, policy analysts, and community stakeholders as these groups looked at the city's strengths and weaknesses and prioritized future urban planning investments and initiatives.

The overall parameters of the competition were:
- A limit of 5 report pages
- Mandatory inclusion of at least one core visual
- Several accessibility standards including color-encoding and font size requirements

This project was meaningful to me because:
- It brought me back to my Civil Engineering roots at its core
- I was able to incorporate concepts I recently learned including field parameters and min/max highlighting
- I got a chance to showcase my ability to generate actionable insights directly on a report

## ---- Results ----
A few insights I gleaned from the processed data included:

Briarport's strongest performance in the last decade was in housing affordability, in which it ranked first consistently, and in emissions with the city seeing the largest emissions reduction of any of its peers in this period.

Despite the strengths above, Briarport also faced two troubling trends: transit accessibility and green space both dropped more than nearly every other peer city in the surveyed time period.

Due to the mixed trends above, the overall livability score in Briarport has remained nearly the same over the past ten years.

To improve residents' quality of life, the city should continue its momentum in affordability and emissions fields while focusing in on maximizing ROI and impact in the city's transit and green space master plans.

As a silver lining and caveat to the above, when conducting regional analysis I found that Briarport performed above average compared to other cities in its region in both transit and green space. As a result, additional information may be needed on whether the Briarport's regional geography plays a part in these two lower-scoring categories.

You can view the final BI dashboard below, or via this link: <a href="https://app.powerbi.com/view?r=eyJrIjoiZThhOGMxNzgtNTlkYS00NzQ4LWE2NjItZjk2NWNiNDdlMTllIiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9">Dashboard Link</a>

The code for this project can also be viewed in this Github repository: <a href="https://github.com/akstl1/DataVizContests/tree/main/2026-01%20BI">Project Repo Link</a>

<p align="center"><iframe title="Fantasy Football 2024 Report PBIR" width="80%" height="560.25" align="center" src="https://app.powerbi.com/view?r=eyJrIjoiZThhOGMxNzgtNTlkYS00NzQ4LWE2NjItZjk2NWNiNDdlMTllIiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9" frameborder="0" allowFullScreen="true"></iframe></p>

## ---- Data Source and Notes ----
Data source: The data for this project was provided by Microsoft, and encoded in Power BI directly.

The tables provided all key information required for this report, including:
- City location - country/state/regional information about each of the 12 cities
- Livability Scores - Annual scores for each city with the following metrics

# Part 2 - Report Components

When outlining the report I wanted to build, I focused on the following key concepts:

## ---- Briarport Trends ----
While comparison to other cities is important, I also wanted to provide analysis solely related to Briarport. 

To this end, I created a series of line charts and commentary that highlighted the ten year trend for each metric and noted their best and worst measurements. 

This would provide stakeholders a baseline for how their city was performing in recent years, and how the city's individual performance compared to pre-existing expectations resultant from prior investments and initiatives.

## ---- Peer comparison ----
To compare Briarport to its peers, I created bar charts comparing Briarport to its peers over a ten year window and used color to highlight how Briarport measured in the rankings.

I also used field parameters and dynamic text strings to allow users to toggle between the five relevant metrics and view contextualized insights.

And finally I used bookmarks that let users switch between analyzing Briarport's 2024 ranking, ten-year score trend, and year-by-year ranking change for the selected metric.

I believed that the combination of charts and options above let the user digest a manageable amount of information in meaningful ways that could actually influence decisions and spark discussion on how to improve city planning going forward.

## ---- Regional Analysis ----
As I learned in my civil engineering studies, environment can play a large role in a city's success. Thus in addition to comparing Briarport to other cities I wanted to see how its region compared to others over this time period.

To do so, I created a line chart and used small multiples to dissect how cities in each region performed in each metric. I overlaid Briarport's data in each region as well to understand how it compared to each city in this new context.

I also averaged each region's metric scores to better understand how the regions overall performed relative to each other.

This regional analysis allowed me to take away some biases that city-only measurements yielded and provided a more wholistic, environmentally informed overview of Briarport's strengths and weaknesses.

## ---- Ingestion and Transform of Data ----
To perform the ETL processes required to make the above visuals, I leveraged Power Query/M language to perform all necessary actions to get to my target state.

Once done transforming my data, I used Power BI's modelling features to link tables together like a relational database so that I could effectively build visuals and measures in DAX for the report.

# Part 3 - Potential Future Work

In the future, I would love to have data on prior livability investments/budgets, milestones and targets. Seeing how a city has evolved is one thing, but measuring ROI and outcomes based on previous decisions would provide much more insight into Briarport's performance.

For example, we could have a "Master Plan" score rating the city's planning efforts/momentum on improving livability or an "ROI" score rating a city's ability to fulfill its promises and do so within the stated timeline and budget. As I've experienced both in construction and finance, focusing on the planning process itself and making improvements there can yield significant dividends to everyone.
