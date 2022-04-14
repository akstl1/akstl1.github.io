---
layout: post
title: Air Quality Benefits of Emissions Testing
image: "/posts/vehicle_emissions.jpg"
tags: [Python, Data Cleaning]
---


# Air Quality Benefits of Emissions Testing Capstone Part I: Data Cleaning

## ----Project Context----

As part of a capstone project my team and I were tasked with using data to inform the Pennsylvania legistlature's decision on whether to continue vehicle emission testing and which tests to use. 

The data at our disposal consisted of emissions test results from Colorado over the span of 2010-2016.

With the provided data, the team sought to investigate the following questions as the overall project goal:

- How can the emissions inspection results/data inform us about actual vehicle emissions over time?
- How does a particular model year’s emissions change over time?
- What amount of emissions are we preventing from entering the environment by testing?

The above questions would inform our recommendations to the state legistlative bodies.

### Summary Of Results

After cleaning the data, as explained below, The end result of this process was generation of vehicle emissions datasets for years 2010-2016, with final datasets retaining ~30-60% of the original dataset records.

|Test Year	|Original count	|17-digit VIN count	|Non-zero emissions count	|First tests count	|Clean OBD count	|Clean IM240 count	|Both cleaned count	|
|---|---|---|---|---|---|---|---|
|2016	|1,140,730	|1,130,164	|729,680	|535,631	|403,683	|535,412	|403,571	|
|2015	|1,149,083	|1,135,860	|712,018	|521,870	|385,160	|521,613	|384,997	|
|2014	|1,264,855	|1,248,393	|1,104,735	|845,512	|684,367	|845,349	|684,272	|
|2013	|1,288,070	|1,270,072	|1,127,618	|856,142	|676,829	|856,008	|676,742	|
|2012	|1,212,144	|1,192,982	|1,052,938	|806,980	|622,752	|806,835	|622,677	|
|2011	|451,792	|443,254	|379,461	|337,732	|243,230	|337,675	|243,209|	
|2010	|514,305	|504,838	|426,018	|362,410	|251,915	|362,308	|251,860	|

## ----Emissions Standards Context----

Emissions standards in the United States began after the passage of the 1970 Clean Air Act (CAA). The CAA defined the US Environmental Protection Agency's (EPA) role in protecting air quality and the ozone layer. To protect air quality, the CAA defined initial vehicle emissions limits for a variety of pollutants including Carbon Monoxide (CO), hydrocarbons (HC), and Nitrogen Oxides (NOx).

The first set of emissions standards, now termed Tier 0 standards, took effect in 1975. In 1979 and 1981, Congress tightened these standards even further and in 1990 a formal set of new emissions limits was defined; these standards, implemented starting in 1994, were deemed Tier 1 standards.

Since that time, the emissions standards have been tighened several more times. In 2000, Tier 2 standards were defined and were implemented starting in 2004. In 2014, Tier 3 standards were defined and are being phased in between 2017 and 2025.

Most vehicles on the road today are held to either Tier 3 or Tier 2 standards, though cars model year 2003 and older are held to Tier 1 standards.

According to the EPA, emissions standards have led to significant decreases in vehicle emissions including up to a 98% reduction in NOx emissions in light duty vehicles on average.

More information about emissions standards can be found <a href="https://www.epa.gov/greenvehicles/light-duty-vehicle-emissions#history">here</a>.

## ----Data Context----

Before starting analysis to answer the team's main questions, I was tasked with creating clean datasets from the original Colorado data. The clean data sets would include only relevant columns, and would take out discrepancies found in the data.

The original datasets the team used were from Colorado emissions testing results. Data from 2010-2016 was available, and each testing year's data held 500,000 - 1,000,000 rows and 250+ columns. Each row included data from a single vehicle inspection including information about the vehicle such as make/model/year/engine data, as well as test and inspection results.

In Colorado, generally speaking, two emissions tests were run on vehicles:

1. An OBD test. This test checks for conditions that waste fuel and shorten engine life, such as a loose gas cap.
3. An IM240 test. This test puts a vehicle on a treadmill and simulates different driving conditions. A sensor put at the tailpipe during this test measures actual emissions of carbon monoxide (CO), hydrocarbons (HC), and nitrogen oxides (NOx) from the vehicle. These emissions are compared against applicable EPA thresholds to determine the overall test result.

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

