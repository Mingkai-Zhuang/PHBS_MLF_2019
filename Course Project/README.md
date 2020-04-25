# Predict Stock Returns with Past Few Days' Data

## Team Member(Group 08)

Student Name | GitHub ID | Student ID
:---------:  |:---------:|:---------:
Zhuang Mingkai| [Mingkai-Zhuang](http://github.com/Mingkai-Zhuang/PHBS_MLF_2019/) |1901212687
Zhou Chi|[zaynmalivski](http://github.com/zaynmalivski/PHBS_MLF_2019/) | 1901212684
Wang Ke|[KeWang-pku](http://github.com/KeWang-pku/) | 1901212641
Ni Ye|[NiYee](http://github.com/NiYee/) | 1901212623

## Project Introduction
* Goal: Predict the return of a stock, given the history of the past few days.
* The datasets provide 5-day windows of time, days D-2, D-1, D, D+1, and D+2. We are given returns in days D-2, D-1, and part of day D, and we will predict the returns in the rest of day D, and in days D+1 and D+2.
During day D, there is intraday return data, which are the returns at different points in the day. The datasets provide 180 minutes of data, from t=1 to t=180. In the training set we are given the full 180 minutes, in the test set just the first 120 minutes are provided.
For each 5-day window, datasets also provide 25 features, Feature_1 to Feature_25. Each row in the dataset is an arbitrary stock at an arbitrary 5 day time window.
![Explanation](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Images/Explanation.jpg)
* Weighted Mean Absolute Error(WMAE) is used to evaluate the results as the competition required. The formual is defined as

<div align=center>
	<img src="https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Images/formula.png">
</div>
where w_i is the weight associated with the return, y_i is the actual value, \hat{y_i} is the predicted value and n is the number of predictions. The weights are given in the original datasets.



## Data
### Data Download
The datasets downloaded from [Kaggle-The Winton Stock Market Challenge](https://www.kaggle.com/c/the-winton-stock-market-challenge/data).

### Data Files Descrition
**Note：** Because of the limited size of files uploaded to GitHub, we only upload a part of our datasets but include all specific labels.
* train.xlsx - the training set, including the columns of:
  * Feature_1 - Feature_25
  * Ret_MinusTwo, Ret_MinusOne
  * Ret_2 - Ret_120
  * Ret_121 - Ret_180: target variables
  * Ret_PlusOne, Ret_PlusTwo: target variables
  * Weight_Intraday, Weight_Daily
* test.xlsx - the test set, including the columns of:
  * Feature_1 - Feature_25
  * Ret_MinusTwo, Ret_MinusOne
  * Ret_2 - Ret_120

### Specific Label
* Feature_1 to Feature_25: different features relevant to prediction.
* Ret_MinusTwo: this is the return from the close of trading on day D-2 to the close of trading on day D-1 (i.e. 1 day).
* Ret_MinusOne: this is the return from the close of trading on day D-1 to the point at which the intraday returns start on day D (approximately 1/2 day).
* Ret_2 to Ret_120: these are returns over approximately one minute on day D. Ret_2 is the return between t=1 and t=2.
* Ret_121 to Ret180: intraday returns over approximately one minute on day D. These are the target variables you need to predict as {id}{1-60}.
* Ret_PlusOne: this is the return from the time Ret_180 is measured on day D to the close of trading on day D+1. (approximately 1 day). * This is a target variable we need to predict as {id}_61.
* Ret_PlusTwo: this is the return from the close of trading on day D+1 to the close of trading on day D+2 (i.e. 1 day) This is a target variable you need to predict as {id}_62.
* Weight_Intraday: weight used to evaluate intraday return predictions Ret 121 to 180.
* Weight_Daily: weight used to evaluate daily return predictions (Ret_PlusOne and Ret_PlusTwo).

## [Data Analysis](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Code/Data%20Analysis.ipynb)
Because the datasets are complicated, we make some basic analysis for it, which can help us better understand data itself. 

Firstly, Ret_2 - Ret_120 are return per minute at Day D(intraday). However, the aggregative return at 120 minute is more useful. Hence, we sum these data and calculate standard deviation. We name them by R_Agg,	R_Agg_Std	and R_Std respectively.

Secondly, we can find that some data are integer and some others are decimal from the ```.csv``` files. Apart from these, some data are missing. In order to better use these data, we will try to understand natures of these features. The part of the tabel is the analysis results.(You can see the original tabel in [```Data Analysis.ipynb``` ](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Code/Data%20Analysis.ipynb) )

![data_analysis](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Images/data%20analysis.jpg)

**Analysis Label Description:** 
* Missing and Missing % represent the number and percentage of missing data.
* Unique and Unique % represent the number and percentage of unique numbers, which can help determine the category features.
* Special and Special % represent the number and percentage of the most unique numbers, which can help determine the category features.
* Singular and Singular % represent the number and percentage of singular values.

**Summary of data:**
After some simple analysis, we can divide the features to two labels -- numerical features and category features.
Classification | Specific Features or Index
:------:  |:----------------------------------------------------------------------------------------:
Numerical Features | 'Feature_2', 'Feature_3', 'Feature_4', 'Feature_6','Feature_11', 'Feature_14', 'Feature_17', 'Feature_18', 'Feature_19','Feature_21', 'Feature_22', 'Feature_23', 'Feature_24','Feature_25','Ret_MinusTwo', 'Ret_MinusOne', 'R_Agg', 'R_Agg_Std','R_Std' 
Category Features |  'Feature_1', 'Feature_5', 'Feature_7', 'Feature_8', 'Feature_9', 'Feature_10','Feature_12', 'Feature_13','Feature_15', 'Feature_16', 'Feature_20' 

## [Data Preprocessing](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Code/Data_Proprocessing.ipynb)

In the data preprocessing part, we did the following transformation in order to get better result:

- Fill in the missing value

  Since we do not know what is the actual meaning of each feature, we chose to use constants to fill in all missing values, which is a simple and conservative option.


- Nomalize the numerical variables

  For numerical variables, we try to solve the distribution problem by appling the ﻿QuantileTransformer function to each of them, which is a quite robust transformer and it can also reduce our concern about outliers. 

  In the beginning, we are considering to apply PCA method to the numerical variables, just as what we learned in the class. But we find that we can get a much higher prediction accuracy without this step. 

- Apply PCA method to categorical variables

  We apply PCA method to the categorical variables. After PCA, we find that the first principle component can explain most of the variance. And it is much larger than other variables.

  After normaling the first principle component, we find it's value becomes 1 or -1, which seperate the sample into two categories.This variable is very important in our model, and can imporve the model performance significantly.

   
## [Visual Data Analysis](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Code/Visual%20Data%20Analysis.ipynb)
In this part, we will try to get a better understanding of the properties of different features and their relation to the targets by plotting:
* the features' correlations
* the distributions of the features
* regression plots for the features.

Through the distributions graph, most of the features don't have a normal distribution. 

By the correlation heatmap and the regression plots, we could see the evidence of correlations both within features and between features and targets. 

![heatmap](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Images/heatmap.png)

Moreover, some features have higher correlations which present a pattern of clusters, which are:
* [Feature_3 - Feature_11] & [Feature_3 - Feature_11]
* [Feature_3 - Feature_11] & [Feature_17 - Feature_25]
* [Feature_17 - Feature_25] & [Feature_17 - Feature_25]

**Visual Data Analysis After Processing:**

In the part of data Processing, we deal with the issues above, normalizing the features and eliminating the correlation between features and targets.

![RegressionPoints](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Images/RegressionPoints.png)

## [Modelling and Results](https://github.com/Mingkai-Zhuang/PHBS_MLF_2019/blob/master/Course%20Project/Code/Modelling.ipynb)

We will focus on LinearSVR, as this method seems to deals particullary well with data with a very low Signal-to-noise ratio as one would expect from financial data with high frequency. It is also a very fast algorithm with liblinear. For comparison purposes, we also run a LinearRegression model. We will do a grid search in the 2 key parameters, epsilon and C, which is related to the tube's width and regularization.

To be consistent with the competition's requirement, we use weighted mean absolute error (WMAE) to evaluate the model performance. It's calculated by firstly take the difference of the predicted value and given true value, then take weighted average with prediction weight given by the competition dataset. Models with lower WMAE is better.

Additionally, the baseline model mentioned below is just simply take the average of the given values to predict future values. Once we get a result with lower WMAE than the baseline model, we can say that we have exploit something useful to predict future returns.

Here are the learning results:
RESULT| Baseline | LinearRegression | LinearSVR
:---------:|:---------:|:---------:|:----------:
WMAE|27837.4369|27817.3544|27812.5624
Parameter|NA|Default|`{'C':0.0001, 'epsilon':0.0, 'loss': 'epsilon_incensitive'}`

We observe a much smaller error, i.e. the LinearSVR model performs better than the others.

## Improvements and Notes

We would like to briefly touch on a few possible improvements to potentially increase model performance:
- Ensemble. We did not investigate ensemble models. This is one of the usual ways to improve final models performance.
- Features interaction. We should consider a more selective feature interaction to improve model performance. Since we don't really know what the features are, this is difficult to achieve.
- Categorical features. We suspect that there may be some features that are ordinal while the others are nominal, but it is hard to auto-identify them by only the numbers and distribution without knowing what the feature is.

Overall, our model has exploited something useful to predict future returns, for there's lower error in our chosen model, but it is not closely related to any trading metrics (such as Sharpe ratio), which means we will need to consider more things for it to become a profitable trading strategy (such as maximum shortfall and market friction).

