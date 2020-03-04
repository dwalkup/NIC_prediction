# Nenana Ice Classic Prediction
Predicting the outcome of the Nenana Ice Classic

## Overview
This is my capstone project for the Flatiron School Data Science Bootcamp.

For my project, I chose to predict the winning day of the Nenana Ice Classic in Alaska. I chose this project for several reasons:
* I'm from Alaska, and the Nenana Ice Classic is a uniquely Alaskan thing.
* It seemed like a challenging project.
* Doing this project would involve several methods and types of data I hadn't worked with before.

In the course of this project, I learned more about modeling time-series data and working with weather data.

## Project Goal
My goal was to predict the winning day of the Nenana Ice Classic.

## Project Evaluation
I used accuracy as my evaluation metric.

The naive approach (which would be to predict "the ice will not break today" every day) is 98.27% accurate.

My model is successful if it can predict the winning day with accuracy greater than 98.27%.

## Methodology
I collected data from two primary sources:
* weather data <a href = https://darksky.net/poweredby/>Powered By DarkSky</a>'s API.
* ice measurements and historical winning times were taken from the <a href = https://www.nenanaakiceclassic.com/index.htm>Nenana Ice Classic official website</a>.

I chose to start with the year 1989, because that is the earliest year that ice measurements from the contest are available. I gathered data for the months of March through May, because the winning day is generally between April 20 and May 20.

Once I cleaned and merged the data, I ended up discarding all information for the years 1993 and 1995. Those years were missing weather information for many days, including the winning days.

I discarded all information for the days after the winning day in each year.

Initially, I engineered several features that I thought would be useful: the number of daylight hours per day, binary indicators for precipitation type (rain or snow), and a running total of snow accumulation in a year. Later, I added time-series features in the form of moving averages for 3-, 5-, 7-, and 10-day windows for most features in the dataset. 

I performed experiments with several models: Logistic Regression, Random Forests, Histogram Gradient Boosted Trees, Support Vector Machines, and a couple of stacked models combining Logistic Regression, Random Forests, and Support Vector Machines.

In the end, the most performant model was one where I fed the results of a Random Forest classifier to a Logistic Regression classification model.

## Findings
My best model's accuracy was 95.71%. This is below the success target of 98.27%, but I'm not completely dissatisfied with my results.

The model successfully predicted 4 of the 5 winning days in the testing set of data. The winning day in 2019 was the earliest in the contest's history, and I suspect that's why my model was unable to predict it successfully.

The model's predictions included 9 false positives (predicted winning days that weren't actually) and 1 false negative (2019, which I discussed above).

## Repository Contents
The root folder of this repository contains the final model notebook (ice_classic_modeling.ipynb) and a PDF copy of the accompanying presentation I created for this project, "Headed For A Breakup."

The presentation can also be seen on the Web at this link: https://www.canva.com/design/DAD1ayplZIg/zsh-FXgjj-vxKsmNy_nS_g/view?utm_content=DAD1ayplZIg&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton.

There are 2 subfolders in this repository, "data" and "notebooks."

### The data subfolder contains 8 files:

* "Raw" data (raw_weather_1989-2020.csv, raw_ice_thickness_1989-2019.csv, weather_data_1989-2020.json, ice_classic_winning_times.csv)
* The "cleaned" dataset: raw data combined into a single file before moving average features were added (cleaned_data.csv)
* The full dataset with moving average features added (features_added.csv)
* The final testing and training datasets (model_training_data.csv, model_testing_data.csv)

### The notebooks subfolder contains 7 files:

* Two notebooks for collecting data from the internet (ice_classic_scraper_darksky.ipynb, ice_classic_scraper_ice_data.ipynb)
* A notebook for data exploration and cleaning (ice_classic_data.ipynb)
* Four notebooks for modeling experimentation (ice_classic_logreg.ipynb, ice_classic_trees.ipynb, ice_classic_svm.ipynb, ice_classic_stacking.ipynb)

## Future Steps
I have several ideas in mind for future steps:
* Additional parameter tuning of my final model
* Gathering additional data:
  * Expand the window of weather data back to January 1
  * Gather river flow data from USGS
* Apply other models to the data:
  * Clustering, then using those results in another model, may provide insights
  * I attempted to use survival analysis to model this problem but it was very unstable. I'd like to revisit this technique.
* Once I can accurately predict the winning day, I will try to narrow the prediction window down. For instance, which half of the day the ice will break, or further, which hour the ice will break.