Note: In the below I used data from 2012. This process can be replicated for any years' data.


# ----Loading The Data----

First, I loaded in the dataset and found it's length for reference. This gave me a measure of how many emissions tests were conducted in 2012 overall.

Next I filtered out any columns that did not pertain to emissions analysis, as determined by our team and advisors. This allowed me to decrease memory usage while running code and increase data readability.

```python

Data_2012 = pd.read_csv("./data_source.csv")
original_number_of_rows = len(Data_2012)
Data_2012 = VTR2012[["V_VIN","V_DATE_TIME" ,"V_MAKE", "V_MODEL", "V_VEH_YEAR", "V_ODOMETER" , "V_CO", "V_HC", "V_NOX", "V_CO_STD", "V_HC_STD", "V_NOX_STD", "V_OBD_RES", "V_EM_RES", "V_RESULT" , "V_TRANS", "V_CYLINDERS", "V_DISP", "V_DRIVE"]
```

# ----Data Cleaning and Preparation----

To provide usable data for analysis, I conducted cleaning/preparation procedures. These procedures included the following:
1. VIN cleaning: vehicle VINS need to be standardized so that a subset of the VIN can be used for aggregation purposes
2. Emissions results preparation: Any ambiguous or unknown emissions values must be imputed so resulting analyses can be taken in proper context. Results must also be labelled numerically so that it can be more easily summarized/aggregated
3. Initial/Final test result preparation: To understand how emissions may improve due to testing, there needs to be a way to compare the first and last emissions test results for each unique vehicle
4. IM240/OBD results preparation: To conduct analyses on each type of test, data sets will need to be generated that only contain records with full emissions test results in them.
5. Emissions preparation: To analyze emissions, data sets were aggregated two ways: (1)to show emissions by model year and testing year, and (2) to show an estimated amount of pollutants saved as a result of emissions testing.

## Cleaning Vehicle VIN Numbers

One goal the team identified was to cluster vehicles by their VIN number to see whether particular car makes/models perform better than others. To facilitate this goal, the team created a unique ID to define vehicle classes using VINS. The team had learned that particular digits in a vehicle VIN identify it's make, model, year, and engine type. 

A standard vehicle VIN has 17 digits. When looking at the original Colorado dataset, there were many vehicles with shorter VIN lengths. To ensure that vehicles were being grouped and analyzed correctly,all records with shorter/longer VINs were not included in the cleaned data set.

To start the VIN cleaning process, I removed whitespace in the VIN column so that VIN numbers could be grouped properly.

```python
Data_2012["V_VIN"].str.strip()
```

Similar to the VIN process above, white space was removed from the car make and model columns.

```python
Data_2012["V_MAKE"].str.strip()
Data_2012["V_MODEL"].str.strip()
```

Next, all instances where the vehicle VIN wasn't 17 characters long were removed from the dataset. The team had no way of knowing which characters were added ro removed from non-17 digit VINS, and thus thought it best to remove those data points rather than impute with a random guess.

```python
Data_2012 = Data_2012[len(Data_2012["V_VIN"])==17]

#variable to track how many rows are remaining in our cleaned data set
number_of_rows_w_valid_VIN = len(Data_2012)
```

Next, the team created the new unique ID for a vehicle make/model/year to aid in data aggregation. The group called this identifier a "mini-VIN," because it was a subset of the overall VIN number.

```python
Data_2012["mini_vin"] = Data_2012["mini_vin"][1:4]+Data_2012["mini_vin"][7:8]+Data_2012["mini_vin"][9:10]
```

## ----Emissions Results Data Cleaning----

The three pollutants that emissions tests measure are hydrocarbons (HC), carbon monoxide (CO) and nitrogen oxides (NOx). To simplify analysis any instances where an HC, CO, or NOx emission result was missing was removed.

```python
#omit instances where emission results were null
Data_2012 = Data_2012[(Data_2012["V_HC"].notnull()) & (Data_2012["V_CO"].notnull()) & (Data_2012["V_NOx"].notnull())]
```

