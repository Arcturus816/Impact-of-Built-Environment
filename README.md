# Impact of the Built Environment on Motor Vehicle Crashes

## Team members

- [Amanda Curtis](https://github.com/Arcturus816)
- [Arthur Diep-Nguyen](https://github.com/adn1995)
- [Olti Myrtaj](https://github.com/oltimyrtaj)
- [Brandon Owens](https://github.com/Brandon-Owens)
- [Fabio Ricci](https://github.com/HAL9009MATH)

## Introduction

There are more than 5 million motor vehicle crashes (MVCs) in the United States every year.
While some contributing factors are out of the hands of policy makers, others are not.
In our investigation, we seek to better understand the relationship between the
built environment of a community and the number and severity of crashes in that area.
Features of the built environment (which include roads, crosswalks, bike paths,
and traffic signals) can be augmented or replaced.
To more easily discuss the number and severity of crashes across heterogeneous regions,
we define the concept of "crash density" as

$$
\frac{\text{crashes} \times \text{severity}}{\text{population density}}
$$

for a given area.
By modeling the relationship between built environment features and crash density,
we can better predict the number of severe MVCs a community may expect in certain areas.
Such knowledge could help local governments and city planning organizations
better anticipate and respond to MVCs with emergency services and other resources,
or consider ways to lower the crash density of existing and future regions,
thus making their communities safer for both drivers and pedestrians.

## Data sets

We used two data sets, one for MVCs, and another for features of the built environment.

The MVC data comes from
[US Accidents (2016-2023)](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents)
on Kaggle.[^kaggle]
The data set contains approximately 7.7 million accident records spanning the
contiguous United States from 2016 to 2023, scraped from Bing and MapQuest
Traffic APIs and augmented with data from various entities, such as government
agencies.
The data set has 46 columns, with variables including accident location, time,
and severity, as well as many categorical variables describing weather
conditions, visibility conditions, and proximity to points-of-interest like
junctions, traffic stops, roundabouts, etc.

[^kaggle]: For more information about the Kaggle data set, see the
  [Kaggle page](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents)
  ([archive](https://archive.today/2025.04.21-223913/https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents)
  on archive.today), as well as the accompanying articles from the authors of
  the data set:
  [A Countrywide Traffic Accident Dataset](https://arxiv.org/abs/1906.05409)
  ([archive](https://web.archive.org/web/20250321054247/https://arxiv.org/pdf/1906.05409)
  on the Wayback Machine),
  and
  [Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights](https://arxiv.org/abs/1909.09638)
  ([archive](https://web.archive.org/web/20250224100248/https://arxiv.org/pdf/1909.09638)
  on the Wayback Machine).

The data about the built environment comes from the [Smart Location Database](https://www.epa.gov/smartgrowth/smart-location-mapping)
from the EPA.[^epa]
The data set contains data for every census block group (CBG) in the United States,[^cbg]
with over 200,000 rows.
The data set has over 90 variables across various categories such as housing
density, diversity of land use, neighborhood design, destination accessibility,
transit service, employment, demographics, etc.
Notably, the data set comes not as a CSV file, but as a file geodatabase.[^gdb]

[^epa]: For more information about the Smart Location Database, see the
  [EPA's website](https://www.epa.gov/smartgrowth/smart-location-mapping)
  ([archive](https://web.archive.org/web/20250412100204/https://www.epa.gov/smartgrowth/smart-location-mapping)
  on the Wayback Machine), as well as the accompanying documentation:
  [Smart Location Database Technical Documentation and User Guide Version 3.0](https://www.epa.gov/system/files/documents/2023-10/epa_sld_3.0_technicaldocumentationuserguide_may2021_0.pdf)
  ([archive](https://web.archive.org/web/20250412095056/https://www.epa.gov/system/files/documents/2023-10/epa_sld_3.0_technicaldocumentationuserguide_may2021_0.pdf) on the Wayback Machine).

[^cbg]: A census block group is the smallest geographic entity for which the
  US Census publishes sample data.
  For more information about census block groups, see the [Glossary](https://www.census.gov/programs-surveys/geography/about/glossary.html)
  ([archive](https://web.archive.org/web/20250421225546/https://www.census.gov/programs-surveys/geography/about/glossary.html)
  on the Wayback Machine) from the US Census Bureau.

[^gdb]: For more information on file geodatabases, see the
[Wikipedia article](https://en.wikipedia.org/wiki/Geodatabase_(Esri))
([archive](https://archive.today/2025.04.21-230032/https://en.wikipedia.org/wiki/Geodatabase_(Esri)) on archive.today).

## Data augmentation and processing

Before we could begin, we needed to combine our two data sets.
One possible approach would be to add built environment variables from the
EPA's Smart Location Database to each MVC in the Kaggle data set.
Such an approach would mean that our model would be observing features
of crashes, then predicting the severity of the crash.
However, this approach does not address the likelihood of a crash happening;
it addresses only how the built environment attenuates severity, given that a
crash already happened.

Thus, we adopted a different approach:
Instead of attaching variables from the Smart Location Database to each crash
from the Kaggle data set, we instead aggregated the crashes by CBG.
With this approach, our model would observe features of a CBG,
then predict the number of severity-weighted crashes that occurred during the
timeframe when the crash data was collected.

To engineer our target variable, we took the severity variable from Kaggle
to create a "severity-weighted" crash, so that our prediction model would
weigh severe crashes more heavily than light crashes.
We also realized that, since CBGs are highly heterogeneous in terms of both
population and land area, we would need to somehow control for these
confounding variables.
Ideally, we would have controlled for vehicle miles traveled in each CBG.
While the Smart Location Database had variables with `VMT` in the name, we
could not find explanations of these variables in the
[Smart Location Database Technical Documentation and User Guide](https://www.epa.gov/system/files/documents/2023-10/epa_sld_3.0_technicaldocumentationuserguide_may2021_0.pdf),
so we decided not to use these variables.
Instead, we controlled our target variable by population density.
Thus, we engineered our target variable to be

$$
\text{"crash density"} =
\frac{\text{crashes} \times \text{severity}}{\text{population density}}
$$

To merge the two data sets, we used GeoPandas to take the latitude-longitude data
from each motor vehicle crash and convert those coordinates to the same
Coordinate Reference System used by the Smart Location Database.
We could then determine the census block group in which each crash occurred by
using spatial join in GeoPandas.

We cleaned the data of rows with nonsensical or extreme data
(e.g. CBGs with zero population, zero land area, zero roads, etc.).
We chose to exclude CBGs with fewer than 4 crashes, since a
CBG with fewer than 4 crashes from 2016 to 2023 probably has negligible motor
vehicle activity.
We also excluded crashes from the year 2020, since driving and pedestrian
activity during that year was impacted by the COVID-19 pandemic lockdown.

After cleaning our data, we performed log transformations on highly skewed
variables, which increased their interpretability.

## Feature selection

During our feature selection process, we sought to reduce our number of features to fewer than 20.
We began by highlighting features that had a moderate correlation
(roughly $0.3 < |R^2| < 0.8$) to our target variable, as indicated by correlation heat maps.
We eliminated features by selecting between highly correlated variables to
reduce multicollinearity.
We also found redundant variables that we could eliminate.
For example, some columns indicated the number of households with 0, 1, or 2 automobiles in a given CBG,
while others listed the respective percentages of each in the same CBG.

The data analysis and visualization that aided in feature selection can be
found in [`data_visualizations.ipynb`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/code/data_visualizations.ipynb).
Ultimately, we were able to trim our number of features down to 17 while maintaining a representative sample
of features.

## Model selection and results

After performing an 80/20 train-test split of our data set and spending some time on exploratory data analysis,
we began to train some models and compare their performances using root mean squared error (RMSE) as a performance metric.
We started with Multiple Linear Regression as a baseline model and went on to consider Lasso and Ridge regression,
Random Forest Regression and XGBoost Regression.
For each of these models we used 5-fold cross validation while tuning hyperparameters in an attempt to minimize overfitting.
In the end, XGBoost performed the best, scoring a RMSE of 0.552 on the training data.
We chose to use this tuned XGBoost model as our final model, and found that it scored a RMSE of 0.547 on the test data.

## Files

### Notebooks

- [`data_processing.ipynb`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/code/data-processing.ipynb) contains our data processing pipeline
- [`data_visualizations.ipynb`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/code/data_visualizations.ipynb) contains visualizations of our data
- [`geospatial_visualizations`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/code/geospatial_visualizations.ipynb) contains geospatial visualizations, mapping our target variable and some feature variables
- [`model_fitting`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/code/model_fitting.ipynb) contains our model training and evaluation

### CSVs

We did not include the raw data sets in our repo because they are too large.
- You can find the US Accidents data as a CSV on [Kaggle](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents).
- You can find the Smart Location Database as a file geodatabase on the [EPA's website](https://www.epa.gov/smartgrowth/smart-location-mapping).

Our [`data_processing.ipynb`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/code/data-processing.ipynb)
notebook generated the following processed data sets on our repo.
- [`cbg_feature_select_and_transform_train`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_feature_select_and_transform_train.csv) and [`bg_feature_select_and_transform_test`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_feature_select_and_transform_test.csv) are the most inclusive version of our processed data
- [`cbg_gt3crashes_feature_select_and_transform_train`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_gt3crashes_feature_select_and_transform_train.csv) and [`cbg_gt3crashes_feature_select_and_transform_test`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_gt3crashes_feature_select_and_transform_test.csv) exclude census block groups with fewer than 4 crashes
- [`cbg_no2020_feature_select_and_transform_train`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_no2020_feature_select_and_transform_train.csv) and [`cbg_no2020__feature_select_and_transform_test`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_no2020__feature_select_and_transform_test.csv) exclude crashes from the year 2020
- [`cbg_no2020_gt3crashes_feature_select_and_transform_train`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_no2020_gt3crashes_feature_select_and_transform_train.csv) and [`cbg_no2020_gt3crashes_feature_select_and_transform_test`](https://github.com/Arcturus816/Impact-of-Built-Environment/blob/main/data/processed/CSVs/cbg_no2020_gt3crashes_feature_select_and_transform_test.csv) are the data sets that we ended up using
