---
layout: post
title: Air Quality Benefits of Emissions Testing - Analysis
image: "/posts/vehicle_emissions.jpg"
tags: [Python, Data Analysis, Pandas, Seaborn, Visualization]
---

# Air Quality Benefits of Emissions Testing Capstone Part II: Data Analysis

## ----Project Context (Re-stated from Part I) ----

As part of a capstone project my team and I were tasked with using data to inform the Pennsylvania legistlature's decision on whether to continue vehicle emission testing and which tests to use. 

The data at our disposal consisted of emissions test results from Colorado over the span of 2010-2016.

With the provided data, the team sought to investigate the following questions as the overall project goal:

- How can the emissions inspection results/data inform us about actual vehicle emissions over time?
- How does a particular model year’s emissions change over time?
- What amount of emissions are we preventing from entering the environment by testing?

The above questions would inform our recommendations to the state legistlative bodies.

### Summary of Results

#### --- How can the emissions inspection results/data inform us about actual vehicle emissions over time? --

To answer the group's first question, I did the following:

First, I looked at vehicle pass rates over time, by model year. This analysis showed that for both OBD and IM240 tests, newer vehicles passed at higher rates than older vehicles. For model years where testing was mandatory every year, it appeared that passing rates slowly increased with each testing year.

Next, I looked at the percent of cars passing both emissions tests. For older model years, only ~70% of vehicles passed both tests while for newer model year vehicles the passing rate of both tests was close to 100%. This would indicate that the impact of using one test over another could be small in new vehicles but relatively larger in older vehicles. Having a large amount of vehicles pass one test but not another would be critical if a state was considering implementing only one emissions test rather than both the IM240 and OBD tests.

I also analyzed average pollution rates by model year for NOx, CO and HC. The resultant plots showed that newer vehicle emissions rates were more than 90% lower than those of older vehicles. The plots also showed that up to model year 2003-2004, emissions rates in vehicles steadily increased year over year. This could indicate that a vehicle's pollution control devices become less efficient over time. 

#### --- How does a particular model year’s emissions change over time?

To answer the group's second question, I looked at apparent pollution control degradation over time. The resultant analysis/figures showed that with each testing year, a given model year's average initial emissions rates were higher than the previous year's rates. This likely meant that pollution control devices were getting less efficient over time, and required routine maintenance to ensure vehicles were actually in accordance with EPA standards.

#### --- What amount of emissions are we preventing from entering the environment by testing?

Finally, I created a measure to estimate the pollutants saved each year by conducting IM240 emissions tests. This showed that each year hundreds to thousands of tonnes of pollutants could be saved due to this testing as shown below.

|Testing Year	|Theoretical HC Saved (tonnes)|	Theoretical CO Saved (tonnes)|	Theoretical NOX Saved (tonnes)|
|---|---|---|---|	
|2010|	360|	3970|	350|
|2011|	291|	3674|	278|
|2012|	609|	6816|	666|
|2013|	569|	6316|	663|
|2014|	538|	5799|	625|
|2015|	424|	4734|	451|
|2016|	399|	4465|	466|

#### --- Final Conclusions ---
The team compiled all the above findings, as well as research from other project team efforts, into a report for the Pennsylvania legislature recommending that emissions testing continue in the state and include both IM240 and OBD tests to ensure that vehicles were performing efficiently and polluting within EPA standards. Peak vehicle performance would lead to savings for drivers, and lower emissions would help keep air clean for residents and reduce the amount of pollutants in the air that contrinute to climate change / the greenhouse effect. 



## ----Emissions Standards Context----

Emissions standards in the United States began after the passage of the 1970 Clean Air Act (CAA). The CAA defined the US Environmental Protection Agency's (EPA) role in protecting air quality and the ozone layer. To protect air quality, the CAA defined initial vehicle emissions limits for a variety of pollutants including Carbon Monoxide (CO), hydrocarbons (HC), and Nitrogen Oxides (NOx).

The first set of emissions standards, now termed Tier 0 standards, took effect in 1975. In 1979 and 1981, Congress tightened these standards even further and in 1990 a formal set of new emissions limits was defined; these standards, implemented starting in 1994, were deemed Tier 1 standards.

Since that time, the emissions standards have been tighened several times. In 2000, Tier 2 standards were defined and were implemented starting in 2004. In 2014, Tier 3 standards were defined and are being phased in between 2017 and 2025.

Most vehicles on the road today are held to either Tier 3 or Tier 2 standards, though cars model year 2003 and older are held to Tier 1 standards.

According to the EPA, emissions standards have led to significant decreases in vehicle emissions including up to a 98% reduction in NOx emissions in light duty vehicles on average.