All vehicles that use fossil fuels will emit the aforementioned pollutants into the air. To quantify the impact of emissions testing on these vehicles, any instances were HC/CO/NOx levels are 0 are removed.

The team noted that zeros could be the result of a state inspection conducted on an electric vehicle, or an omission during data entry but could not confirm the cause for certain.

```python

#omit instances where emission results were 0
Data_2012 = Data_2012[(Data_2012["V_HC"]>0) & (Data_2012["V_CO"]>0) & (Data_2012["V_NOx"]>0)]

#variable to track how many rows are remaining in our cleaned data set
number_of_rows_w_nonzero_emissions = len(Data_2012)
```
The dataset was sorted by inspection date and vin, so the team could scroll through and quickly see instances where a vehicle had been tested multiple times.

This could be used to find a case study of a vehicle tested multiple times, and the emissions improvements that resulted from re-testing.

```python
Data_2012 = Data_2012.sort_values(by=['V_VIN', "V_DATE_TIME"], axis=0, ascending=True)
```

Finally, I replaced the Pass and Fail results in the OBD and IM240 results columns with ones and zeros, for more efficient data aggregation in the future.

I also replaced any unclear or ambiguous results in the overall test result column with 0, making the assumption for now that any result that wasn't a "P" was a fail

```python
Data_2012["V_OBD_RES"].replace({"P":1, "F":0, " ":0, "C":10, "1":0,"0":0, "B":0, "M":0}, inplace=True)
Data_2012["V_EM_RES"].replace({"P":1, "F":0}, inplace=True)
Data_2012["V_RESULT"].replace({"P":1, "F":0, "W":10, "A":0, "V":0,"H":0, "5":0,"9":0,"10":0}, inplace=True)
```

To make this process more efficient, I created the function below that takes a dataset as amn input and runs it through the above steps to return a cleaned dataset.

```python
def data_cleaning_function(df):
    df["V_OBD_RES"].replace({"P":1, "F":0, " ":0, "C":0, "1":1,"0":0, "B":0, "M":0}, inplace=True)
    df["V_EM_RES"].replace({"P":1, "F":0}, inplace=True)
    df["V_RESULT"].replace({"P":1, "F":0, "W":0, "A":0, "V":0,"H":0, "5":0,"9":0,"10":0}, inplace=True)
    df["V_VIN"].str.strip()
    df["V_MAKE"].str.strip()
    df["V_MODEL"].str.strip()
    df = df[len(df["V_VIN"])==17]
    df["mini_vin"] = df["mini_vin"][1:4]+df["mini_vin"][7:8]+df["mini_vin"][9:10]
    return df
```

## Pollutants Saved Preparation

One of the group's questions was to determine how much pollution is prevented by conducting emissions tests. The team decided to approach this question by estimating the pollutants saved by testing.

To do this, two data sets were created:

One dataset included the first emissions test results from each unique vehicle tested.

The second dataset included the final emissions test results from each unique vehicle tested.

The values of HC, CO, and NOx were taken from the first and last tests and (in Part II) subtracted from each other for each vehicle. This value was multipliecd by an average mileage calculation to estimate to determine how much pollution could be saved from that vehicle per year. This pollutants saved measure was taken for each vehicle in the dataset and aggregated to come up with a final yearly emissions saved estimate.

In this portion of the project, the data was split into the two aforementioned data sets based on unique vehicle VIN and first/last inspection date as outlined above.

First, two datasets were created. The first contains records from each unique vehicle's first emissions inspection results. Unique vehicles are identified by VIN number.

A similar dataset was then created to track each vehicle's last emissions inspection results.

```python
first_tests_df = Data_2012.drop_duplicates(subset=["V_VIN"], keep="first")
last_tests_df = Data_2012.drop_duplicates(subset=["V_VIN"], keep="last")
```

As a quick measure, the total number of vehicles tested was determined by finding the length of the first_tests dataset.

```python
count_of_first_tests = len(first_tests_df)
```

Next, the datetime column of the data was transformed into a format usable in Python. The date from this datetime column was used to calculate the average miles travelled / year metric for a vehicle. This metric was calculated by dividing a vehicle's odometer reading by the difference between the testing year and the model year.

For example, if the model year is 2005, the testing date is is 2015 and the odometer reading is 100,000. The average miles per year calc is:
- 100,000 miles / (2015-2005) = 10,000 miles/year on average

