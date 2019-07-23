# P2P-_Data-driven-Strategy
## Introduction
Peer-to-peer lending refers to the practice of lending money to individuals (or small businesses) via online services that match anonymous lenders with borrowers. Typically, lenders can earn higher returns relative to savings and investment products offered by banking institutions.

This dataset comes from LendingClub (and Prosper), which is publicly available online. It contains comprehensive information on all loans issued between 2007 and the third quarter of 2018. For this project, I used the combined dataset coming from NathanGeorge. (https://github.com/nateGeorge/preprocess_lending_club_data)

The data set includes 151 features. 113 of them are numerical features and 38 of them are categorical features. Below will show some features inside this data set.

1. Interest rate
2. Loan amount
3. Monthly installment amount
4. Loan status (e.g., fully paid, default, charged-off)
5. Several additional attributes related to the borrower such as type of house ownership, annual income, monthly FICO score, debt-to-income ratio, and number of open credit lines.

The data set used in this case study contains 2132287 loan listings.

For this project, the goal is to maximize return based on the return calculation method and the strategy chosen by investor.
There will be two statuses for on loan. If the payment is delayed by more than planned days, the loan is considered to be in “Default”. Otherwise it will be considered fully paid. We use classification methods to predict if a loan will be defaulted. Also, we need to predict the return rate of each loan based on the strategies for maximizing return.

## Data Exploratory Analysis
I selected some features to visualize the data. We began by exploring the distribution of ...
## Data Cleaning and Feature Selection
There are 151 features inside this data set, but some of them cannot be used as predictive variables because of the leakage problem. 
### leakage problem
A feature whose value would not actually be available in practice at the time we would want to use the model to make a prediction is considered a feature that can introduce leakage to our model— this is known as the Data Skeptic perspective.

Examples of leakage:

● Total payment: when investing in future loans, we cannot access to the total amount that would eventually be repaid on that loan.

● "grade", "int_rate", "installment": change occasionally after the loan is issued

If we do not exclude leakage features, our AUC (area under curve) is about 1, which is unreliable.

### AUC
This is again one of the popular metrics used in the industry.  
AUC - ROC curve is a performance measurement for classification problem at various thresholds settings. ROC is a probability curve and AUC represents degree or measure of separability. Higher the AUC, better the model is at distinguishing between patients with disease and no disease.

![Image of auc](https://github.com/xinyueniu/P2P-_Data-driven-Strategy/blob/master/AUC.png)

Finally, we selected 34 features, which is shown below.

![Image of selected](https://github.com/xinyueniu/P2P-_Data-driven-Strategy/blob/master/selected.png)

## Classification

We used three algorithm(Logistic Classification, Naive Bayes and Random Forest) to predict default for loans and finally choose Random Forest at an AUC with 0.68
## Regression

To predict return rate for each loan, we need to consider the return calculation method.
### return calculation methods(3)
Method 1 (M1—Pessimistic) supposes that, once the loan is paid back, the investor is forced to sit with the money without reinvesting it anywhere else until the term of the loan.

Method 2 (M2—Optimistic) supposes that, once the loan is paid back, the investor’s money is returned and the investor can immediately invest in another loan with exactly the same return.

Method 3 ( M3) considers a fixed time horizon (e.g., T months) and calculates the return on investing in a particular loan under the assumption that any revenues paid out from the loan are immediately reinvested at a yearly rate of i%, compounded monthly, until the T-month horizon is over (throughout this case study, we consider a 5-year horizon, i.e., T = 60).

We used five Algorithm methods to predict return rate for each loan by using calculation methods mentioned above. For M3, we give two i%, which are 1.2% and 3%. Here five Algorithm methods are Linear Regression, Ridge Regression, Lasso Regression, Random Forest and Neural Network.

![Image of regression](https://github.com/xinyueniu/P2P-_Data-driven-Strategy/blob/master/regression.png)

For this project, I chose M1—Pessimistic. Since Random Forest has the Highest R^2, we use
Neural Network for the next steps.
## Optimization
four Strategies that can be used
1. Random strategy (Rand)—randomly picking loans

2. Default-based strategy (Def)—using the random forest default-predictor models. Then, sorting loans by their estimated default probabilities, and selecting the loans with the lowest probabilities.

3. Simple return-based strategy (Ret)—training a simple regression model to predict the return on loans directly. Then, sorting loans by their predicted returns and selecting the loans with the highest predicted returns.

4. Default and return-based strategy (DefRet)—training two additional models—one to predict the return on loans that did not default, and one to predict the return on loans that did default. Then, using the probability of default to find the expected value of the return from each future loan. Invest in the loans with the highest expected returns.

![Image of stand](https://github.com/xinyueniu/P2P-_Data-driven-Strategy/blob/master/standby.png)

### Simple return-based strategy (Ret)+M1-Pessimistic

![Image of retm1](https://github.com/xinyueniu/P2P-_Data-driven-Strategy/blob/master/Ret%2BM1.png)

###  default and return-based strategy (DefRet)+M1-Pessimistic

● Maximizing the total expected profit
● 900<=Num_loans<= 1000
● Budget <=10000000

![Image of opt](https://github.com/xinyueniu/P2P-_Data-driven-Strategy/blob/master/defret%2Bm1.png)

Here we get the objective-return rate at 0.024955

the picture of last one and the r^2 problem.
