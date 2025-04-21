# Impact of the Built Environment on Motor Vehicle Crashes

## Team members

- [Amanda Curtis](https://github.com/Arcturus816)
- [Arthur Diep-Nguyen](https://github.com/adn1995)
- [Olti Myrtaj](https://github.com/oltimyrtaj)
- [Brandon Owens](https://github.com/Brandon-Owens)
- [Fabio Ricci](https://github.com/HAL9009MATH)

## Introduction

## Data sets

## Data processing and feature selection

## Model selection and results
After performing an 80/20 train-test split of our dataset and spending some time on exploratory data analysis, we began to train some models and compare their performances using root mean squared error (RMSE) as a performance metric. We started with Multiple Linear Regression as a baseline model and went on to consider Lasso and Ridge regression, Random Forest Regression and XGBoost Regression. For each of these models we used 5-fold cross validation while tuning hyperparameters in an attempt to minimize overfitting. In the end, XGBoost performed the best, scoring a RMSE of 0.552 on the training data. We chose to use this tuned XGBoost model as our final model, and found that it scored a RMSE of 0.547 on the test data.  

## Files

### Notebooks

### CSVs