```python
data["date_time_year"]=data["V_DATE_TIME_first"].dt.year
data = data[(data['V_EM_RES_first']==0)&(data["date_time_year"]!=data["V_VEH_YEAR"])]
data["avg_miles_travelled"] = data["V_ODOMETER_first"]/(data["date_time_year"]-data["V_VEH_YEAR"])
```

Next the difference in emissions for a vehicle was calculated for CO, HC and NOx. For a vehicle, this difference would be the difference between the applicable vehicle's first and last emissions result.

Finally, the emissions difference for each vehicle and pollutant type was multiplied by the average miles/year measure to find a theoretical value for pollutants saved due to emissions testing. These values were summed and grouped into one dataset for easier viewing.

Note that the original pollutants were measured in grams, and the team converted this into tonnes for the final measures for easier interpretation.

```python
# only include data in which the emissions result was higher than the standard. 
#otherwise if a car is better, the value would be negative and would throw off the aggregate pollutants saved values
final_HC_df = data[data["V_HC_first"]>=data["V_HC_STD"]]

final_HC_df["Theoretical HC Saved (tonnes)"]=((final_HC_df["V_HC_first"]-final_HC_df["V_HC_last"])*final_HC_df["avg_miles_travelled"])/907185
final_HC_df=final_HC_df.groupby(by="testing_year").agg({"Theoretical HC Saved (tonnes)":"sum"})
    
#the same process is repeated for NOx and CO
```

To make the above process more efficient, I created the below function. It takes in a dataset, runs it through the above steps and returns a final dataset showing emissions savings for an applicable testing year.

```python
def first_last_test(df,test_year):
    #change datetime col so it si usable in Python
    df["V_DATE_TIME"]=pd.to_datetime(df["V_DATE_TIME"]) 
    #create a first and last test df
    first_tests_df = df.drop_duplicates(subset=["V_VIN"], keep="first")
    last_tests_df = df.drop_duplicates(subset=["V_VIN"], keep="last")
    
    #select only relevant cols for analysis
    first_tests_df=first_tests_df[['V_VIN', 'V_DATE_TIME','V_MAKE', 'V_MODEL', 'V_CO_STD','V_NOX_STD','V_HC_STD','V_VEH_YEAR', 'V_ODOMETER', 'V_CO', 'V_HC','V_NOX', 'V_EM_RES']]
    last_tests_df=last_tests_df[['V_VIN', 'V_DATE_TIME','V_ODOMETER', 'V_CO', 'V_HC','V_NOX', 'V_EM_RES']]
    
    #rename cols to make future column names more straightforward when mergin df's
    first_tests_df.rename(columns={"V_CO": "V_CO_first", "V_HC": "V_HC_first","V_NOX": "V_NOX_first","V_DATE_TIME": "V_DATE_TIME_first", "V_ODOMETER":"V_ODOMETER_first", "V_EM_RES":"V_EM_RES_first"}, inplace=True)
    last_tests_df.rename(columns={"V_CO": "V_CO_last", "V_HC": "V_HC_last","V_NOX": "V_NOX_last","V_DATE_TIME": "V_DATE_TIME_last", "V_ODOMETER":"V_ODOMETER_last", "V_EM_RES":"V_EM_RES_last"}, inplace=True)
    
    #merge the first and last test df's on VIN number
    merged_df=first_tests_df.merge(last_tests_df,left_on="V_VIN", right_on="V_VIN")
    #set testing year and extract year from datetime
    merged_df["testing_year"]=test_year
    merged_df["date_time_year"]=merged_df["V_DATE_TIME_first"].dt.year
    #include only data where a vehicle didn't pass an emissions test and where vehicle year didn't equal test year (avoid divide by zero error)
    merged_df = merged_df[(merged_df['V_EM_RES_first']==0)&(merged_df["date_time_year"]!=merged_df["V_VEH_YEAR"])]
    #calculate average miles travelled measure
    merged_df["avg_miles_travelled"] = merged_df["V_ODOMETER_first"]/(merged_df["date_time_year"]-merged_df["V_VEH_YEAR"])
    
    #calculate pollutants saved for HC, CO, NOx
    final_HC_df = merged_df[merged_df["V_HC_first"]>=merged_df["V_HC_STD"]]
    final_HC_df["Theoretical HC Saved (tonnes)"]=((final_HC_df["V_HC_first"]-final_HC_df["V_HC_last"])*final_HC_df["avg_miles_travelled"])/907185
    final_HC_df=final_HC_df.groupby(by="testing_year").agg({"Theoretical HC Saved (tonnes)":"sum"})
    
    final_NOX_df = merged_df[merged_df["V_NOX_first"]>=merged_df["V_NOX_STD"]]
    final_NOX_df["Theoretical NOX Saved (tonnes)"]=((final_NOX_df["V_NOX_first"]-final_NOX_df["V_NOX_last"])*final_NOX_df["avg_miles_travelled"])/907185
    final_NOX_df=final_NOX_df.groupby(by="testing_year").agg({"Theoretical NOX Saved (tonnes)":"sum"})
    
    final_CO_df = merged_df[merged_df["V_CO_first"]>=merged_df["V_CO_STD"]]
    final_CO_df["Theoretical CO Saved (tonnes)"]=((final_CO_df["V_CO_first"]-final_CO_df["V_CO_last"])*final_CO_df["avg_miles_travelled"])/907185
    final_CO_df=final_CO_df.groupby(by="testing_year").agg({"Theoretical CO Saved (tonnes)":"sum"})

    #merge pollutant df's, for easier interpretation of results
    final = final_HC_df.merge(final_CO_df, left_on="testing_year",right_on="testing_year").merge(final_NOX_df, left_on="testing_year",right_on="testing_year")
    
    #return final df
    return final
```

