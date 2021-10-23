---
layout: post
title: Air Quality Benefits of Emissions Testing: Data Cleaning
image: "/posts/Park2.jpg"
tags: [Python, Parkinson's, Classification]
---

# Air Quality Benefits of Emissions Testing Capstone: Data Cleaning

## Project Context

In 2017 The Pennsylvania legislature was deciding whether to continue emissions testing in the state, and which test(s) to use if emissions testing was to continue.


As part of a capstone project, I was part of a team tasked with using existing data to determine potential benefits of emissions testing to help inform public policy descisions. The main questions the team sought to answer were:

- How can the emissions inspection results/data inform us about actual vehicle emissions over time?
- How does a particular model year’s emissions change over time?
- How much emissions are we preventing from entering the environment by testing?
- What are the emissions impacts of disagreeing OBD and IM240 results?

To start, the team needed to clean datasets at our disposal so they could be used for analysis. Below the code to clean the data is developed and explained. The end result of this process was generation of vehicle emissions datasets for eyars 2010-2016, with final datasets retaining ~50-60% of the original dataset records.


## Data Context


Before starting analysis to answer the team's main questions, there was a desire to create a clean data set from the original Colorado data. The clean data set would include relevant data, and take out outliers or discrepancies found in the original data.

The original datasets the team used were from Colorado emissions testing results. Data from 2010-2016 was available to the team. Each testing year's data held 500,000 - 1,000,000 rows and 250+ rows, with each row representing a unique inspection. These rows included data about the vehicle such as make/model/year/engine data, as well as test and inspection results.

In Colorado, generally speaking, two main tests were run on vehicles:
1. An OBD test. This test checks for conditions that waste fuel and shorten engine life, such as a loose gas cap.
2. An IM240 test. This test puts a vehicle on a treadmill and simulates different driving conditions. A sensor put at the tailpipe during this test measures actual emissions of carbon monoxide (CO), hydrocarbons (HC), and nitrogen oxides (NOx) from the vehicle.

The above tests were conducted according to Colorado's testing regimen. For light duty vehicles, the following rules defined testing requirements:
- Vehicles within their first 7 model years are exempt from testing
- Vehicles Model Year 1982 and newer must be inspected every other year
- Vehicles Model Year 1981 and older and diesel Vehicles Model Year 2003 and older must be inspected every year

The process of cleaning the Colorado datasets is described below.

### Clustering Records

In many analyses, the group wanted to analyze a fleet of vehicles. This could be an analysis by car make, model or year. To do so, the team created a unique ID to define these fleets using vehicle VINS. Certain digits in a vehicle VIN identifies it's make, model, year, and engine type. A standard vehicle VIN has 17 digits. When looking at the original Colorado dataset, there were many vehicles with shorter VIN lengths. To ensure that vehicles were being grouped and analyzed correctly,all records with shorter VINs were not included in the cleaned data set.

In 2017 The Pennsylvania legislature was deciding whether to continue emissions testing in the state, and which test(s) to use if emissions testing was to continue.


As part of a capstone project, I was part of a team tasked with using existing data to determine potential benefits of emissions testing to help inform public policy descisions. The main questions the team sought to answer were:

- How can the emissions inspection results/data inform us about actual vehicle emissions over time?
- How does a particular model year’s emissions change over time?
- How much emissions are we preventing from entering the environment by testing?
- What are the emissions impacts of disagreeing OBD and IM240 results?

To start, the team needed to clean datasets at our disposal so they could be used for analysis. Below the code to clean the data is developed and explained. The end result of this process was generation of vehicle emissions datasets for eyars 2010-2016, with final datasets retaining ~50-60% of the original dataset records.


## Data Context


Before starting analysis to answer the team's main questions, there was a desire to create a clean data set from the original Colorado data. The clean data set would include relevant data, and take out outliers or discrepancies found in the original data.

The original datasets the team used were from Colorado emissions testing results. Data from 2010-2016 was available to the team. Each testing year's data held 500,000 - 1,000,000 rows and 250+ rows, with each row representing a unique inspection. These rows included data about the vehicle such as make/model/year/engine data, as well as test and inspection results.

In Colorado, generally speaking, two main tests were run on vehicles:
1. An OBD test. This test checks for conditions that waste fuel and shorten engine life, such as a loose gas cap.
2. An IM240 test. This test puts a vehicle on a treadmill and simulates different driving conditions. A sensor put at the tailpipe during this test measures actual emissions of carbon monoxide (CO), hydrocarbons (HC), and nitrogen oxides (NOx) from the vehicle.

The above tests were conducted according to Colorado's testing regimen. For light duty vehicles, the following rules defined testing requirements:
- Vehicles within their first 7 model years are exempt from testing
- Vehicles Model Year 1982 and newer must be inspected every other year
- Vehicles Model Year 1981 and older and diesel Vehicles Model Year 2003 and older must be inspected every year

The process of cleaning the Colorado datasets is described below.

### Clustering Records

In many analyses, the group wanted to analyze a fleet of vehicles. This could be an analysis by car make, model or year. To do so, the team created a unique ID to define these fleets using vehicle VINS. Certain digits in a vehicle VIN identifies it's make, model, year, and engine type. A standard vehicle VIN has 17 digits. When looking at the original Colorado dataset, there were many vehicles with shorter VIN lengths. To ensure that vehicles were being grouped and analyzed correctly,all records with shorter VINs were not included in the cleaned data set.

# Data Analysis

## Loading The Data

For the below code, I used data from 2012. This process can be replicated for any years' data.

First, I loaded in the dataset and find it's length. This gave me a measure of how many tests were conducted in 2012.

The original dataset had many columns irrelevant to emissions analysis; these were filtered out of our cleaned data set to decrease memory usage and increase data readibility. The relevant columns indicate the following information:

- V_VIN: Vehicle VIN
- V_DATE: Inspection Date
- V_MAKE: Vehicle Make
- V_MODEL: Vehicle Model
- V_VEH_YEAR: Vehicle Year
- V_ODOMETER: Vehicle odometer reading
- V_CO: Carbon monoxide emissions reading
- V_HC: Hydrocarbon emissions reading
- V_NOX: NOx emissions reading
- V_CO_STD: EPA CO maximum allowable emissions limit
- V_HC_STD: EPA HC maximum allowable emissions limit
- V_NOX_STD: EPA NOx maxium allowable emissions limit
- V_OBD_RES: OBD test result
- V_EM_RES: IM240 test result
- V_RESULT: Overall inspection result. If a vehicle fails its OBD or IM240 or other inspection aspects, the entire inspection is a fail
- V_TRANS: Vehicle transmission type
- V_CYLINDERS: Number of cylinders the car's engine has
- V_DISP: Engine size (Liters)
- V_DRIVE: Vehicle drive type, eg all wheel drive

```r
#loads the relevant dataset
VTR2012 <- read.csv("C:/Users/VTR2012.txt")

#finds the original number of vehicles in the data
original_count <- length(VTR2012$V_VIN)

#narrows the VTR2012 dataset by only selecting relevant columns for analysis
VTR2012 <- VTR2012[,c("V_VIN","V_DATE_TIME" ,"V_MAKE", "V_MODEL", "V_VEH_YEAR", "V_ODOMETER" , "V_CO", "V_HC", "V_NOX", "V_CO_STD", "V_HC_STD", "V_NOX_STD", "V_OBD_RES", "V_EM_RES", "V_RESULT" , "V_TRANS", "V_CYLINDERS", "V_DISP", "V_DRIVE" )]
```


