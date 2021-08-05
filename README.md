Link to csv files
- https://www.kaggle.com/c/ashrae-energy-prediction/data

**Default names for CSV are used.
Move all CSV into a folder titled 'energyCSV'. This file should sit in the master root.**

Code for memory reduction function
- Koustav Banerjee: https://www.kaggle.com/c/ashrae-energy-prediction/data

Code for getMissingInfo function
- Alexander Sylvester: https://www.kaggle.com/alexandersylvester

# Business Goal
To take in information about meter readings for a building(s) provide an accurate prediction of what their next year meter readings might look like. The use case for this model is to be used in comparison after a building has switched to green energy for a period of time. This is to show the amount of money that would save in comparison to what the would have spend had they not switched to green energy.

# The Data
The data comes from a Kaggle.com competition hosted by ASHRAE.

The data is comprised of 6 datasets.
- **train** (include into about meter readings, timestamps for those meter readings, and building id to merge on)
- **test** (include into about meter readings, timestamps for those meter readings, and building id to merge on)
- **weather_train** (include information about the weather and the corresponding timestamps)
- **weather_test** (include information about the weather and the corresponding timestamps) 
- **building_metadata** (includes information about buildings, building attributes, and associated site ids)
- **sample_submission** (an example of the format for submitting to the kaggle competition)

# Cleaning the data
Several columns were removed for reasons related to either being unnecessary or reducing the size of the data for sake of running the model faster or not running out of memory.

To that extent datasets 'test', 'weather_test', and 'sample_submission' were unused for being too large to use efficiently.

In dataset 'weather_train' the following columns were removed:
- timestamp (after using to produce new columns)
- air_temperature (after using to produce new columns)
- dew_temperature (after using to produce new columns)
- precip_depth_1_hr (unnecessary)
- sea_level_pressure (unnecessary)
- wind_direction (unnecessary)
- wind_speed (unnecessary)

Column 'air_temperature' was split into 4 new columns before removing:
- air_temp_bellow_0
- air_temp 0to18
- air_temp_18to23
- air_temp_above_23

Column 'dew_temperature' was split into 3 new columns before removing:
- dew_temp_bellow_0
- dew_temp 0to13
- de_temp_above_13

Column 'timestamp' was split into 2 new columns before removing:
- hour
- month

Target column 'meter_reading' was made into an int to round up the numbers and avoid passing an int type into the model. I allow rounding the numbers up since meter readings are typically taken in whole numbers to begin with.

I also downsampled the data significantly for sake of running the model faster and for sake of memory restraints.

# Testing different model types
4 models were tested using light GridSearches
- RandomForestRegressor
- DecisionTreeRegressor
- ExtraTreesRegressor
- XGBRFRegressor

# Final Model (Model 1)
XGBRFRegressor produced the best Mean Squared Error, Root Mean Squared Error, Mean Absolute Error, and r2 scores.

A more extensive GridSearch was run on XGBRFRegressor for final socres

# Results
The final model got the following scored:
- **mean_squared_error Train Score:** 59368.652384292014
- **mean_squared_error Test Score:** 69993.42756228783

- **RMSE Train Score:** 243.6568332394805
- **RMSE Test Score:** 264.56271007511214

- **mean_absolute_error Train Score:** 85.83561596210124
- **mean_absolute_error Test Score:** 91.95932940991807

- **r2_score Train Score:** 0.9506766182509664
- **r2_score Test Score:** 0.9424364371970421

At a 94 percent accuracy per prediction of hourly meter reading, I believe the model walls within acceptable apramters to be trusted.