Using the above function, datasets for each testing year were created accordingly:

```python
theoretical_pollutants_saved_df_2010 = first_last_test(Data_2010_IM240_clean,2010)
theoretical_pollutants_saved_df_2011 = first_last_test(Data_2011_IM240_clean,2011)
theoretical_pollutants_saved_df_2012 = first_last_test(Data_2012_IM240_clean,2012)
theoretical_pollutants_saved_df_2013 = first_last_test(Data_2013_IM240_clean,2013)
theoretical_pollutants_saved_df_2014 = first_last_test(Data_2014_IM240_clean,2014)
theoretical_pollutants_saved_df_2015 = first_last_test(Data_2015_IM240_clean,2015)
theoretical_pollutants_saved_df_2016 = first_last_test(Data_2016_IM240_clean,2016)
```

## IM240 and OBD Dataset Preparation

Another question the team had was how often IM240 and OBD results differed, and what was the impact of these differences.

Thus far data cleaning/prep was focused on the overall columns necessary for clustering and analysis. To approach the above question, the team wanted to create statistics for OBD and IM240 tests both separately and combined.

To do this, three general types of datasets needed to be prepared:

- A dataset where all OBD results were valid, for use on OBD-only analysis
- A dataset where all IM240 results were valid, for us on IM240-only analysis
- A dataset where all OBD and IM240 results were valid, for use in comparing IM240 and OBD results head-to-head

First, a dataset for OBD results was made. This was done by only including pass (1) or fail (0) results. All other results indicated a problem, omission, or ambiguity in the test results.

```python
Data_2012_OBD_clean = first_tests_df[(first_tests_df["V_OBD_RES"]==1)|(first_tests_df["V_OBD_RES"]==0)]

count_of_OBD_clean_data = len(Data_2012_OBD_clean)
```

Second, a dataset for IM240 results was made. This was done by only including pass (1) or fail (0) results. All other results indicated a problem, omission, or ambiguity in the test results.

```python
Data_2012_IM240_clean = first_tests_df[(first_tests_df["V_EM_RES"]==1)|(first_tests_df["V_EM_RES"]==0)]

count_of_IM240_clean_data = len(Data_2012_IM240_clean)
```

Lastly, a dataset for OBD & IM240 results was made. This was done by only including pass (1) or fail (0) results for IM240 and OBD tests. All other results indicate a problem, omission, or ambiguity in the test results.

