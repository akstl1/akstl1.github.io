---
layout: post
title: Air Quality Benefits of Emissions Testing Part II - Analysis
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

<img src="/img/posts/emissions_imgs/1.png">

After initial inspection of the above plots, the team saw several general trends:

Firstly, vehicles with older model years generally had lower passing rates than vehicles with newer model years. This made sense for several reasons. Firstly, since the OBD test checks for engine inefficiencies and breakdowns, it was plausible to see that older vehicles with older equipment had more "broken" components. Secondly, since older vehicles were created with different pollution requirements they are less likely to meet the most current EPA standards.

Secondly, for OBD results the team noticed that between testing years 2010-2016, the passing rate trends decreased year over year for all model years up to model year 2005. The team believes this trend was the result of engine components breaking down over time; as more vehicles from a particular model year were eligible for testing as time went on, more were brought in and had failing results.

For model years newer than 2005, the 2015 and 2016 passing rates was higher than the other testing year rates. The team was not certain why this occurred, but speculated that it was a combination of the following:
- Since these cars were newer, their engine components may not have been as damaged as older vehicles
- Newer vehicles didn't require testing in Colorado until they were 7 years old. As a result, OBD tests could have been performed "just to check the vehicle" and failing results may not have been recorded in the database

Finally, for IM240 results model years and model years up to 1994 testing results appeared to improve year over year. Thereafter for model years after 1994 testing results began to decline year over year. This might be a result of the various EPA emissions standards. Tier 1 standards were put in effect in 1994, so vehicles older than that would be more likely to fail current emissions tests. Upon a test failure, owners would have to improve their vehicles and when re-tested the next year the overall fleet would have a higher passing average. For newer vehicles it is possible that, as for OBD trends, newer vehicles were tested despite not being required to within their first 7 years. If these vehicles failed, they would not necessarily need to be fixed until mandated. Though there could be other explanations, this was the team's best intuition on this pattern.

### ----Percent of Cars Passing Both / One Test----

