---
layout: post
title: Analyducks
image: "/posts/2DRubberDuck2.png"
tags: [Power BI, Analytics, Linear Regression]
---

# Part 1 - Introduction

You may have seen data lakes, now welcome to my data pond - where information about my rubber ducks gets stored and visualized. Though an estimated 1.4 Billion rubber ducks are sold each year, and numerous duck events happen worldwide, I have not been able to find any concise analytics about rubber ducks.

I've been collecting rubber ducks since 2017, and my flock has really taken flight since then! I hope that you enjoy learning about them as much as I enjoy finding them during my travels. (And no, I don't own a Jeep.)

Each duck has had information recorded about its location/time of purchase, length/height/width/weight, and personality. 

The data is stored in my own local repository, and this project was meaningful to me because:
- I got to showcase data and pictures of my prized collection of ducks
- I was able to use,format, and provide supporting information for map visuals which I do not get to use often
- I figured out how to get rubber ducks as a background to my charts, which makes them far more interesting in my opinion

## ---- Results ----
A few insights I gleaned from the processed data included:

I got really serious about collecting in 2023, which makes sense as the latter half of 2022 started a big period of changes in my profressional and personal life.

Though generally as ducks get larger, they also weigh more there was a relatively low R-squared score to represent this relationship - 67. This is because the ducks are made out of a variety of different material types and thicknesses, and sport all sorts of different accessories that throw off a "straight-line" relationship.

My rubber ducks have been to more countries than I have due to some generous donations from friends and family!

And as a bonus insight - taking pictures of 200+ ducks all at once can be tedious and time-consuming, if you are replicating this I would highly recommend collecting this data as you go rather than batching it.

You can view the final BI dashboard below, or via this link: <a href="https://app.powerbi.com/view?r=eyJrIjoiNzM4MWJjZTYtOGQ5OC00MWM1LWE4M2EtNDVlNGFkZWY3MmY5IiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9">Dashboard Link</a>

The code for this project can also be viewed in this Github repository: <a href="https://github.com/akstl1/Analyducks">Project Repo Link</a>

<p align="center"><iframe title="Fantasy Football 2024 Report PBIR" width="80%" height="560.25" align="center" src="https://app.powerbi.com/view?r=eyJrIjoiNzM4MWJjZTYtOGQ5OC00MWM1LWE4M2EtNDVlNGFkZWY3MmY5IiwidCI6IjVjOTQ5NmQ0LTgzODgtNDk4Ni1iNjFjLTQ5NTIxODY2NThkYSIsImMiOjF9" frameborder="0" allowFullScreen="true"></iframe></p>

## ---- Data Source and Notes ----
Data source: The data for this project was my own measurements and data inputs, currently stored in an excel file.

The Excel tables provide all key information required for this report, including:
- Rubber Duck purchase details - purchaser/location/date
- Rubber Duck dimensions - length/width/height/weight
- Rubber Duck personality details = name/catch phrase
- Location details - list of purchase location metadata so everyone can collect

# Part 2 - Data Requirements & Ingestion

When outlining the report I wanted to build, I focused on the following key concepts:

## ---- Quantity & Weights ----
As an analyst, I was of course interested in seeing how my flock has grown in size and weight over time as well as whether there was a clear trend relating duck dimensions and weight.

## ---- Donors+Locations ----
It takes a village to raise a duck collection, and accordingly I wanted to spotlight where my flock has come from both in terms of geography and donors.

## ---- Flock Details ----
Each (well, most) of my ducks are unique and so I wanted to showcase their images and individual stories in a comprehensive manner.

## ---- Where To Collect ----
Being an avid collector myself, I wanted to make sure that other quacks could see some good locations to start or expand their own collections. And make a cheat sheet for myself in the future of course.

## ---- Quack Facts ----
Beyond my own collection, rubber ducks have become a notable part of modern culture. I wanted to highlight a few key facts about rubber ducks to pique everyone's interests.

## ---- Ingestion and Transform of Data ----
To perform the ETL processes required to make the above visuals, I leveraged Power Query/M language to perform all necessary actions to get to my target state.

Once done transforming my data, I used Power BI's modelling features to link tables together like a relational database so that I could effectively build visuals and measures in DAX for the report.

# Part 3 - Potential Future Work

In the future, it would be interesting to conduct more analysis on rubber duck dimensions, as well as donors over time.

For example, it would be fun to identify the quartiles and outliers of my ducks' sizes and length/width/height ratios. I would also like to analyze the % of ducks that come from donors over time, to see how much momentum my collection gets from others as knowledge of my collection spreads.