```python
Data_2012_Both_clean = first_tests_df[((first_tests_df["V_EM_RES"]==1)|(first_tests_df["V_EM_RES"]==0)) & (first_tests_df["V_OBD_RES"]==1)|(first_tests_df["V_OBD_RES"]==0)]

Data_2012_Both_clean = Data_2012_Both_clean[Data_2012_Both_clean["V_RESULT"].notnull()]

count_of_both_clean_df = len(Data_2012_Both_clean)
```

## Pass/Fail Dataset Generation

With the above 3 datasets created, data could be grouped and analyzed. This would help analyze the group's questions regarding a model year's emissions over time, in the lens of each type of test.

### General Process

For IM240 and OBD datasets, the process to get overall pass rates by model year are the same:
- group the clean OBD or IM240 dataset by model year
- aggregate the inspection result column two ways: count, to find total number of tests conducted, and sum, to find total number of inspections passed
- rename columns for readability
- divide the number of passed rates by number of tests conucted to find an overall pass rate for the given model year fleet
- append a testing year column for graphing purposes

To this end, I created the function below to make the process more efficient. It takes a dataset, testing year, and metric (OBD or IM240) as inputs and outputs a summarized dataset with passing rates by model year.

```python
def pass_rate_by_model_year(df,year,test,metric):
    df = df[["V_VEH_YEAR", metric]].groupby(by="V_VEH_YEAR", axis=0).agg({metric: ["count","sum"]})
    df.columns=df.columns.droplevel(0)
    df.rename(columns={"count": "Model_year_"+test+"_tests_conducted", "sum":"Model_year_"+test+"_passed_tests"}, inplace=True)
    df["Model_Year_"+test+"_Pass_Rate"] = 100*df["Model_year_"+test+"_passed_tests"]/df["Model_year_"+test+"_tests_conducted"]
    df["testing_year"]=year
    return df
```

### ---- IM240 PASS RATE ----

Using the above function, I found the passing rates for each testing year's IM240 data. 

```python
pass_rate_by_model_year_IM240_df_2010=pass_rate_by_model_year(Data_2010_IM240_clean,2010,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2011=pass_rate_by_model_year(Data_2011_IM240_clean,2011,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2012=pass_rate_by_model_year(Data_2012_IM240_clean,2012,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2013=pass_rate_by_model_year(Data_2013_IM240_clean,2013,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2014=pass_rate_by_model_year(Data_2014_IM240_clean,2014,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2015=pass_rate_by_model_year(Data_2015_IM240_clean,2015,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2016=pass_rate_by_model_year(Data_2016_IM240_clean,2016,"IM240","V_EM_RES")

pass_rate_by_model_year_IM240_df_2010.to_csv("./pass_rate_by_model_year_IM240_df_2010.csv")
pass_rate_by_model_year_IM240_df_2011.to_csv("./pass_rate_by_model_year_IM240_df_2011.csv")
pass_rate_by_model_year_IM240_df_2012.to_csv("./pass_rate_by_model_year_IM240_df_2012.csv")
pass_rate_by_model_year_IM240_df_2013.to_csv("./pass_rate_by_model_year_IM240_df_2013.csv")
pass_rate_by_model_year_IM240_df_2014.to_csv("./pass_rate_by_model_year_IM240_df_2014.csv")
pass_rate_by_model_year_IM240_df_2015.to_csv("./pass_rate_by_model_year_IM240_df_2015.csv")
pass_rate_by_model_year_IM240_df_2016.to_csv("./pass_rate_by_model_year_IM240_df_2016.csv")

```
### ---- OBD PASS RATE ----

Next I found the passing rates for each testing year's OBD data. 

```python
pass_rate_by_model_year_IM240_df_2010=pass_rate_by_model_year(Data_2010_IM240_clean,2010,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2011=pass_rate_by_model_year(Data_2011_IM240_clean,2011,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2012=pass_rate_by_model_year(Data_2012_IM240_clean,2012,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2013=pass_rate_by_model_year(Data_2013_IM240_clean,2013,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2014=pass_rate_by_model_year(Data_2014_IM240_clean,2014,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2015=pass_rate_by_model_year(Data_2015_IM240_clean,2015,"IM240","V_EM_RES")
pass_rate_by_model_year_IM240_df_2016=pass_rate_by_model_year(Data_2016_IM240_clean,2016,"IM240","V_EM_RES")
```

