# Impact of the Built Environment on Motor Vehicle Crashes

## Team members

- [Amanda Curtis](https://github.com/Arcturus816)
- [Arthur Diep-Nguyen](https://github.com/adn1995)
- [Olti Myrtaj](https://github.com/oltimyrtaj)
- [Brandon Owens](https://github.com/Brandon-Owens)
- [Fabio Ricci](https://github.com/HAL9009MATH)

## Introduction

## Data sets

We used two data sets, one for motor vehicle crashes, and another for features
of the built environment.

The motor vehicle crash data comes from [US Accidents (2016-2023)](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents)
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
  the data set.
  - [A Countrywide Traffic Accident Dataset](https://arxiv.org/abs/1906.05409) ([archive](https://web.archive.org/web/20250321054247/https://arxiv.org/pdf/1906.05409) on the Wayback Machine)
  - [Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights](https://arxiv.org/abs/1909.09638) ([archive](https://web.archive.org/web/20250224100248/https://arxiv.org/pdf/1909.09638) on the Wayback Machine)

The data about the built environment comes from the [Smart Location Database](https://www.epa.gov/smartgrowth/smart-location-mapping)
from the EPA.[^epa]
The data set contains data for every census block group in the United States,[^cbg]
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
([archive]() on archive.today).

## Data processing and feature selection

## Model selection and results

## Files

### Notebooks

### CSVs
