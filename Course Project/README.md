#

## Team Member

Student Name | GitHub ID | Student ID
:---------:  |:---------:|:---------:
Zhuang Mingkai| Mingkai-Zhuang  |1901212687
Zhou Chi|zaynmalivski | 1901212684
Wang Ke|KeWang-pku | 1901212641
Ni Ye|NiYee | 1901212623

## Data
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