### ---- IM240 AND OBD PASS RATE ----

Finally I found the passing rates for each testing year's data where a vehicle passed both tests. 

Note that the function needed to be altered for this aggregation. A column was created to add up the inspection results. A vehicle that passed both tests should have two '1' values for a total of 2.

```python
def pass_rate_by_model_year_both_tests(df,year,test):
    df["two_test_passed"]= (df["V_OBD_RES"]+df["V_EM_RES"]==2).astype(int)
    df = df[["V_VEH_YEAR", "two_test_passed"]].groupby(by="V_VEH_YEAR", axis=0).agg({"two_test_passed": ["count","sum"]})
    df.columns=df.columns.droplevel(0)
    df.rename(columns={"count": "Model_year_"+test+"_tests_conducted", "sum":"Model_year_"+test+"_passed_tests"}, inplace=True)
    df["Model_Year_"+test+"_Pass_Rate"] = 100*df["Model_year_"+test+"_passed_tests"]/df["Model_year_"+test+"_tests_conducted"]
    df["testing_year"]=year
    return df
```

```python
pass_rate_by_model_year_both_pass_df_2010=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2010,2010,"both_tests")
pass_rate_by_model_year_both_pass_df_2011=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2011,2011,"both_tests")
pass_rate_by_model_year_both_pass_df_2013=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2013,2013,"both_tests")
pass_rate_by_model_year_both_pass_df_2014=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2014,2014,"both_tests")
pass_rate_by_model_year_both_pass_df_2015=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2015,2015,"both_tests")
pass_rate_by_model_year_both_pass_df_2016=pass_rate_by_model_year_both_tests(Data_OBD_and_IM240_clean_2016,2016,"both_tests")
```

## Emissions, Make and Model Data Generation

In this section, data was created to summarize the total vehicles with a given make, model, and year in separate tables. This provided insights into the distribution of vehicles in the state and allow us to create average emissions figures based on total emissions and total vehicles of a given type.

This was done for each emission type separately, after which the data was aggregated.

For each pollutant, the following steps were followed:
- a subset of the overall dataset was taken with only the vehicle make, model, year and relevant emission total included
- this dataset was grouped by model and make
- this dataset also included aggregate columns to show emissions summary statistics for each miniVIN/make/model combination 
- finally, the resultant dataframe was formatted so that column labels could be read more easily

```python
# function to get the first and third quartile emissions value for a given miniVIN/make/model vehicle
def percentile(n):
    def percentile_(x):
        return np.percentile(x, n)
    percentile_.__name__ = 'percentile_%s' % n
    return percentile_
```

```python
CO_summary = Data_2012[["V_MAKE","V_MODEL", "V_CO"]].groupby(by=["mini_vin", "V_MODEL", "V_MAKE"], axis=0).agg({"mini_vin":["count"],"V_CO": ["min",percentile(25),"median","mean",percentile(75),"max"]})
CO_summary.columns=CO_summary.columns.droplevel(0)
CO_summary.rename(columns={"count": "mini_vin_count", "min":"V_CO_min", "percentile_25": "V_CO_1st_qua.", "median":"V_CO_median","mean":"V_CO_mean","percentile_75":"V_CO_3rd_qua.","max":"V_CO_max"}, inplace=True)
CO_summary.reset_index(inplace=True)

HC_summary = Data_2012[["V_MAKE","V_MODEL", "V_HC"]].groupby(by=["mini_vin", "V_MODEL", "V_MAKE"], axis=0).agg({"V_HC": ["min",percentile(25),"median","mean",percentile(75),"max"]})
HC_summary.columns=HC_summary.columns.droplevel(0)
HC_summary.rename(columns={"min":"V_HC_min", "percentile_25": "V_HC_1st_qua.", "median":"V_HC_median","mean":"V_HC_mean","percentile_75":"V_HC_3rd_qua.","max":"V_HC_max"}, inplace=True)
HC_summary.reset_index(inplace=True)

NOX_summary = Data_2012[["V_MAKE","V_MODEL", "V_NOX"]].groupby(by=["mini_vin", "V_MODEL", "V_MAKE"], axis=0).agg({"V_NOX": ["min",percentile(25),"median","mean",percentile(75),"max"]})
NOX_summary.columns=NOX_summary.columns.droplevel(0)
NOX_summary.rename(columns={"min":"V_NOX_min", "percentile_25": "V_NOX_1st_qua.", "median":"V_NOX_median","mean":"V_NOX_mean","percentile_75":"V_NOX_3rd_qua.","max":"V_NOX_max"}, inplace=True)
NOX_summary.reset_index(inplace=True)

```

