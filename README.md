# Prediction-of-Forest-Fires
In this project, I am using publicly available climate and environment data to predict the area burned due to forest fires.

# Capstone: Predicting Forest Fires

**Overview**: Science has changed the way we deal with natural disasters. Today's disaster management departments are able to plan better by being informed using past data and by data driven decisions.

One such problem is to predict the area burned due to forest fires. I am leveraging data set from https://archive.ics.uci.edu/dataset/162/forest+fires.This is a difficult regression task, where the aim is to predict the burned area of forest fires, in the northeast region of Portugal, by using meteorological and other data.

![image](https://github.com/user-attachments/assets/7f6b9140-4606-46e9-bd5e-541bbb15dce1)


**Scope**: In this project, my goal is to leverage Regressors such as K Nearest Neighbor, Linear Regression, Ensemble models, Support Vector Machines and neural networks to predict the burned area due to forest fires and compare their performance.

**Data Overview**: The data is related to Climate and environemnt data. Its is small dataset with 517 instances and 12 multivariate features.

## Section 1: Understanding the Features

Examine the data description below, and determine if any of the features are missing values or need to be coerced to a different data type.


```
Input variables:
Variable Name	Type	
X:              Integer	x-axis spatial coordinate within the Montesinho park map: 1 to 9		
Y:              Integer	y-axis spatial coordinate within the Montesinho park map: 2 to 9		
month:          Categorical	month of the year: 'jan' to 'dec'		
day:            Categorical	day of the week: 'mon' to 'sun'		
FFMC:           Continuous	FFMC index from the FWI system: 18.7 to 96.20		
DMC:            Integer         DMC index from the FWI system: 1.1 to 291.3		
DC:             Continuous	DC index from the FWI system: 7.9 to 860.6		
ISI:            Continuous	ISI index from the FWI system: 0.0 to 56.10		
temp:           Continuous	temperature: 2.2 to 33.30	Celsius degrees	
RH:             Integer	relative humidity: 15.0 to 100	%	
wind:       	Continuous	wind speed: 0.40 to 9.40	km/h	
rain:           Integer	outside rain: 0.0 to 6.4	mm/m2	

Output variable (desired target):
area is the Target which is of type Integer	
It represents the burned area of the forest: 0.00 to 1090.84 (this output variable is very skewed towards 0.0, thus it may make sense to model with the logarithm transform).
```

## Section 2: Feature Engineering
##### No null values to be removed
##### Encoded each categorical features value to boolean and scaled all numerical features
##### No need to remove any features since not any features are highly correlated to target (area)
##### The distribution of target - burned area is very skewed and gence, log transforming the data made area follow more like a normal distribution

## Section 3: Insights from Exploratory Data Analysis

#### - August and September seem to have high fire occurences.
#### - Weekends incling friday has higher burns than other days. I wonder if this is to do with majority of workers not being available to put off fires on weekend.s This however, is only a deduction and my interpretation of data.
#### - When we look at number of fires by month and day, it is clear that the fires are still high in August and September. However, there seems to be no particular skew or variability across days of the week.
#### - X and Y are binomial distributions.
#### - FFMC, is right skewed and so is DC and ISI.
#### - DMC also seems to be binomial distribution wih left skew.
#### - Rain and area are highly skewed and may need a logarithmic transformation.
#### - Distribution of wind is close to normal.
#### -  Relative humity is highest in January.
#### - Temperatue peaks around May and goes beyond 15 degrees and starts decresing around November to under 10 degrees.
#### - Wind peaks in the month of feb and continues to peak and reaches maximum in november and december, although it gradually decreases in oct before it peaks.
#### - High wind and temperature does not seem to guarentee high likelihood of higher burned area.
#### - Rain is highly peaking in only August. It is a wonder that burnt area is high in that same month as well.

## Section 4: Modeling : Performance Results Comparison after GridSearchCv

![image](https://github.com/user-attachments/assets/58bd2de9-e4a0-4866-8cf3-e8d52307f4ac)

#### - Ran Linear Regression, KNN, SVR, Decision Tree Regressor and Ridge regressors with no gridsearchCV. All have low mean square error but bad R2
#### - Ran XGB, KNN, Lasso with GridsearchCV. These models also have good MSE but bad R2.
#### - R2 is very bad & negative for all regressors including ensemble models after gridsearch.
#### - Cross validation did not improve score on linear regression
#### - Since R2 are negative, we need to go back to data features transformation and see if there is any standardization required

### For Next direction: 
#### - Lasso did best in terms of producing least test mse. Since Lasso did well and it does well with feature selection, it can deduced that lasso selected good features. 
#### - XGB is also a good due to its high train r2
#### - Hence lets consider feature selection and running ensemble models like XGB on the subselected features
#### - Apply Regularization techniques
#### - Try neural network model as well

## Section 5: Feature Selection to identify more important features

#### 1. Recursive Feature Elimination (RFE)
#### 2. Random Forest Regressors
Using RandomForestRegressor: Selected Features using Random Forest Regressor is 10/29: ['day_sat', 'Y', 'FFMC', 'ISI', 'DC', 'X', 'wind', 'DMC', 'RH', 'temp']
RFE did not earn much benefits

## Section 6: Model Tuning
#### Ran XGB, SVR and NN with gridsearch and on subset of features.

![PerformanceofRegressors](https://github.com/user-attachments/assets/36680551-9c66-43ca-ad9a-34811075c0b8)


## Section 7: Conclusion

#### I ran several regressors models to predict the burned area due to fires including: Linear Regression, KNNRegression, Decision Tree Regressor, Ridge, Lasso, Support Vector Regressor, XGB Regressor as well as Neural Network
#### KNN Regressor was the fastest to train and SVM was the slowest.

#### - All models are performing weakly with test R2 being negative.

#### - Amongst all models, XBG with selected feature and gridsearchCV features seem to be doing much better than most models.

Best Params: {'XGB__alpha': 2, 'XGB__eta': 0.01, 'XGB__gamma': 3, 'XGB__max_depth': 2, 'XGB__n_estimators': 100}
Train MSE: 1.828419406136051
Test MSE: 1.9694429508540994

![XGBPredictionResult](https://github.com/user-attachments/assets/df762d64-0e5f-406e-b0ce-5b1171365fe6)

## Recomendation

#### Add more data from more and iteratively refresh the model on new data to get latest to get better model Test R2. 
### Currently , we cannot derive any decisions on the regressors on the prediction of the burned area.
