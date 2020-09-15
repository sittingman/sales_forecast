# Forecast sales for a gaming retailer

### Problem:

* Given 34 months of sales history (Jan 2013 – Oct 2015) of a gaming retailer by items and by shops, predict sales for Nov 2015 (period 35)
* [Steps on obtaining data](https://github.com/sittingman/sales_forecast/blob/master/obtain_data.ipynb). First time users to Kaggle will need to set up API, refer to [document](https://www.kaggle.com/docs/api)

### Clients:
* **Leadership of the toy company**: need a statistic reference on company sales potential based on historical trend in order to predict opportunities/risks for business to hit company sales target.Based on the study, the company will priorities marketing/investment resources to maximize opportunities/mitigate risks.
* **Toy product developers**: Study may reflect meaningful category trend in which developer can prioritize resources on innovation in those categories
* **Competitors**: leverage the result to develop market/product strategy to tap into market potential
* **Data Scientists/Researchers**: If the model generates reasonable prediction, it can be scaled up to include more company’s data and to be used to predict total market share for the toy industry

### Outline of Approach

* [Data Cleansing/Wrangling](https://github.com/sittingman/sales_forecast/blob/master/clean_wrangling.ipynb): Understand the data structures, checking for missing values or invalid records
    
    * Do not observe missing data
    * Identified outliers on item price, applied [outlier](https://github.com/sittingman/sales_forecast/blob/master/outlier.ipynb) procedures

* [Exploratory](https://github.com/sittingman/sales_forecast/blob/master/exploratory.ipynb): Finding historical sales patterns and identify potential correlation factors that could serve as good training features
    
    * Identified seasonal sales pattern for major product categories
    * Top 7 categories (out of 84) contribute to ~90% of total sales
    * Store volumes varies, some stores did less than $1M per month, some stores did more than $10M
    * Day of week appear to influence daily sales, major holiday also play a role in sales peaks
    * Preliminary conclusion: seasonal pattern suggests applying Time Series models. Influential attributes such as product categories/shop imply the use of classification/regression models


* Statistical Test: evaluate the statistical significance of the features identified in the exploratory stage on predicting target (i.e. sales), narrow down features that matters most to the predictions
    * [Time Series Test](https://github.com/sittingman/sales_forecast/blob/master/ts_stattest.ipynb): identified strong lag of 7 days through auto-correlation graphs, data also passed Dickey-Fuller test that data do not follow random walk patter under 95% confidence level.
    * [Regression Test](https://github.com/sittingman/sales_forecast/blob/master/regtest.ipynb): confirmed that categorical features such as shop_id, item_category, and month would be strong features to use for regression/classification models. Same applies for item prices.
    

* Machine Learning: measure accuracy as well as root mean squared errors across 3-4 models to pick the winning models
    * Naïve model: Use Nov 2014 actual item count by shop to predict Nov 2015
    * [Time Series](https://github.com/sittingman/sales_forecast/blob/master/model_ts.ipynb) Models tested include:
        * ARMA, SARIMA, Holt Winter, Prophet, VARMAX
        * Training results indicates that none of the time series would give reasonably good result. Predictions errors could be as high as 50% off from target
        * Picked SARIMA to be submitted to Kaggle for accuracy scoring

    * [Regression/Classification](https://github.com/sittingman/sales_forecast/blob/master/model_cls.ipynb) Models tested include:
        * Xgboost, Random Forest, Rigde
        * Training results suggested that Xgboost has the highest accuracy, but the other model performance are within 1 rmse range
        * [Submit](https://github.com/sittingman/sales_forecast/blob/master/data_submit.ipynb) all three model for accuracy scoring

### Summary of Findings

|Model | Kaggle Score (RMSE) | Time |
|------| -------------| ---|
|Naive| 3.77| 1s |
|SARIMA| 12.40|1m 24s|
|Xgboost| 2.16| 1m 42s |
|Random Forest|1.40 | 25m 24s|
|Ridge|3.42| 3s |

### Recommendations/next steps

Random Forest has the best accuracy among all. From the execution standpoint, xgboost runs faster.

Next steps will be to include more relevant features at the shop level trend and product category trend to improve model performance. Yet, the trade off of getting more features may lead to overfit and more costly to run. Clients will need to decide what rmse level they would feel comfortable on making business decision.

It would helpful if Clients can provide information on company internal marketing efforts (e.g. markdown/promotional activities) which help generate other useful features which are absent in current model.

### [Capstone Report](https://github.com/sittingman/sales_forecast/blob/master/capstone_report.pdf)
### [Final Presentation](https://github.com/sittingman/sales_forecast/blob/master/sales_fcst_presentation.pdf)