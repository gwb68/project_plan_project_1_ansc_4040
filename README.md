# Project Plan: Filling Missing Dairy Milk Production Data

**Course:** ANSC 4040 — Mini Project  
**Project Timeline:** September 15 – October 6, 2026  
**Development Environment:** Local computer using Visual Studio Code (VS Code) and Python

## Project Purpose

The purpose of this project is to fill in missing milk production data in a dairy farm dataset. Sometimes sensors used to identify cows through their ear tags malfunction or break, which causes gaps in the data. These missing records make it difficult to accurately track individual cow production. My goal is to develop a reliable method for estimating missing milk production values based on the cow's previous and surrounding production records.

## Strategy and Model Choice

I will first gather and organize the raw milk production data. I will then process and clean the data using Python and pandas by identifying missing values, removing or addressing incorrect records, and organizing the data by cow ID and milking time. After the data is cleaned, I will split the available data into training and testing sets so I can evaluate how accurately the model predicts milk production values that are already known.

My main goal is to create a prediction model that uses the available milk flow data to estimate missing milk production values. The model will learn the production patterns of each cow and use those patterns to predict values when a sensor fails to record milk production. Instead of simply filling in missing values with an average, the model will use the surrounding and previous milk production data to make a more informed prediction.

The final goal is to use these predictions to create a continuous lactation curve for each cow, even when some milk production records are missing. I will compare the predicted values to the actual values in the testing data using an accuracy measure such as mean absolute error (MAE). Based on the results, I will determine whether the model is accurately predicting missing milk production and whether it can be reliably applied to the actual missing records in the dataset.

## Timeline

| Date | Project Task |
|---|---|
| September 15–16 | Understand and prepare the data. Identify the dataset, inspect its columns, locate null values, and organize records by cow ID and date. |
| September 17–18 | Explore the data. Analyze individual cow milk production patterns, examine missing-data gaps, and create visualizations. |
| September 19–22 | Develop the filling strategy. Create a baseline using cow-level averages and test interpolation for short gaps. Investigate regression or other prediction methods for longer gaps. |
| September 23–26 | Test and compare models. Hide known milk production values, predict them using each method, and compare the estimates with the actual values. |
| September 27–30 | Finalize the data pipeline. Apply the selected method to the actual missing records, document the process, and preserve the original data. |
| October 1–5 | Review the results, create final visualizations, explain limitations, and prepare the final report or presentation. |
| October 6 | Submit the completed mini project. |

## Testing

To test the accuracy of my method, I will use artificial missingness. I will temporarily remove some milk production values that are already known and use the model to predict them. I can then compare the predicted values with the original measurements using mean absolute error (MAE). MAE will show how many pounds of milk the predictions differ from the actual values on average.

I will test different sizes of missing-data gaps and compare multiple methods to determine which one performs best. I will also make sure that the model does not use future information that would not have been available when a prediction is actually needed. This will help me determine whether the method is reliable for real sensor-related data gaps.

## Data Lineage

I will keep track of where the dataset came from, when it was downloaded, and how the data was cleaned and processed. I will preserve the original dataset and create separate versions for the cleaned data and the completed data.

Each estimated milk production value will be labeled as an estimate rather than an actual sensor measurement. I will retain the cow ID, date, and milking time, along with the method used to fill the gap. I will also document any assumptions, changes, or limitations. This will make it possible to trace each prediction back to the original data and distinguish actual measurements from estimated values.

## Expected Outcome

By October 6, I expect to have a reproducible Python workflow in Visual Studio Code that identifies missing milk production records, fills gaps using an appropriate prediction method, and evaluates the accuracy of the results. The final project will also explain the strengths and limitations of the chosen method and provide a clear record of how the original data was processed.
