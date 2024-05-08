<h1>Description
  
<h4>My submission for the Kaggle Competition "Digital Turbine's Auction Bid Price prediction".

Competition link: https://www.kaggle.com/competitions/digital-turbine-auction-bid-price-prediction/overview

<h1>Problem Statement  
<h4>User app interaction data is being received in real time. 

<h4>User is eligible to view an advertisement. Type and cost of that advertisement is determined via real-time-bidding.

<h4>Objective is to develop a predictive model using data from user’s device/interactions to forecast the Ad Bid Prices that maximize advertisement profit while still winning real-time auctions. 


<h1>Solution
<h4>LightGBM Model was built to utilize existing & additionally engineered features to achieve RMSE of 1.01.

<h4>Features were engineered after performing minimal Data Cleaning and EDA.

<h4>Model’s optimal parameters were determined using Optuna.


<h1>EDA
<h4>Dataset is rather clean NA-values-wise, with only ~700 NAs out of ~7kk training set rows. NAs have been filled-in using ‘Other’ / ‘Unknown’ values already existing in the dataset.


<h4>Dataset is imbalanced in several features, including unitDisplayType (80% of records belong to ‘banner’), bundleId, connectionType, size.


<h4>Half of winBids (target) is concentrated in $0 - $0.5 price range, while having ‘outlier’ values like $3k. 95% of values are concentrated in range $0 - $15.93, which was used as a cap value for remaining 5%.


<h4>Some features are correlated with higher winBids. For example unitDisplayType ‘banner’ delivers the lowest bids vs. remaining types.


<h4>Out of 4 anonymized features, two (c2, c4) are distributed quite evenly and seem to have no impact on the target.



<h1>Data Preparation & Feature Engineering

<h4>NA values replaced with corresponding ‘unknown’ values from the data. No sensible imputation approach derived from other features was found.


<h4>Timestamp utilized to create Year, Month, Day of the Week features.


<h4>Target capped at the value of 95% percentile.


<h4>Since training set contains multiple rows for the same user, history was used to produce lag features of 5 last winBids. Test set seems to follow the training set timestamp-wise, thus it received lag features calculated from the training set (previous user activity).


<h4>Dataset contains categorical features with very high cardinality (dozens of labels in device model, country etc). Therefore, those features need to be encoded. Considering the cardinality, one-hot encoding would yield a significant increase in feature count and memory usage. Therefore LGBM is used since it deals with categorical feature encoding in a more efficient way. If a different model was used, encoding / embedding would be required.


<h4>Features c2 and c4 seem to have no predictive value and can be dropped. However, since LGBM is used, feature selection will be handled by the model.


<h4>Since test set does not contain true values, test set was split into train and test to validate model performance before predicting final submission values.


<h1>Model Setup & Evaluation


<h4>Due to high cardinality of numerous categorical features, LGBMRegressor Model was selected.


<h4>Evaluation metric was chosen to be RMSE as per task requirement & accent on outliers. MAPE was utilized as a secondary metric to provide a relative scale of error.


<h4>Evaluation was performed utilizing a test set that was extracted and separated from original training set using the train_test_split.


<h4>Initial parameters were initialized as (num_leaves=31, learning_rate=0.05, n_estimators=100), yielding RMSE of 1.049.


<h4>Optuna was utilized to search for optimal hyperparameters, running for 200 trials and yielding the best parameter model with RMSE of 1.01.
