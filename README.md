# Case_Study_Bellabeat
**About the company:**
Bellabeat is a high-tech company that manufactures health-focused products. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with  knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned  itself as a tech-driven wellness company for women.

# 1. Ask 
 ## 📊 Business Task

This project analyzes Fitbit smart device usage data to uncover customer trends and behavioral patterns. These insights will support Bellabeat’s marketing team by identifying what features and habits users most value, helping guide product marketing strategies and future development.

---

## 👥 Key Stakeholder
- **Urska Srsen** – Bellabeat Co-founder and Chief Creative Officer  
- **Sando Mur** – Mathematician and Bellabeat Co-founder  
- **Bellabeat Marketing Team** – Drives marketing strategies based on insights

# 2. Prepare

### 2.1 Data At Hand

The dataset was generated by Fitbit trackers and is stored in 18 CSV files, each corresponding to different tracked activities. It contains data for 30 users collected between **March 12, 2016** and **May 12, 2016**.  
The data includes various metrics such as:
- Daily activity  
- Heart rate  
- Sleep patterns  
- Steps taken, and more  

The dataset is provided in long format.

---

### 2.2 Limitations

- The dataset is small, with data from only 30 users.  
- It is outdated (from 2016).  
- Data collection is limited to specific days in the week (Tuesday to Thursday only).

---

### 2.3 Does the Data Follow ROCCC?

**ROCCC** stands for:  
**R**eliable, **O**riginal, **C**omprehensive, **C**urrent, and **C**ited.

- ❌ **Reliable**: The data comes from only 30 users who self-reported via Amazon Mechanical Turk.  
- ❌ **Original**: The data was distributed by third-party vendors.  
- ⚠️ **Comprehensive**: While minute-level data is available for some metrics, the timeframe and population size limit its completeness.  
- ❌ **Current**: The dataset is nearly a decade old (2016).  
- ❌ **Cited**: The original source (Amazon MTurk) is not verifiable or peer-reviewed.

➡ Overall, the dataset **does not fully meet the ROCCC criteria**.

# 3. Process
I used R to conduct my analysis as it is easy to use and keeping in mind the amount of data we have.

### Install and Load required Packages 
```r
install.packages("tidyverse")
install.packages("janitor")
install.packages("ggpubr")
install.packages("here")
install.packages("skimr")
install.packages("ggrepel")

library(tidyverse)
library(janitor)
library(ggpubr)
library(here)
library(skimr)
library(ggrepel)
library(lubridate)
library(dplyr)
```
### Import datasets
For my analysis, I worked on three datasets and imported them to R for my Business Task.
- dailyActivity_merged
- dailyIntenisties_merged
- hourlyCalories_merged
```r
dailyActivity_merged <- read_csv("dailyActivity_merged.csv")
hourlyCalories_merged <- read_csv("hourlyCalories_merged.csv")
dailyIntensities_merged <- read_csv("dailyIntensities_merged.csv")
```
### Display Dataset
```r
head(hourlyCalories_merged)
str(hourlyCalories_merged)
```

### Data Formatting and Preprocessing (Blue Color represents output respectively for corressponding code)
**1. Unique Combinations**
```r
nrow(distinct(dailyActivity_merged, Id, ActivityDate)) 
[1] 940
nrow(distinct(dailyIntensities_merged, Id, ActivityDay))
[1] 940
nrow(distinct(hourlyCalories_merged, Id, ActivityHour))
[1] 22099
```
**2. Duplicates**
```r
> sum(duplicated(dailyActivity_merged))
[1] 0
> sum(duplicated(dailyIntensities_merged))
[1] 0
> sum(duplicated(hourlyCalories_merged))
[1] 0
```
**3. Remove Duplicates and N/A**
```r
Dataser<-Dataset %>% distinct() %>% drop_na()  #inplace of dataset replace with one of the dataset variable where you saved your dataset while reading
```
**4. Renaming columns**
Change the format of columns to lowercase to maintain consistency.
```r
> dailyActivity_merged <- rename_with(dailyActivity_merged, tolower)
> dailyIntensities_merged <- rename_with(dailyIntensities_merged, tolower)
> hourlyCalories_merged <- rename_with(hourlyCalories_merged, tolower)
```
**5. Cleaning**
Changes the format of columns so that they are consistent. Ex: Replaces spaces and special characters with underscores, Removes or substitutes non-ASCII characters, Ensures valid R variables.
```r
clean_names(dataset)
```
**6.Date and Time Consistency**
Formatting the Date and time columns of all CSV files for proper consistency.
```r
data <- read_csv("dailyActivity_merged.csv")
hourlyCalories_merged <- hourlyCalories_merged %>% rename(date_time = activityhour) %>% mutate(date_time = mdy_hms(date_time))
dailyIntensities_merged <- dailyIntensities_merged %>% rename(date = activityday) %>% mutate(date, format = "%m/%d/%Y")
> view(dailyIntensities_merged)
> data <- data %>% 
+    rename(date = activitydate) %>% 
+    mutate(date = as_date(date, format = "%m/%d/%Y"))
> view(data)
```
**7. Merging datasets**
Merging the dailyActivity_merged and dailyIntensities_merged csv files to do the analysis
```r
> dailyActivity_Intensity <- merge(data, dailyIntensities_merged, by = c("id", "date"))
```