More information about emissions standards can be found <a href="https://www.epa.gov/greenvehicles/light-duty-vehicle-emissions#history">here</a>.

## ----Data Context----

Before starting analysis to answer the team's main questions, I was tasked with creating clean data sets from the original Colorado data. The clean data sets would include only relevant columns, and take out outliers or discrepancies found in the data.

The original datasets the team used were from Colorado emissions testing results. Data from 2010-2016 was available, and each testing year's data held 500,000 - 1,000,000 rows and 250+ columns. Each row included data from a single vehicle inspection including information about the vehicle such as make/model/year/engine data, as well as test and inspection results.

In Colorado, generally speaking, two emissions tests were run on vehicles:

1. An OBD test. This test accesses a car's diagnostic center, and checks the status of the emissions system and key engine components. This helps identify conditions that waste fuel and shorten engine life, such as a loose gas cap.

3. An IM240 test. This test puts a vehicle on a dynamometer treadmill and simulates different driving conditions. A sensor put at the tailpipe during this test measures actual emissions of carbon monoxide (CO), hydrocarbons (HC), and nitrogen oxides (NOx) from the vehicle. These emissions are compared against applicable EPA thresholds to determine the overall result.

The above tests were conducted according to Colorado's testing regimen. For light duty vehicles, the following rules defined testing requirements:

- Vehicles within their first 7 model years are exempt from testing
- Vehicles Model Year 1982 and newer must be inspected every other year
- Vehicles Model Year 1981 and older and diesel Vehicles Model Year 2003 and older must be inspected every year

### Data Dictionary

Below is a list of column names in this dataset, and a short description of their meanings:
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

### Data Privacy Note
Please note that, for privacy reasons, the datasets originally used are no longer published for use.

### Import Python Libraries

```python
#analysis and viz imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Import Datasets

```python
#load original datasets

Data_2010_IM240_clean = pd.read_csv("./Data_2010_IM240_clean.csv")
Data_2011_IM240_clean = pd.read_csv("./Data_2011_IM240_clean.csv")
Data_2012_IM240_clean = pd.read_csv("./Data_2012_IM240_clean.csv")
Data_2013_IM240_clean = pd.read_csv("./Data_2013_IM240_clean.csv")
Data_2014_IM240_clean = pd.read_csv("./Data_2014_IM240_clean.csv")
Data_2015_IM240_clean = pd.read_csv("./Data_2015_IM240_clean.csv")
Data_2016_IM240_clean = pd.read_csv("./Data_2016_IM240_clean.csv")

Data_2010_OBD_clean = pd.read_csv("./Data_2010_OBD_clean.csv")
Data_2011_OBD_clean = pd.read_csv("./Data_2011_OBD_clean.csv")
Data_2012_OBD_clean = pd.read_csv("./Data_2012_OBD_clean.csv")
Data_2013_OBD_clean = pd.read_csv("./Data_2013_OBD_clean.csv")
Data_2014_OBD_clean = pd.read_csv("./Data_2014_OBD_clean.csv")
Data_2015_OBD_clean = pd.read_csv("./Data_2015_OBD_clean.csv")
Data_2016_OBD_clean = pd.read_csv("./Data_2016_OBD_clean.csv")

Data_OBD_and_IM240_clean_2010 = pd.read_csv("./OBD_and_IM240_clean_2010_df.csv")
Data_OBD_and_IM240_clean_2011 = pd.read_csv("./OBD_and_IM240_clean_2011_df.csv")
Data_OBD_and_IM240_clean_2013 = pd.read_csv("./OBD_and_IM240_clean_2013_df.csv")
Data_OBD_and_IM240_clean_2014 = pd.read_csv("./OBD_and_IM240_clean_2014_df.csv")
Data_OBD_and_IM240_clean_2015 = pd.read_csv("./OBD_and_IM240_clean_2015_df.csv")
Data_OBD_and_IM240_clean_2016 = pd.read_csv("./OBD_and_IM240_clean_2016_df.csv")
```

## ---- Analysis Part I: Emission Changes Over Time----

The first part of my analysis focused on how vehicle emissions changed over time. I approached this in two ways.

First, I analyzed the passing rate of vehicles by model year for each testing year in the data set. I was looking to see whether there appeared to be an improvement in passing rate year over year by model year, and whether there was a trend in passing rate across different model years.

### ----Pass Rate By Model Year----

First, I analyzed the passing rates by model year.

I used the cleaned OBD data prepared in the data cleaning process, and combined each testing year's data into one dataframe.

```python
def pass_rate_by_model_year(df,year,test,metric):
    df = df[["V_VEH_YEAR", metric]].groupby(by="V_VEH_YEAR", axis=0).agg({metric: ["count","sum"]})
    df.columns=df.columns.droplevel(0)
    df.rename(columns={"count": "Model_year_"+test+"_tests_conducted", "sum":"Model_year_"+test+"_passed_tests"}, inplace=True)
    df["Model_Year_"+test+"_Pass_Rate"] = 100*df["Model_year_"+test+"_passed_tests"]/df["Model_year_"+test+"_tests_conducted"]
    df["testing_year"]=year
    return df

