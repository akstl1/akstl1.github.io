---
layout: post
title: Air Quality Benefits of Emissions Testing: Data Cleaning
image: "/posts/vehicle_emissions.jpg"
tags: [Python, Parkinson's, Classification]
---
 ## Intro here
 
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

## Cleaning Vehicle VIN Numbers

One goal the team identified was to cluster vehicles by their VIN number to see whether particular car makes/models perform worse than others. To start this process, I removed whitespace in the VIN column so that VIN numbers can be grouped properly.

```r
VIN_cleaned <- trimws(VTR2012$V_VIN,"both")
VIN_cleaned <- as.data.frame(VIN_cleaned)
```

Now that the VIN number was cleaned, I could append the cleaned column to the main dataset.

```r
#appends the cleaned vin category to the original dataset
VTR2012 <- transform(VTR2012, VIN_cleaned = VIN_cleaned)
```

Similar to the VIN process above, white space was removed from the car make and model columns and the resultant columns were appended to the main dataset.

```r
# trim white space from the car make, and append to the main dataset
cleaned_car_make <- trimws(VTR2012$V_MAKE,"both")
cleaned_car_make <- as.data.frame(cleaned_car_make)

VTR2012 <- transform(VTR2012, cleaned_car_make = cleaned_car_make)


# trim white space from the car model, and append to the main dataset
MODEL_cleaned <- trimws(VTR2012$V_MODEL,"both")
MODEL_cleaned <- as.data.frame(MODEL_cleaned)

VTR2012 <- transform(VTR2012, MODEL_cleaned = MODEL_cleaned)
```

A vehicle VIN number should be 17 characters in length. The team found that some VIN numbers in the data were not the proper length. 

Each character/combination of characters in a VIN corresponds to a particular vehicle characteristic. Because there is no way in knowing which characters were omitted/added to non-17 character VINS, all rows with such VINs listed must be omitted to ensure each instance in the data is properly classified.

```r
#filters the data so only rows with a 17 digit cleaned_vin show up
VTR2012 <- VTR2012[nchar(as.character(VTR2012$VIN_cleaned)) == 17 ,]

#general count of how many instances are left after filtering out improper VINs
digit_17_VIN_count <- length(VTR2012$V_VIN)
digit_17_VIN_count <- as.data.frame(digit_17_VIN_count)
```

Now a new variable was created using a subset of digits from the VIN number. This subset indicates the vehicle type, which will be used for further grouping.

This variable was then inserted into the main dataset

```r
mini_vin <- substr(VTR2012$VIN_cleaned, 2 ,4)
mini_vin <-paste(mini_vin, substr(VTR2012$VIN_cleaned, 8,8), sep = "")
mini_vin <-paste(mini_vin, substr(VTR2012$VIN_cleaned, 10,10), sep = "")
mini_vin <- as.data.frame(mini_vin)

VTR2012 <- cbind(VTR2012, mini_vin)
```

## Emissions Results Cleaning

The three pollutants that emissions tests look for are hydrocarbons (HC), carbon monoxide (CO) and nitrogen oxides (NOx). To simplify analysis any instances where an HC, CO, or NOx emission result was missing was removed.

```r
VTR2012 <- subset(VTR2012, !is.na(V_HC & V_CO & V_NOX))
```

All vehicles that use fossil fuels will emit the aforementioned pollutants into the air. To quantify the impact of emissions testing on these vehicles, any instances were HC/CO/NOx levels are 0 are removed. 

The team noted that zeros could be the result of a state inspection conducted on an electric vehicle, or an omission during data entry but could not confirm the cause for certain.

```r
VTR2012 <- VTR2012[VTR2012$V_HC > 0 & VTR2012$V_CO > 0 & VTR2012$V_NOX > 0 ,]
#gets rid of data that has all zeros for emissions results.

#below the total number of non_zero instances remaining is found
non_zero_emissions_count <- length(VTR2012$V_VIN)
non_zero_emissions_count <- as.data.frame(non_zero_emissions_count)
```

The dataset was sorted by inspection date and vin, so the team could scroll through and quickly see instances where a vehicle had been tested multiple times.

This could be used to find a case study of a vehicle tested multiple times, and the emissions improvements that resulted from re-testing. 

```r
sorted_list <- VTR2012[with(VTR2012, order(VIN_cleaned, V_DATE_TIME, decreasing=FALSE)),] 
#sort the data by date and vin
```

## Pollutants Saved Preparation

One of the group's questions was to determine how much pollution is prevented by conducting emissions tests. The team decided to approach this question by estimating the pollutants saved by testing.

To do this, two data sets were created:

One dataset included the first emissions test results from each unique vehicle tested.

The second dataset included the final emissions test results from each unique vehicle tested.

The values of HC, CO, and NOx were taken from the first and last tests and (in analysis portion) subtracted from each other for each vehicle. This value was multipliecd by an average mileage calculation to estimate to determine how much pollution could be saved from that vehicle per year. This pollution saved value was taken for each vehicle in the dataset and aggregated to come up with a final yearly emissions saved value. 

First, the data was split into the two aforementioned data sets based on unique vehicle VIN and first/last inspection date.

```r
first_tests <- sorted_list[!duplicated(sorted_list$VIN_cleaned, fromLast=FALSE),]
last_tests <- sorted_list[!duplicated(sorted_list$V_VIN, fromLast=TRUE),]
```

As a quick measure, the total number of vehicles tested was determined by finding the length of the first_tests dataset.

```r
#determines the count of vehicles that were tested total in the given year
count_of_first_tests <- length(first_tests$V_VIN)
count_of_first_tests <- as.data.frame(count_of_first_tests)
```

## IM240 and OBD Dataset Preparation

Another question the team had was how often IM240 and OBD results differed, and what was the impact of these differences. 

Thus far data cleaning was focused on the overall columns necessary for clustering and analysis. To approach the above question, the team wanted to create statistics for OBD and IM240 tests both separately and as a whole.

To do this, three general types of datasets needed to be prepared:
- A dataset where all OBD results are valid, for use on OBD-only analysis
- A dataset where all IM240 results are valid, for us on IM240-only analysis
- A dataset where all OBD and IM240 results are valid, for use in comparing IM240 and OBD results head-to-head

First, a dataset for OBD results was made. This was done by only including pass ("P") or fail ("F") results. All other results indicated a problem, omission, or ambiguity in the test results.

```r
VTR2012_OBD_clean_2012 <- first_tests[first_tests$V_OBD_RES == "P" | first_tests$V_OBD_RES == "F",]

#determines count of vehicles tested in total, once the above filter was applied
count_of_OBD_clean <- length(VTR2012_OBD_clean_2012$V_VIN)
count_of_OBD_clean <- as.data.frame(count_of_OBD_clean)
```

Second, a dataset for IM240 results was made. This was done by only including pass ("P") or fail ("F") results. All other results indicated a problem, omission, or ambiguity in the test results.

```r
VTR2012_IM240_clean <- first_tests[first_tests$V_EM_RES == "P" | first_tests$V_EM_RES == "F",]

count_of_IM240_clean <- length(VTR2012_IM240_clean$V_VIN)
count_of_IM240_clean <- as.data.frame(count_of_IM240_clean)
```

Lastly, a dataset for OBD & IM240 results was made. This was done by only including pass ("P") or fail ("F") results for IM240 and OBD tests. All other results indicate a problem, omission, or ambiguity in the test results.

```r
VTR2012_Both_clean <- first_tests[(first_tests$V_EM_RES == "P" | first_tests$V_EM_RES =="F") & (first_tests$V_OBD_RES == "P" | first_tests$V_OBD_RES == "F")),]
#creates data set with clean OBD and IM240 results (P/F in all results columns)

VTR2012_Both_clean <- subset(VTR2012_Both_clean, !is.na(V_RESULT))
#filters out any instances with NA values out of the RESULT field

count_of_both_clean <- length(VTR2012_Both_clean$V_VIN)
count_of_both_clean <- as.data.frame(count_of_both_clean)
```

## Mini Vin Pass/Fail Dataset Generation

With the above 3 datasets created, miniVIN data could be grouped and analyzed. This would help analyze the group's questions regarding a model year's emissions over time, in the lens of each type of test. 

To start, I made a list of the miniVINS present in the cleaned IM240 dataset, and got a count for reference.

```r
#this section generates the mini vins for each data set in a proper order to run in the for loop later
#mini_vins become a unique identifier for vehicle types
mini_vin_tests_IM240 <- VTR2012_IM240_clean[order(VTR2012_IM240_clean$mini_vin),]
mini_VIN_IM240 <- table(as.character(mini_vin_tests_IM240$mini_vin))
mini_VIN_IM240_2012 <- as.data.frame(mini_VIN_IM240)


count_of_IM240_minivin <- length(mini_VIN_IM240_2012$Var1)
count_of_IM240_minivin <- as.data.frame(count_of_IM240_minivin)
#generates a table of all the unique minivins for the IM240 analysis and their counts in the data set
```

Similarly, a list of miniVINs in the cleaned OBD dataset was generated.

```r
mini_vin_tests_OBD <- VTR2012_OBD_clean_2012[order(as.character(VTR2012_OBD_clean_2012$mini_vin)),]
mini_VIN_OBD <- table(as.character(mini_vin_tests_OBD$mini_vin))
mini_VIN_OBD_2012 <- as.data.frame(mini_VIN_OBD)
#generates a table of all the unique minivins for the OBD analysis and their counts in the data set
```

Finally, a list of miniVINs in the dataset with cleaned OBD and IM240 test results was created.

```r
mini_vin_tests_both_clean <- VTR2012_Both_clean[order(VTR2012_Both_clean$mini_vin),]
mini_VIN_both_clean <- table(as.character(mini_vin_tests_both_clean$mini_vin))
mini_VIN_both_clean_2012 <- as.data.frame(mini_VIN_both_clean)
#generates a table of all the unique minivins for the overall pass rate analysis and their counts in the data set
```

With the cleaned data sets and miniVINs from each dataset created, I could generate new datasets summarizing the passing statistics for each miniVIN and each test result. This allowed the team to analyze overall passing statistics and the breakdown of passing rates by vehicle type.

This was done below. First, objects were initialized to store the passing statistics.

Next, three loops were created. Each had the same function, but looped through the different datasets. The loop functions as follows. For i, with i ranging up to the number of unique miniVINs in the dataset:

- A progress message is shown every hundredth iteration; this can be removed as desired
- A new entry is created in the relevant initialized object (described above). That entry consists of a count of rows where the inspection test is passed and the miniVIN is equal to the i'th term in the unique miniVIN list

```r
#reminder --= need to initialize some objects to hold results

bothpass <- vector(mode="numeric", length=length(unique(VTR2012_Both_clean$mini_vin)))
IMpass <- vector(mode="numeric", length=length(unique(VTR2012$mini_vin)))
OBDpass <- vector(mode="numeric", length=length(unique(VTR2012_OBD_clean_2012$mini_vin)))

for (i in 1:length(unique(VTR2012_IM240_clean$mini_vin))) {
  if (i %% 100 ==0) {
    cat(paste(i, length(unique(VTR2012_IM240_clean$mini_vin)), sep=": "), "\n")
  }
  IMpass[i] <- nrow(VTR2012_IM240_clean[which(VTR2012_IM240_clean$V_EM_RES %in% c("P") & VTR2012_IM240_clean$mini_vin == unique(mini_vin_tests_IM240$mini_vin)[i]),])
}

for (i in 1:length(unique(VTR2012_OBD_clean_2012$mini_vin))) {
  if (i %% 100 ==0) {
    cat(paste(i, length(unique(VTR2012_OBD_clean_2012$mini_vin)), sep=": "), "\n")
  }
  OBDpass[i] <- nrow(VTR2012_OBD_clean_2012[which(VTR2012_OBD_clean_2012$V_OBD_RES %in% c("P") & VTR2012_OBD_clean_2012$mini_vin == unique(mini_vin_tests_OBD$mini_vin)[i]),])
}

for (i in 1:length(unique(VTR2012_Both_clean$mini_vin))) {
  if (i %% 100 ==0) {
    cat(paste(i, length(unique(VTR2012_Both_clean$mini_vin)), sep=": "), "\n")
  }
  bothpass[i] <- nrow(VTR2012_Both_clean[which(VTR2012_Both_clean$V_RESULT %in% c("P") & VTR2012_Both_clean$mini_vin == unique(mini_vin_tests_both_clean$mini_vin)[i]),])
}
#the above loops find the number of vehicles that pass the IM240/OBD/both tests by miniVIN
```

Now that I quantified how many vehicles pass each test, I created a passing rate for each vehicle type as well. I did this by dividing the quantity of passing results by total tests conducted for each miniVIN type.

```r
# create passing rates for IM240 dataset
IMpass <- as.data.frame(IMpass)
IMpass_rates <- cbind(mini_VIN_OBD_2012, IMpass)
IMpass_rates <- transform(IMpass_rates, IMpass_rates = 100*(IMpass_rates$IMpass_rates/IMpass_rates$Freq))

# create passing rates for OBD dataset
OBDpass <- as.data.frame(OBDpass)
OBD_pass_rates <- cbind(mini_VIN_OBD_2012, OBDpass)
OBD_pass_rates <- transform(OBD_pass_rates, OBD_pass_rate = 100*(OBD_pass_rates$OBDpass/OBD_pass_rates$Freq))

# create passing rates for combined dataset
bothpass <- as.data.frame(bothpass)
overall_pass_rates <- cbind(mini_VIN_both_clean_2012, bothpass)
overall_pass_rates <- transform(overall_pass_rates, overall_pass_rate = 100*(overall_pass_rates$bothpass/overall_pass_rates$Freq))
```

## Emissions, Make and Model Data Generation

In this section, data was created to summarize the total vehicles with a given make, model, and year in separate tables. This  provided insights into the distribution of vehicles in the state and allow us to create average emissions figures based on total emissions and total vehicles of a given type.

This was done below. First, empty vector objects were initiated to store data about pollutants, make, model and year.

Next a for loop was initiated. For i, ranging up to the length of the miniVIN dataset:

- a new dataframe, 'subset_data' is created with only miniVINs that match the i'th miniVIN
- a make table is generated, showing the number of vehicles with the given make as well as the vehicle make itself. This is appended to the make_data_list object
- similar to the make table, model and model year information is aggregated and stored into objects
- summary statistics are then created for the miniVIN subset data for each pollutant. This shows us the min, max, average, and quantiles for emissions of CO/HC/NOx for the given miniVIN

```r
#create some objects
CO_data_list <- vector(mode="list", length=nrow(mini_VIN_IM240_2012)) #will hold summary stats for each minivin
Nox_data_list <- vector(mode="list", length=nrow(mini_VIN_IM240_2012)) #will hold summary stats for each minivin
HC_data_list <- vector(mode="list", length=nrow(mini_VIN_IM240_2012)) #will hold summary stats for each minivin
make_data_list <- vector(mode="list", length=nrow(mini_VIN_IM240_2012)) #will hold summary stats for each minivin
model_data_list <- vector(mode="list", length=nrow(mini_VIN_IM240_2012)) #will hold summary stats for each minivin
year_data_list <- vector(mode="list", length=nrow(mini_VIN_IM240_2012)) #will hold summary stats for each minivin

for (i in 1:nrow(mini_VIN_IM240_2012)) { #for each minivin
  if (i %% 100 ==0) {
    cat(paste(i, nrow(mini_VIN_IM240_2012), sep=": "), "\n")
  }
  mini_vin <- as.character(mini_VIN_IM240_2012$Var1)[i]
  #get the "i"th minivin
  
  #new dataframe with instances in the original dataset that have the mini_vin currently being analyzed
  subset_data <- VTR2012[which(as.character(VTR2012$mini_vin) == mini_vin),]
  
  #creates a tables using the above filtered dataset, which sums the number of vehicles with a given make
  make_table <- table(subset_data$V_MAKE)
  make_table <- as.data.frame(make_table)
  make_table <- make_table[with(make_table, order(Freq, decreasing =TRUE)),]
  make_data_list[[i]] <- as.character(make_table[1,1])
  
  #creates a tables using the above filtered dataset, which sums the number of vehicles with a given model type
  model_table <- table(subset_data$V_MODEL)
  model_table <- as.data.frame(model_table)
  model_table <- model_table[with(model_table, order(Freq, decreasing =TRUE)),]
  model_data_list[[i]] <- as.character(model_table[1,1])
  
  #creates a tables using the above filtered dataset, which sums the number of vehicles with a given model year
  year_table <- table(subset_data$V_VEH_YEAR)
  year_table <- as.data.frame(year_table)
  year_table <- year_table[with(year_table, order(Freq, decreasing =TRUE)),]
  year_data_list[[i]] <- as.character(year_table[1,1])
  
  #creates summary statistics for the given vehicle types' CO, NOX, HC emissions
  CO_data_list[[i]] <- summary(subset_data$V_CO)
  Nox_data_list[[i]] <- summary(subset_data$V_NOX)
  HC_data_list[[i]] <- summary(subset_data$V_HC)
}
```

The above data is now put into final objects. These are then bound into a final dataset to be used for future analysis. The dataset shows, for each miniVIN:
- make
- model
- model year
- emissions statistics for HC/CO/NOx

```r
#creates final lists showing emissions statistics and car data counts from the above loop
CO_data_list <- do.call(rbind, CO_data_list)
Nox_data_list <- do.call(rbind, Nox_data_list)
HC_data_list <- do.call(rbind, HC_data_list)
make_data_list <- do.call(rbind, make_data_list)
model_data_list <- do.call(rbind, model_data_list)
year_data_list <- do.call(rbind, year_data_list)

#creates a field to determine the IM240 pass rates by mini VIN
mini_VIN_IM240_2012 <- transform(mini_VIN_IM240_2012, IM240_pass = IMpass)
mini_VIN_IM240_2012 <- transform(mini_VIN_IM240_2012, IM240_pass_rate = 100*(mini_VIN_IM240_2012$IMpass/mini_VIN_IM240_2012$Freq))
mini_vin_summary_data_2012 <- cbind(mini_VIN_IM240_2012, make_data_list, model_data_list, year_data_list,CO_data_list, Nox_data_list, HC_data_list)
mini_vin_summary_data_2012 <- mini_vin_summary_data_2012[with(mini_vin_summary_data_2012, order(Freq, decreasing = TRUE)),]
#generates a table of summary stats of emissions for all the remaining mini vins in the data
```

## Conclusion

With the above steps taken, the team now had the following cleaned datasets at its disposal:
- Overall cleaned dataset, with complete inspections for both IM240 and OBD results
- Cleaned dataset including records with complete IM240 results, regardless of whether OBD results where available
- Cleaned dataset including records with complete OBD results, regardless of whether OBD results where available
- Overall cleaned dataset including each unique vehicle's first emissions test only
- Overall cleaned dataset including each unique vehicle's last emissions test only
- Dataset showing summary statistics for each vehicle make/model/year's emissions testing results

The above data could now be used to investigate the team's questions.

### Summary of Remaining Data

The following summary table has been created to show how many rows remain in each dataset to be used and how many unique miniVINs were recorded in each testing year.

| Year | Original count | 17-digit VIN count | Non-zero emissions count | First tests count | Clean OBD count | Clean IM240 count | Both cleaned count | IM240 miniVIN count |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 2016 | 1,140,730 | 1,130,164 | 729,680 | 535,631 | 403,683 | 535,412 | 403,571 | 12,557 |
| 2015 | 1,149,083 | 1,135,860 | 712,018 | 521,870 | 385,160 | 521,613 | 384,997 | 12,264 |
| 2014 | 1,264,855 | 1,248,393 | 1,104,735 | 845,512 | 684,367 | 845,349 | 684,272 | 14,793 |
| 2013 | 1,288,070 | 1,270,072 | 1,127,618 | 856,142 | 676,829 | 856,008 | 676,742 | 14,345 |
| 2012 | 1,212,144 | 1,192,982 | 1,052,938 | 806,980 | 622,752 | 806,835 | 622,677 | 13,791 |
| 2011 | 451,792 | 443,254 | 379,461 | 337,732 | 243,230 | 337,675 | 243,209 | 12,042 |
| 2010 | 514,305 | 504,838 | 426,018 | 362,410 | 251,915 | 362,308 | 251,860 | 11,951 |