For the second part of this analysis, I looked at what percent of cars passed both tests vs exactly one test. If many cars are passing one test but failing the other, this could lead to cars on the road that:
1. Have inefficient engine components but pass the emissions test (pass IM240 test, but wouldn't pass an OBD test)
2. Have working engine components but are emitting more than EPA standards allow (pass OBD test, but wouldn't pass IM240)

If many cars meet one of the above scenarios, that would mean that if the PA legislature chose to implement one test rather than both there could be cars on the road with inefficient engine components or high emissions.

First I will load in the data for this portion of analysis.

```python
#load data

pass_rate_by_model_year_both_pass_df_2010=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2010,2010,"both_tests")
pass_rate_by_model_year_both_pass_df_2011=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2011,2011,"both_tests")
pass_rate_by_model_year_both_pass_df_2013=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2013,2013,"both_tests")
pass_rate_by_model_year_both_pass_df_2014=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2014,2014,"both_tests")
pass_rate_by_model_year_both_pass_df_2015=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2015,2015,"both_tests")
pass_rate_by_model_year_both_pass_df_2016=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2016,2016,"both_tests")
```

I will then combine the above datasets so they can be plotted more easily. I then generate a linechart to show this data.

```python
pass_rate_by_model_year_passed_both_tests_df = [pass_rate_by_model_year_both_pass_df_2010,pass_rate_by_model_year_both_pass_df_2011,pass_rate_by_model_year_both_pass_df_2013,pass_rate_by_model_year_both_pass_df_2014,pass_rate_by_model_year_both_pass_df_2015,pass_rate_by_model_year_both_pass_df_2016]
pass_rate_by_model_year_passed_both_tests_df=pd.concat(pass_rate_by_model_year_passed_both_tests_df)
```

```python
fig, ax = plt.subplots(figsize=(15, 5), facecolor="white")
sns.lineplot(data=pass_rate_by_model_year_passed_both_tests_df, x='V_VEH_YEAR', y='Model_Year_both_tests_Pass_Rate', hue="testing_year", palette="coolwarm")
plt.xlabel("Model Year")
plt.ylabel("Percent of Cars Passing Both Tests (%)")
plt.title("Percent Cars Passing Both Tests vs Model Year, by Testing Year")
plt.savefig('V_VEH_YEAR_vs_Model_Year_both_Pass.png')
```

<img src="/img/posts/emissions_imgs/2.png">

In the above chart, I saw a few trends.

Firstly, passing rate of both tests generally increased in newer testing years. This might indicate that more cars are getting proper maintenance as time goes on so that their engine and emissions performance improves.

Secondly, newer model years had significantly higher rates of passing both tests than older model years. This likely reflects newer vehicles being designed to meet new regulations and standards.

To complete this portion of analysis, I plotted out the percent of vehicles passing exactly one of the required inspection tests. 

```python
def pass_rate_by_model_year_exactly_one_test(df,year,test):
    df["one_test_passed"]= (df["V_OBD_RES"]+df["V_EM_RES"]==1).astype(int)
    df = df[["V_VEH_YEAR", "one_test_passed"]].groupby(by="V_VEH_YEAR", axis=0).agg({"one_test_passed": ["count","sum"]})
    df.columns=df.columns.droplevel(0)
    df.rename(columns={"count": "Model_year_"+test+"_tests_conducted", "sum":"Model_year_"+test+"_passed_tests"}, inplace=True)
    df["Model_Year_"+test+"_Pass_Rate"] = 100*df["Model_year_"+test+"_passed_tests"]/df["Model_year_"+test+"_tests_conducted"]
    df["testing_year"]=year
    return df
```

```python
both_clean_data_frames_exactly_one_test_passed = [pass_rate_by_model_year_exactly_one_test,pass_rate_by_model_year_one_pass_df_2011,pass_rate_by_model_year_one_pass_df_2013,pass_rate_by_model_year_one_pass_df_2014,pass_rate_by_model_year_one_pass_df_2015,pass_rate_by_model_year_one_pass_df_2016]
both_clean_data_frames_exactly_one_test_passed=pd.concat(both_clean_data_frames_one_test_passed)

fig, ax = plt.subplots(figsize=(15, 5), facecolor="white")
sns.lineplot(data=both_clean_data_frames_one_test_passed, x='V_VEH_YEAR', y='Model_Year_both_tests_Pass_Rate', hue="testing_year", palette="coolwarm")
plt.xlabel("Model Year")
plt.ylabel("Percent of Cars Passing Exactly One Test (%)")
plt.title("Percent Cars Passing Exactly One Test vs Model Year, by Testing Year")
plt.savefig('V_VEH_YEAR_vs_Model_Year_both_Pass.png')
```

<img src="/img/posts/emissions_imgs/3.png">

As seen above, the rate of cars passing only one test decreased drastically in newer model years. In the newer model years this rate was close to 0% while in the oldest model year in this data the rate was close to 30%. Having lower rates in this instance is good; if most vehicles are either passing both tests or failing both tests, there may not be as big of an impact of choosing to implement one test over another in new public policy decisions.

### ----Model Year Emission Analysis----

In theory, due to incresingly stringent emissions standards by the EPA, newer vehicles should have fewer emissions than older vehicles. The team wanted to confirm this and see if any other emissions trends can be spotted for a given model year.

To start this analysis, I load the model year emission datasets from Part I of this project.

```python
# load data

model_year_pollution_2010_df=pd.read_csv("./model_year_pollution_2010_df.csv")
model_year_pollution_2011_df=pd.read_csv("./model_year_pollution_2011_df.csv")
model_year_pollution_2013_df=pd.read_csv("./model_year_pollution_2012_df.csv")
model_year_pollution_2014_df=pd.read_csv("./model_year_pollution_2014_df.csv")
model_year_pollution_2015_df=pd.read_csv("./model_year_pollution_2015_df.csv")
model_year_pollution_2016_df=pd.read_csv("./model_year_pollution_2016_df.csv")
```

Next, I combined the data into one dataset for easier plotting.

```python
model_year_pollution_dfs = [model_year_pollution_2010_df,model_year_pollution_2011_df,model_year_pollution_2013_df,model_year_pollution_2014_df,model_year_pollution_2015_df,model_year_pollution_2016_df]
model_year_pollution_dfs_merged=pd.concat(model_year_pollution_dfs)
```

Finally,

I plotted model year against average emissions rate for each testing year. This would give the team a sense of any general emissions trends.

```python
fig, ax = plt.subplots(3,1,figsize=(15, 30), facecolor="white")
sns.lineplot(data=model_year_pollution_dfs_merged, x='V_VEH_YEAR', y='V_CO_mean', hue="testing_year", palette="coolwarm",ax=ax[0])
ax[0].set_xlabel("Model Year")
ax[0].set_ylabel("Average CO emissions (g/mile)")
ax[0].set_title("Average Vehicle Carbon Monoxide Emissions (g/mile) vs Model Year, by Testing Year")
# plt.savefig('V_VEH_YEAR_vs_CO_mean.png')

sns.lineplot(data=model_year_pollution_dfs_merged, x='V_VEH_YEAR', y='V_HC_mean', hue="testing_year", palette="coolwarm",ax=ax[1])
ax[1].set_xlabel("Model Year")
ax[1].set_ylabel("Average HC emissions (g/mile)")
ax[1].set_title("Average Vehicle Hydrocarbon Emissions (g/mile) vs Model Year, by Testing Year")
# ax[1].savefig('V_VEH_YEAR_vs_HC_mean.png')

sns.lineplot(data=model_year_pollution_dfs_merged, x='V_VEH_YEAR', y='V_NOX_mean', hue="testing_year", palette="coolwarm",ax=ax[2])
ax[2].set_xlabel("Model Year")
ax[2].set_ylabel("Average NOX emissions (g/mile)")
ax[2].set_title("Average Vehicle Nitrogen Oxide Emissions (g/mile) vs Model Year, by Testing Year")
plt.savefig('Model_Year_vs_avg_Pollution.png')
```

<img src="/img/posts/emissions_imgs/4.png">

Two general trends appeared evident from the above three plots.

Firstly, emissions rates in newer model years dropped significantly compared to older model years. Newer emissions rates were more than 90% lower than the oldest averages in this dataset. This appears to confirm the EPA's claims that emissions testing and more stringent standards have led to decreases in overall vehicle emissions.

Second, up to model year 2003-2004 it appears that emissions rates in vehicles steadily increased year over year. this could indicate that as a vehicle ages it's pollution control devices become less efficient. This possibility is explored more below. This trends appears to break after model year 2003 or 2004. This may be because newer model years do not require emissions testing for 7 years, so the data could be incomplete or represent a different subset of data than for model years where testing is compulsory.

## Analysis Part II: Pollution Analysis

In addition to understanding trends with regard to emissions test passing rates, the team wanted to better understand the tangible benefits of emissions testing. To do this, we focused on looking at the potential impact of testing on overall vehicle pollution.

I approached this in two ways. First, I created a visual comparison between the average emissions of vehicles on their first and last emissions tests in a given testing year. I wanted to see if there was any difference between values before and after potential intervention to fix vehicles. Secondly, I created an average pollution per year metric to estimate/quantify the amount of pollution avoided due to testing.

Seeing benefits in the two above areas could lead to a more compelling case to continue emissions testing in PA, so that vehicle performance and air quality would improve statewide.

### ----Pollution Control Degradation----

The first pollution-related analysis the team conducted was to compare emissions results after a vehicle's first and last IM240 test in a given year.

If a vehicle failed it's IM240 test, in theory it would need to get repaired and re-take the test until it's emissions values reached acceptable levels. As such, the team wanted to compare emissions pre-repair and post-repair to see whether there was a noticable difference.

First I loaded in the data from Part I and listed the average initial and final pollutant average values in a figure.

```python
pollution_degradation_df=pd.read_csv("./Pollution_Degradation_V2.csv")

#display first 5 rows to show data structure
pollution_degradation_df[pollution_degradation_df["Model Year"]==1995].sort_values(by="testing_year").head(7)
```

|row|Model Year|	First HC Result (g)|	Last HC Result (g)|	HC Result Difference (g)|	First NOx Result (g)|	Last NOx Result (g)|	NOx Result Difference (g)|	First CO Result (g)|	Last CO Result (g)|	CO Result Difference (g)|	testing_year|
|---|---|---|---|---|---|---|---|---|---|---|---|
|90|	1995|	0.831245|	0.683727|	0.147518|	1.479708|	1.338230|	0.141478|	8.007292|	6.861715|	1.145577|	2010|
|75|	1995|	0.839182|	0.739528|	0.099654|	1.505301|	1.416771|	0.088530|	8.359716|	7.466390|	0.893326|	2011|
|60|	1995|	0.921640|	0.647053|	0.274587|	1.666744|	1.381827|	0.284918|	9.398543|	7.097648|	2.300895|	2012|
|45|	1995|	0.900587|	0.610880|	0.289707|	1.650960|	1.356891|	0.294069|	9.130560|	6.800476|	2.330084|	2013|
|30|	1995|	0.993556|	0.657401|	0.336155|	1.717200|	1.388370|	0.328830|	9.823488|	7.238162|	2.585326|	2014|
|15|	1995|	0.917959|	0.638397|	0.279562|	1.662034|	1.386467|	0.275567|	9.437303|	7.335974|	2.101329|	2015|
|0| 	1995|	1.028063|	0.697158|	0.330905|	1.823751|	1.500109|	0.323642|	10.416887|	7.858637|	2.558250|	2016|

As seen above for model year 1995, as time went on the first test emissions averages for each pollutant type increased by 20-100%. Similar trends were seen for other model years as well.

To explore the data visually, I then plotted the average emissions values for each model year.

For each pollutant I created two plots. One showing the average emissions after the vehicle's first IM240 test (left below), and one showing the average emissions after the vehicle's last IM240 test (right).

```python
fig, ax = plt.subplots(3,2,figsize=(15, 25), facecolor="white")
fig.suptitle('Comparison of First and Last Test Emissions (g/mile)')

sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_HC_first', hue="testing_year", palette="coolwarm",ax=ax[0][0])
sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_HC_last', hue="testing_year", palette="coolwarm",ax=ax[0][1])
ax[0][0].set_ylim(0,1)
ax[0][1].set_ylim(0,1)
ax[0][0].set_xlabel("Model Year")
ax[0][0].set_ylabel("Average First Test NOx emissions (g/mile)")
ax[0][1].set_xlabel("Model Year")
ax[0][1].set_ylabel("Average Last Test NOx emissions (g/mile)")

sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_CO_first', hue="testing_year", palette="coolwarm",ax=ax[1][0])
sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_CO_last', hue="testing_year", palette="coolwarm",ax=ax[1][1])
ax[1][0].set_ylim(0,12)
ax[1][1].set_ylim(0,12)
ax[1][0].set_xlabel("Model Year")
ax[1][0].set_ylabel("Average First Test CO emissions (g/mile)")
ax[1][1].set_xlabel("Model Year")
ax[1][1].set_ylabel("Average Last Test CO emissions (g/mile)")

sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_NOX_first', hue="testing_year", palette="coolwarm",ax=ax[2][0])
sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_NOX_last', hue="testing_year", palette="coolwarm",ax=ax[2][1])
ax[2][0].set_ylim(0,2)
ax[2][1].set_ylim(0,2)
ax[2][0].set_xlabel("Model Year")
ax[2][0].set_ylabel("Average First Test HC emissions (g/mile)")
ax[2][1].set_xlabel("Model Year")
ax[2][1].set_ylabel("Average Last Test HC emissions (g/mile)")

plt.savefig('First_and_Last_Test_Comparisons.png')
```
<img src="/img/posts/emissions_imgs/5.png">

As seen in the above plots, it appears that generally the average pollution amounts after an initial test were higher than those after a final test for a model year fleet of vehicles. This would appear to show that emissions testing led to lower vehicle pollution overall.

To show this data differently, I plotted the difference between post-first test and post-last test average emissions for each model year and testing year below.

```python
fig, ax = plt.subplots(3,1,figsize=(15, 30), facecolor="white")
fig.suptitle('Comparison of First and Last Test Emissions Mean Values (g/mile)')

sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_HC_diff', hue="testing_year", palette="coolwarm",ax=ax[0])
ax[0].set_xlabel("Model Year")
ax[0].set_ylabel("HC difference between first and last test means (g/mile)")

sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_CO_diff', hue="testing_year", palette="coolwarm",ax=ax[1])
ax[1].set_xlabel("Model Year")
ax[1].set_ylabel("CO difference between first and last test means (g/mile)")

sns.lineplot(data=pollution_degradation_df, x='V_VEH_YEAR', y='V_NOX_diff', hue="testing_year", palette="coolwarm",ax=ax[2])
ax[2].set_xlabel("Model Year")
ax[2].set_ylabel("NOx difference between first and last test means (g/mile)")

plt.savefig('Difference_Bewteen_first_and_last_test_means.png')
```

<img src="/img/posts/emissions_imgs/5.png">

In the above, two general trends emerged.

Firstly, it appeared that in newer testing years the difference between the first and last test means was significantly smaller than that of earlier years. One possible cause of this was that as time went on, more vehicles received repairs to their pollution control devices and thus there was less correction to be made in later testing years.

Secondly, it appeared that later model years had a smaller difference between first and last test means than earlier model years. This was likely because newer vehicles were designed with stricter EPA standards in mind and thus required less correction than older model year vehicles that didn't have such upper emissions limits to adhere to.

### ----Pollutants Saved----

The final pollution analysis I performed aimed to estimate the amount of pollutants saved as a result of emissions test implementation.

In the data preparation portion of the project, I created a function to do the following:
- Take vehicles that failed their initial IM240 test
- Approximate an average miles per year column by dividing the total years the car could be on the road by it's odometer reading
- Approximate an average emissions saved rate for the vehicle by subtracting the initial and final emissions rates for each of the relevant pollutant types
- Multiply the averagen pollutant saved rate column values by the average miles per year approximation
- Sum the resultant potential pollutant saved column by testing year

By doing the above steps, I would now have approximate pollutants saved values for each type of pollutant and each testing year. This is shown in the table below, after loading in the data and combining it into a table.

```python
theoretical_pollutants_saved_df_2010 = pd.read_csv("./theoretical_pollutants_saved_df_2010.csv")
theoretical_pollutants_saved_df_2011 = pd.read_csv("./theoretical_pollutants_saved_df_2011.csv")
theoretical_pollutants_saved_df_2012 = pd.read_csv("./theoretical_pollutants_saved_df_2012.csv")
theoretical_pollutants_saved_df_2013 = pd.read_csv("./theoretical_pollutants_saved_df_2013.csv")
theoretical_pollutants_saved_df_2014 = pd.read_csv("./theoretical_pollutants_saved_df_2014.csv")
theoretical_pollutants_saved_df_2015 = pd.read_csv("./theoretical_pollutants_saved_df_2015.csv")
theoretical_pollutants_saved_df_2016 = pd.read_csv("./theoretical_pollutants_saved_df_2016.csv")

theoretical_pollutants_dfs=[theoretical_pollutants_saved_df_2010,theoretical_pollutants_saved_df_2011,theoretical_pollutants_saved_df_2012,theoretical_pollutants_saved_df_2013,theoretical_pollutants_saved_df_2014,theoretical_pollutants_saved_df_2015,theoretical_pollutants_saved_df_2016]
theoretical_pollutants_dfs_merged=pd.concat(theoretical_pollutants_dfs)
theoretical_pollutants_dfs
```

|Testing Year	|Theoretical HC Saved (tonnes)|	Theoretical CO Saved (tonnes)|	Theoretical NOX Saved (tonnes)|
|---|---|---|---|	
|2010|	360|	3970|	350|
|2011|	291|	3674|	278|
|2012|	609|	6816|	666|
|2013|	569|	6316|	663|
|2014|	538|	5799|	625|
|2015|	424|	4734|	451|
|2016|	399|	4465|	466|

As seen in the above, it appears that emissions testing could be saving Colorado alone hundreds to thousands of tonnes in pollutants each year. This could lead to cleaner air for residents and fewer fossil fuels contributing to climate change.

The above emissions savings, backed up by trends showing decreasing emissions after testing and lower emissions in newer vehicles, were all crucial to report to our legislative body so that similar benefits could be seen in PA.

## Future Work

In the future, I would like to analyze the impact of OBD and IM240 results that disagree, eg passing one test while failing the other. This would help further quantify the potential impact of utilizing only one of these tests in a testing regimen. Using only an OBD test could lead to vehicles with unacceptable emissions values being on the road because a meter reads that no small defects like gas caps are broken. Using only an IM240 test could lead to vehicles on the road with defects like leaking gas caps, which decrease fuel efficiency and overall vehicle performance.

I would also like to add more statistical methods to the above analysis. For example, I could run a paired t-test to determine whether the difference in emissions means between first and last tests is significant. This would help quantify/verify the trends I appeared to see in my initial analysis.

Thirdly, I would like to go more in-depth into a mini-VIN. It would be interesting to see if any particular vehicle types stand out as emitting far less or far more than other vehicles. Analyses like these could help pinpoint areas of improvement for car manufacturers and regulators.

Lastly, I would like to create a tool to show the benefits of emissions testing to the public. I would envision this as a dashboard. A user would be able to type in a vehicle type or model year and see the anticipated emissions savings that testing provides, accompanied by graphics showing that vehicle type's emissions trends over time as more testing and stricter standards have been implemented. This would be a great way to get people interested and involved in emissions reduction methods and legislation.

## Conclusion

In the analysis component of this project, I looked at several different aspects of vehicle emissions trends.

First, I looked at vehicle pass rates over time, by model year. This analysis showed that for both OBD and IM240 tests, newer vehicles passed at higher rates than older vehicles. For model years where testing was mandatory every year, it appeared that passing rates slowly increased with each testing year.

Next, I looked at the percent of cars passing both emissions tests. For older model years, only ~70% of vehicles passed both tests while for newer model year vehicles the passing rate of both tests was close to 100%. This would indicate that the impact of using one test over another could be small in new vehicles but relatively larger in older vehicles. Having a large amount of vehicles pass one test but not another would be critical if a state was considering implementing only one emissions test rather than both the IM240 and OBD tests.

To wrap up the first part of my analysis, I also analyzed average pollution rates by model year for NOx, CO and HC. The resultant plots showed that newer vehicle emissions rates were more than 90% lower than those of older vehicles. 

The plots also showed that up to model year 2003-2004, emissions rates in vehicles steadily increased year over year. This could indicate that a vehicle's pollution control devices become less efficient over time. 

To analyze emissions implications in more detail, I first looked at apparent pollution control degradation over time. The resultant analysis/figures showed that with each testing year, a given model year's average initial emissions rates were higher than the previous year's rates. This likely meant that pollution control devices were getting less efficient over time, and required routine maintenance to ensure vehicles were actually in accordance with EPA standards.

Finally, I created a measure to estimate the pollutants saved each year by conducting IM240 emissions tests. This showed that each year hundreds to thousands of tonnes of pollutants could be saved due to this testing.

The team compiled all the above findings, as well as research from other project team efforts, into a report for the Pennsylvania legislature recommending that emissions testing continue in the state and include both IM240 and OBD tests to ensure that vehicles were performing efficiently and polluting within EPA standards. Peak vehicle performance would lead to savings for drivers, and lower emissions would help keep air clean for residents and reduce the amount of pollutants in the air that contrinute to climate change / the greenhouse effect. 