After statistics were generated for each pollutant type, the above tables were joined on miniVIN, make, and model to create one summary table.

```python
summary_stats_2012 = CO_summary.merge(HC_summary, how='inner', left_on=["V_MAKE","V_MODEL"],right_on=["V_MAKE","V_MODEL"]).merge(NOX_summary, how='inner', left_on=["V_MAKE","V_MODEL"],right_on=["V_MAKE","V_MODEL"])
summary_stats_2012.drop(['index'],axis=1,inplace=True)
```

## Model Year Emission Analysis Preparation

To answer part of the team's third question, I aggregated the data by model year and calculated the average emissions rates for each testing year. To do this, I created the function below to group the data and calculate the appropriate mean values. The team hoped that this method, implemented in the function below, woudl reveal if pollution control devices in vehicles appeared to degrade over time.

```python
def model_year_pollution_degradation(df,year):
    df = df.drop_duplicates(subset=["V_VIN"], keep="first")
    df = df[["V_VIN","V_VEH_YEAR","V_CO", "V_HC","V_NOX"]].groupby(by=["V_VEH_YEAR"], axis=0).agg(V_CO_mean=("V_CO", "mean"),V_HC_mean=("V_HC", "mean"),V_NOX_mean=("V_NOX", "mean"))
    df["testing_year"]=year
    df.reset_index()
    return df
```

The above function was used to generate data for each testing year as shown below:

```python
model_year_pollution_2010_df=model_year_pollution_degradation(Data_2010_IM240_clean, 2010)
model_year_pollution_2011_df=model_year_pollution_degradation(Data_2011_IM240_clean, 2011)
model_year_pollution_2014_df=model_year_pollution_degradation(Data_2013_IM240_clean, 2013)
model_year_pollution_2015_df=model_year_pollution_degradation(Data_2014_IM240_clean, 2014)
model_year_pollution_2016_df=model_year_pollution_degradation(Data_2015_IM240_clean, 2015)
model_year_pollution_2016_df=model_year_pollution_degradation(Data_2016_IM240_clean, 2016)
```

## Conclusion
With the above steps taken, the team now had the following cleaned datasets at its disposal:

- Dataset with complete inspections for both IM240 and OBD results
- Dataset including records with complete IM240 results, regardless of whether OBD results where available
- Dataset including records with complete OBD results, regardless of whether OBD results where available
- Dataset including each unique vehicle's first emissions test only
- Dataset including each unique vehicle's last emissions test only
- Dataset showing pollutants saved estimates for each testing year
- Dataset showing inspection passing rates for each testing year and each test type
- Dataset showing summary statistics for each vehicle make/model/year's emissions testing results

The above data could now be used to investigate the team's questions.

### Summary of Remaining Data

The following summary table has been created to show how many rows remain in each dataset to be used.

|Year	|Original count	|17-digit VIN count	|Non-zero emissions count	|First tests count	|Clean OBD count	|Clean IM240 count	|Both cleaned count	|
|---|---|---|---|---|---|---|---|
|2016	|1,140,730	|1,130,164	|729,680	|535,631	|403,683	|535,412	|403,571	|
|2015	|1,149,083	|1,135,860	|712,018	|521,870	|385,160	|521,613	|384,997	|
|2014	|1,264,855	|1,248,393	|1,104,735	|845,512	|684,367	|845,349	|684,272	|
|2013	|1,288,070	|1,270,072	|1,127,618	|856,142	|676,829	|856,008	|676,742	|
|2012	|1,212,144	|1,192,982	|1,052,938	|806,980	|622,752	|806,835	|622,677	|
|2011	|451,792	|443,254	|379,461	|337,732	|243,230	|337,675	|243,209|	
|2010	|514,305	|504,838	|426,018	|362,410	|251,915	|362,308	|251,860	|



