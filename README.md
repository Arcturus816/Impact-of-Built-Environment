# Impact of the Built Environment on Motor Vehicle Crashes

## Team members

- [Amanda Curtis](https://github.com/Arcturus816)
- [Arthur Diep-Nguyen](https://github.com/adn1995)
- [Olti Myrtaj](https://github.com/oltimyrtaj)
- [Brandon Owens](https://github.com/Brandon-Owens)
- [Fabio Ricci](https://github.com/HAL9009MATH)

## Introduction
There are more than 5 millioin motor vehicle accidents (MVCs) in the United States every year. While some contributing factors are out of the hands of policy makers, others are not. In our investigation, we seek to better understand the relationship between the built environment of a community and the number and severity of crashes in that area. Features of the built environment (which include roads, crosswalks, bike paths, and traffic signals) can be augmented or replaced. To more easily discuss the number and severity of crashes in a small region, we define the concept of "crash density" as (severity * number of crashes)/ (population density) for a given area. By modeling the relationship between built environment features and crash density, we can better anticipate the number of severe MVCs a community may expect in certain areas. Such knowledge could help local governments and city planning organizations better anticipate and respond to MVCs with emergency services and other resources, or consider ways to lower the crash density of existing and future regions; thus making their communities safer.

## Data sets

## Data processing and feature selection

Our data processing steps required the use of geopandas, as the location information for the datas sets we used were in different formats. Though this package, we were able to match the exact site of a crash with the Census Block Group (CBG) to which that location belongs. 

During our feature selection process, we sought to reduce our number of features to fewer than 20. We began by highlighting features that had a moderate correlation (roughly .3<|R^2|<.8)to our target variable, as indicated by correlation heat maps. We eliminated features by selecting between highly correlated variables to reduce multicolinearity and by eliminating redundant features. Work toward this goal can be found in ________, ____, within the repository.
This heat map indicates correlations between our final 17 features and our target variable. 

Some of our features were highly skewed and we performed a log_10 transformation on these to increase interpretability. 

## Model selection and results
After performing an 80/20 train-test split of our dataset and spending some time on exploratory data analysis, we began to train some models and compare their performances using root mean squared error (RMSE) as a performance metric. We started with Multiple Linear Regression as a baseline model and went on to consider Lasso and Ridge regression, Random Forest Regression and XGBoost Regression. For each of these models we used 5-fold cross validation while tuning hyperparameters in an attempt to minimize overfitting. In the end, XGBoost performed the best, scoring a RMSE of 0.552 on the training data. We chose to use this tuned XGBoost model as our final model, and found that it scored a RMSE of 0.547 on the test data.  

## Files

### Notebooks

### CSVs
