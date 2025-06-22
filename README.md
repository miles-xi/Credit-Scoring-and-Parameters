# Credit-Scoring-and-Parameters

### Executive summary

In this project, we analyzed the Single-Family Loan-Level Dataset from the U.S. Freddie Mac. This extensive dataset consists of two data files, a mortgage application file (61,404 rows, 32 columns) and a repayment performance file (1,759,940 rows, 32 columns). Our first objective is to build a behavioral scorecard predicting one-year-ahead default probability for each observation.

We first gained a robust understanding of the data by reading the General User Guide [1]. The response variable is a binary indicator created based on the delinquency status in the next 12 months. If the observation window is shorter than 12 months, due to, for example, early repayment or reasons arising from default, the target value can still be determined. If there was neither enough observation nor relevant information, the target could not be determined and later the observation was removed.  Useful features such as the rate of change in unpaid balance were created. Then two datasets were merged and the joined data set was split into a training set, a test set, and an OOT sample. Finally, missing values in each feature were handled through imputation; if there were a substantial portion of missing values due to unknown reasons, that feature was deleted. Outliers were handled by removing the case or were left as is. The data cleaning procedures were done separately and consistently on the training and test sets.

We binned the remaining features using Weight of Evidence binning, creating a more meaningful interpretation for each feature and introducing non-linearity into the linear model. We used logistic regression with the ElasticNet regularization and trained the model with the 3-fold cross-validation to tune the hyperparameters. The model achieved an ROCAUC score of 0.940 – 0.944 at a 95% confidence level. Given the assumption that each loan could yield a profit margin of 30% of the interest or suffer 40% of the house price, with the predicted probability, we computed the total expected profits of the loans to determine the best cutoff for the loan approval decision-making.

The second task is to develop a time series model of the PD for ratings of loans that have distinct risk profiles from low to high. We obtained a series of breakpoints (which are the False Positive Rate) by approximating the ROC curve of the logistic regression model using piecewise linear functions and mapped these breakpoints to the probability cutoffs, resulting in eight ratings. With these ratings, we put observations in the test set into their corresponding bins and time slots, getting eight time series of the default probability for each state, which was modeled using the SARMIAX model where exogenous variables are macroeconomic factors including the monthly unemployment rate and the House Price Index (HPI) obtained from St Louis Fed. Finally, we obtained the long-run unemployment rate and estimated the HPI to forecast the loan’s performance in the OOT sample using the fitted time series model.

For defaulted loans, we derived their LGD using the features of loss calculation and unpaid balance in the dataset. We performed similar data cleaning procedure to the training, test, and OOT samples and then trained and fine-tuned an XGBoost model to predict LGDs. The model achieved an RMSE of 0.050 on the training and 0.068 on the test set. SHAP were employed to explain the feature importance. Finally, we used to model to predict the LGD on the whole OOT set and trimmed the predicted values so that they satisfied the Basel III floor of 5%.
 
We simulated recovery rates using a uniform distribution to derive EAD, and with PD and LGD obtained above, we calculated the provision for a portfolio of loans in the OOT sample. We found that the provision accounts for 0.1878% of the portfolio value and is not sensitive to the recovery rates. This is likely due to low long-run PDs and small and stable predicted LGDs in the sample.
![image](https://github.com/user-attachments/assets/3a723bb2-512d-4fd8-a778-09d845e44269)



### License and Usage Terms
This repository and its contents are © 2025 Miles Xi. All rights reserved.

You are welcome to: <br>
• View and use the code and materials directly via this GitHub repository <br>
• Use the code for educational or non-commercial personal projects, as long as you do so through GitHub

However, you may not: <br>
• Rehost, mirror, or redistribute this repository or any part of its content on third-party websites or platforms <br>
• Use any automated means (e.g., scraping tools or bots) to copy or mirror the repository or its contents elsewhere

Unauthorized redistribution or rehosting is strictly prohibited. Please contact me for permission if needed.
