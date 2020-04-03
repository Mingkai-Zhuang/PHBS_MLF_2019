# Predict Stock Returns

## Team Member

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

## Data
### Data Download
The datasets downloaded from [Kaggle-The Winton Stock Market Challenge](https://www.kaggle.com/c/the-winton-stock-market-challenge/data)

### Data Files Descrition
* train.csv - the training set, including the columns of:
  * Feature_1 - Feature_25
  * Ret_MinusTwo, Ret_MinusOne
  * Ret_2 - Ret_120
  * Ret_121 - Ret_180: target variables
  * Ret_PlusOne, Ret_PlusTwo: target variables
  * Weight_Intraday, Weight_Daily
* test.csv - the test set, including the columns of:
  * Feature_1 - Feature_25
  * Ret_MinusTwo, Ret_MinusOne
  * Ret_2 - Ret_120

### Specific Label
* Feature_1 to Feature_25: different features relevant to prediction
* Ret_MinusTwo: this is the return from the close of trading on day D-2 to the close of trading on day D-1 (i.e. 1 day)
* Ret_MinusOne: this is the return from the close of trading on day D-1 to the point at which the intraday returns start on day D (approximately 1/2 day)
* Ret_2 to Ret_120: these are returns over approximately one minute on day D. Ret_2 is the return between t=1 and t=2.
* Ret_121 to Ret180: intraday returns over approximately one minute on day D. These are the target variables you need to predict as {id}{1-60}.
* Ret_PlusOne: this is the return from the time Ret_180 is measured on day D to the close of trading on day D+1. (approximately 1 day). * This is a target variable you need to predict as {id}_61.
* Ret_PlusTwo: this is the return from the close of trading on day D+1 to the close of trading on day D+2 (i.e. 1 day) This is a target variable you need to predict as {id}_62.
* Weight_Intraday: weight used to evaluate intraday return predictions Ret 121 to 180
* Weight_Daily: weight used to evaluate daily return predictions (Ret_PlusOne and Ret_PlusTwo).