# load passing rate data from Part I

pass_rate_by_model_year_OBD_df_2010=pass_rate_by_model_year(Data_2010_OBD_clean,2010,"OBD","V_OBD_RES")
pass_rate_by_model_year_OBD_df_2011=pass_rate_by_model_year(Data_2011_OBD_clean,2011,"OBD","V_OBD_RES")
pass_rate_by_model_year_OBD_df_2012=pass_rate_by_model_year(Data_2012_OBD_clean,2012,"OBD","V_OBD_RES")
pass_rate_by_model_year_OBD_df_2013=pass_rate_by_model_year(Data_2013_OBD_clean,2013,"OBD","V_OBD_RES")
pass_rate_by_model_year_OBD_df_2014=pass_rate_by_model_year(Data_2014_OBD_clean,2014,"OBD","V_OBD_RES")
pass_rate_by_model_year_OBD_df_2015=pass_rate_by_model_year(Data_2015_OBD_clean,2015,"OBD","V_OBD_RES")
pass_rate_by_model_year_OBD_df_2016=pass_rate_by_model_year(Data_2016_OBD_clean,2016,"OBD","V_OBD_RES")

pass_rate_by_model_year_IM240_df_2010=pass_rate_by_model_year(Data_2010_IM240_clean,2010,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2011=pass_rate_by_model_year(Data_2011_IM240_clean,2011,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2012=pass_rate_by_model_year(Data_2012_IM240_clean,2012,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2013=pass_rate_by_model_year(Data_2013_IM240_clean,2013,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2014=pass_rate_by_model_year(Data_2014_IM240_clean,2014,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2015=pass_rate_by_model_year(Data_2015_IM240_clean,2015,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2016=pass_rate_by_model_year(Data_2016_IM240_clean,2016,"IM240","V_EM_RES")
```

```python
# create an array with all dfs so they can be combined into one for analysis
pass_rate_by_model_year_passed_obd_only_df = [pass_rate_by_model_year_OBD_df_2010,pass_rate_by_model_year_OBD_df_2011,pass_rate_by_model_year_OBD_df_2012,pass_rate_by_model_year_OBD_df_2013,pass_rate_by_model_year_OBD_df_2014,pass_rate_by_model_year_OBD_df_2015,pass_rate_by_model_year_OBD_df_2016]

# combine above dfs into one
pass_rate_by_model_year_passed_obd_only_df=pd.concat(pass_rate_by_model_year_passed_obd_only_df)

pass_rate_by_model_year_passed_IM240_only_df = [pass_rate_by_model_year_IM240_df_2010,pass_rate_by_model_year_IM240_df_2011,pass_rate_by_model_year_IM240_df_2012,pass_rate_by_model_year_IM240_df_2013,pass_rate_by_model_year_IM240_df_2014,pass_rate_by_model_year_IM240_df_2015,pass_rate_by_model_year_IM240_df_2016]
pass_rate_by_model_year_passed_IM240_only_df=pd.concat(pass_rate_by_model_year_passed_IM240_only_df)
```

After combining the dataframes, I created a line chart to visually inspect what percent of vehicles passed the OBD test.

```python
fig, ax = plt.subplots(2,1,figsize=(15, 15), facecolor="white")

sns.lineplot(data=pass_rate_by_model_year_passed_obd_only_df, x='V_VEH_YEAR', y='Model_Year_OBD_Pass_Rate', hue="testing_year", palette="coolwarm",ax=ax[0])
ax[0].set_xlabel("Model Year")
ax[0].set_ylabel("OBD Passing Rate (%)")
ax[0].set_title("OBD Test Passing Rate vs Model Year, by Testing Year")
# plt.savefig('V_VEH_YEAR_vs_Model_Year_OBD_Pass.png')

sns.lineplot(data=pass_rate_by_model_year_passed_IM240_only_df, x='V_VEH_YEAR', y='Model_Year_IM240_Pass_Rate', hue="testing_year", palette="coolwarm",ax=ax[1])
ax[1].set_xlabel("Model Year")
ax[1].set_ylabel("IM240 Passing Rate (%)")
ax[1].set_title("IM240 Test Passing Rate vs Model Year, by Testing Year")
```

<img src="/emissions_imgs/1.png">


