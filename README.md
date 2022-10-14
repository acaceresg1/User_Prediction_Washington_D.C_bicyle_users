# User_Prediction_Washington_D.C_bicyle_service

## Code and Resources Used:

**Packages:** pandas, datetime, matplotlib, seaborn, sci-kit learn, numpy xgboost

**READme.doc**:
- instant: record index
- dteday : date
- hr : hour (0 to 23)
- weathersit : Weather situation
- temp : Normalized temperature in Celsius. The values are divided to 41 (max)
- atemp: Normalized feeling temperature in Celsius. The values are divided to 50 (max)
- hum: Normalized humidity. The values are divided to 100 (max)
- windspeed: Normalized wind speed. The values are divided to 67 (max)
- casual: count of casual users
- registered: count of registered users
- cnt: count of total rental bikes including both casual and registered

## Executive Summary:

Group project for Python II assignment at IE - University, Master in Business Analytics & Big Data

The goal of this assignment is to Predict the total number of Washington D.C. bicycle users on an hourly basis using a dataset (use attached hour.csv) with data from 2011 and 2012.

### *Exploratory Data Analysis:*
Initially a EDA was done to ensure data quality, plotting some meaningful figures, checking variable correlation and extracting some insights on what seems relevant for prediction and what does not.

### *Data Engineering:*
Once there ir an initial feeling, a data engineering study was needed to check for missing values and outliers. Also was needed the creation of some date features and extra features as they could help to find new combinations making possible the extraction of new insights.
All this process used scikit-learn pipelines to perform transformations.

### *Machine Learning:*

Finally, after preparing our dataset, we need to follow the following steps:
- Creation of our baseline Linear Regression model with the original features
- To add the new features into the baseline model
- Baseline model with a new algorithm, Random Forest for example.
- Tuning model parameters with validation
- Obtaining accurate predictions in test
- Plotting those predictions vs reality for additional insights. 

## Results and Insights:

### *EDA:*

The initial checking for the target value showed a pattern what help us to select a model for predicting time series. Among the variables, the correlation study showed
temp/atemp and registered/casual/cnt are highly correlated, something to take into account for further steps, there are some categorial variables like weathersit and hr which should be converted into ordinal values
and those with a skewed pattern like windspeed should be categorize into different labels. 
Now it is time for missing values and Outliers. 
- Initially there are not missing days (rows), although some days have missing hours. Taking into account the reduce number of missing hours (0.4% missing) would be enough if those rows are not used during the study.
- Through other features like weathersit, temp, atemp, hum & windspeed, some missing values are present, something to remember later in our pipeline to impute a value for those rows.
- Finally Outliers: hum and windspeed have outliers so it is needed to take a decision later about how to treat them.

### *Data Engineering:*

Now it is time to decide what to do with the previous challenges but also it is needed the creation of new features to extract better insights.
Missing Rows: Ignore them as they were only 0.4% of the data.
Missing Values: It will be use a linear interpolation to fill those missing values.
Categorical Values: They will be converted into cardinal.
Correlating Features: Checking in detail with a correlation matrix, and those highly correlated like atemp, registered and casual will be dropped during the ML part.
Outlier: 
- Strong winds is something normal in nature so they will be keept but to improve the results the values will be grouped into 6 bins.
- Humidity has some outliers too, but in this case all of them are zeros, something impossible in nature, so in this case they will be replaced with the mean.

To convert all this, it is needed not only the use of ColumnTransformer, also the use of a Prerocessor to return a dataframe within the pipeline with column names, making possible further transformations.

Generation of extra features:

- Different time features were created as month, season, year, dayofyear, dayofmonth, weekend, daylight, etc.

Once the creation of the new features is defined with their relative functions, a FeatureCreator is created to make possible to call everything in once from the pipeline. 

### *Machine Learning:*

1) Splitting the dataset in training and test data.
2) Setting up the pipeline  with the steps defined using the baseline of a RandomForestRegressor.
3) Model Tunning using grid search:
Here there is a iteration to find manually some good parameters from the grid search.
Once we have the best parameters (model__max_depth': 30, 'model__max_features': 'auto', 'model__n_estimators': 300) from the grid search we initialize our grid
using the pipeline defined before with the parameters selected. After initializing is time to fit, to find the best model and to predict with the best model.
After predicting the test data, it is obtained a R2 of 0.856 showing a high quality predictor.

The last part of the notebook is a graph showing in blue the real data and in orange the prediction. 

**This figure shows very well how the predictor fits the real data, and how it could predict future scenarios for the Bicycle service in Washington D.C.**
