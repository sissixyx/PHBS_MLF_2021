# Predicting Rebar Futures Price with Low-mid Frequency Data
## Team Member
Student Number | Github ID
------------ | -------------
1901212544 | [zxc19960706](https://github.com/zxc19960706)
1901213243 | [sissixyx](https://github.com/sissixyx)

For all the underlying code, please refer to [this](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/final%20project.ipynb).
## Project Introduction
Reinforcing bar, or rebar, is a common steel bar that is hot rolled and is used widely in the construction industry, especially for concrete reinforcement. Steel rebar is most used as a tensioning device to reinforce concrete and other masonry structures to help hold the concrete in a compressed state. Since it is widely used in the construction, the price of the rebar is an important indicator of the economy health, and it is also commonly traded in the commodity market. The purpose of the project is to estimate the price trend of the rebar futures based on some fundamental data so that we can make the trading strategies accordingly in a low-mid frequency.
## Data Description
### Features Description
Abbr. | Features | Description
------------ | ------------- | -------------
Inv_Steel | Steel Inventory | Reflects rebar demand
Inv_Iron | Iron Inventory | Reflects rebar demand 
GP_Rebar | Rebar Gross Profit  | 
Cost_Rebar | Rebar Cost	 | 
SP_Rebar | Rebar Spot Price | 
SP_Iron | Iron Spot Price | Represented by the import price
FP_FS | Ferrosilicon Futures Price | 
FP_Mn-Si | Mn-Si Futures Price | 
FP_Coke | Coke Futures Price | 
SP_Coke | Coke Sport Price | Represented by the coke index
FP_Iron | Iron Futures Price | 
CN1YR | 1 year China yield | 
US1YR | 1 year US yield | 
TV_Rebar | Rebar Futures Trading Volumes | Reflects the S-D relationship
HP_Rebar | Rebar Futures Position | Reflects the S-D relationship
FP_Rebar | Rebar Futures Price | …of last week
CV_Steel | Steel Consumption Volume | 
rCV_Steel | Real Steel Consumption Volume | 
OR_Steel | Steel Capacity Operating Rate | 
rCV_Iron | Real Iron Consumption Volume | 
OR_Iron | Iron Capacity Operating Rate  | 
CV_Iron | Iron Consumption Volume | 
rCV_PIron | Real Pig Iron Consumption Volume | 
CV_PIron| Pig Iron Consumption Volume | 
rCV_Rebar | Real Rebar Consumption Volume | 
CV_Rebar | Rebar Consumption Volume | 
M1 | Money Supply 1 | Includes physical currency, demand deposits, traveler's checks, and other checkable deposits
M2 | Money Supply 2 | Cash, checking deposits, and easily convertible near money
PPI | Producer Price Index | Measures the average change over time in the selling prices received by domestic producers for their output
CA_REDI | Completed Amount of Real Estate Development Investment | 
CA_FAI | Completed Amount of Fixed Asset Investment | 
HCA | Housing Construction Area | 
CSA | Construction Starts Area | 
CHSA | Commercial House Sales Area | 
PPI_I_yoy | Industrial PPI yoy Increase | 
PPI_I_mom | Industrial PPI mom Increase | 

**Note:** Data below Rebar Futures Price are all monthly data but we use it as the weekly data so that the data for the four weeks within the same month will all equal to the month's data.
There are in total 70 initial features (the list above only shows parts not including the wow change for the data that we will calculate and fit the model as the input). They can be devided into 5 categories. 
1. Raw materials: the features include the futures price, spot price, consumption volume, and operating rate of the raw materials including iron, ferrosilicon, manganese silicon (Mn-Si), coke, which will influence the output’s price.
2. Features of Rebar: The cost and gross profit of rebar reflects the cost from electricity and labor. The trading volume and position of the rebar futures reflects whether there is an abnormal change in the trading volume or the position, it suggests the price of the rebar will be volatile. 
3. Demand and supply data: The inventory level of the iron ore and the steel reflects the supply demand relationship. The iron ore is the supply of rebar, so its inventory reflects the demand of the rebar. If the inventory level of iron ore is high, the demand of the rebar is high and vise versa. The steel inventory reflects the demand of the rebar. If the steel inventory is low, it suggests the demand for the rebar is high. The downstream data such as the real-estate and fixed investment also reflects the demand of the rebar. 
4. Economic Fundamentals: The 1yr CN yield and1yr US yield reflects the market liquidity and low yield suggests a large liquidity which may drive the commodity price upward. The amount of M1 and M2 also reflects the liquidity of the market. PPI reflects the economy condition. 
5. Week-on-week change: we also calculate the week-on-week change of each parameter above and fit the model as the input. 
### Output Description
Based on the goal of the project, we want to use the model to predict the changes of the rebar price the next week. The models include both regression and classifier models. The output of in this project is the change of the rebar futures price (r,%). We then can make trading decisions according to the estimated changes. For the classfication model, the output is divided into three based on the rebar futures price. We set the threshold as 2%. Only when the changes are above the threshold, our corresponding trades are meaningful. If the change is above 2%, we will long the futures. If the change is below -2%, we will short the futures. If the change is between (-2%, 2%), we do not take any actions. The output of our model thus is transferred to the categorical output with three categories: 1, 0, -1. 1 means the change of the futures price next week is higher than 2% and we should long. 0 means the change of the futures price next week is between -2% and 2% and wo do nothing. -1 means the change of the futures price next week is lower than 2% and we should short. The active function is shown below.

![Image of Activation Function](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/Classifier%20Activate.png)

where r is the output, the change in price of the rebar futures price for the next week. 

For the regression model, the output is the rebar futures price return. 
## Data Preprocessing
### Null data filling
Our group fills the blanks using the linear interpolation.
### Price transformation
The next preprocessing step is to convert all initial data into wow return data and again input the result as the features.
### Dimension reduction
We used PCA method to reduce the dimensions. Specifically, we are concerned about the multicollinearity between the rebar cost, rebar gross profit, and raw materials’ prices. However, the results turn out that PCA did not significantly reduce the dimensions. Furthermore, the performance of the model after PCA applied is worse than without PCA. Therefore, we did not use PCA in the project. 

The other method we use is the Random Forest and we picked 47 features for the classfication models and 13 features for the regression models according to their importance reflected in the Gini Index.
### Data visualization
To better illustrate the relationship among the input data, our group visulize the data in the heatmap.The blue heatmap shows the correlations among all input data.

![Image of Correl_All](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/Correl_All.png)

The red heatmap of all features selected for the classfier models.
![Image of Correl_All](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/Correl_Classfier.png)

The green heatmap of all features selected for the regression models. 
![Image of Correl_Regression](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/Correl_Regression.png)

### Accuracy test 
The accuracy test is based on the evaluation metric. As the purpose of the model is to support our trading decisions. We care less when the result is 0 and put more focus on the long and short decision. We will calculate the following ratio to test the accuracy of the model:
- **TrueShortRate:** the rate that the model correctly predicts the short opportunity, supposed to be maximized
- **FalseShortRate:** the rate that the model predicts the short opportunity but it turns out not to be the case. Supposed to be minimized
- **CaughtShortRate:** the rate the short opportunity comes and the model catches the opportunity, supposed to be maximized
- **TrueLongRate:** the rate that the model correctly predicts the long opportunity, supposed to be maximized
- **FalseLongRate:** the rate that the model predicts the long opportunity but it turns out not to be the case, supposed to be minimized 
- **CaughtLongRate:** the rate that the long opportunity comes, and the model catches the opportunity, supposed to be maximized

Besides the above rates, we also use the sharpe ratio to determine the results for the classifier models and use the adjusted R-squared to test the regression model. We further use the models to predict 51 nearest weeks' returns to determine which model can earn the highest return.
### Cross validation 
In order to select the optimal parameters for the prediction models. We apply the cross validation and the details is shown below. 

```
 RF_score=float(sliding_window_score(100,30,Sharpe,Input,y,RF,30).mean(axis=0))
 ```
 For each validation test, we use 100 training samples (100 weeks' data), 30 test samples (30 consequent weeks' data), and move by 30 weeks every next validation test. 
 For the classifier models, the cross validation will optimal the sharpe ratio. For the regression models, it aims to optimize the R-squared. 
## Model Training and Predications
We first use the classifier models including logistic regression, support vector machine, decision tree, random forest, and GBDT. 
### Logistic Regression 
```
lr= LogisticRegression(penalty='l2',
                       solver = 'lbfgs',C=C,class_weight={0:0.2, 1:0.4,-1:0.4},multi_class='multinomial'
                       )
```
We use l2 regularization. The optimal parameter C is 0.2 according to the cross validation. Because we care more about the results that indicate we can take some trading actions, we give 1 (long) and -1 (short) more weights in the model and less to 0 (no action). The model is also set to be metaclassifier because we have three categories in the output.
### SVM
```
svm = SVC(kernel='rbf', random_state=0, C=C, gamma=gamma,decision_function_shape='ovr')
```
We use the RBF kernel function. The optimal parameters calculated from the cross validation are 1.1 for C and 0.01 for 𝛄.
### Decision Tree
```
tree = DecisionTreeClassifier(criterion='gini',max_depth=treedepth)
```
For the decision tree model, we use the gini criteria and set the maximum depth of the tree to 46 for optimization. 
### Random Forest
```
RF = RandomForestClassifier(n_estimators=RFn,random_state=0, oob_score=1,criterion='gini')
```
For the random forest, we set the number of trees to 10 as the optimation choice from the cross validation, and the criterion is also gini. We set the out-of-bag score as true, using out-of-bag samples to estimate the generalization accuracy.
### GradientBoostingClassifier
```
gbdt=GradientBoostingClassifier(n_estimators=gbdtT_n,learning_rate=gbdtT_l)
```
The optimal number of trees is 38 and the ptimal learning rate is 0.7.
The results of the 5 classifier models are shown in the following table. 
Model | Logistic Regression | SVM | Decision Tree | Random Forest | GBDT
------------ | -------------  | -------------  | -------------  | -------------  | ------------- 
**Optimal parameters** | C = 0.2 | C = 1.1 gamma =0.01 | depth = 46 | n_estimators = 10 | n = 38 eta = 0.7
**TrueShortRat** | 0.222222222 | nan | 0.15 | 0.25 | 0.216216216
**TrueShortRat** | 0.222222222 | nan | 0.4 | 0 | 0.297297297
**CaughtShortRate** | 0.2 | 0 | 0.3 | 0.1 | 0.8
**TrueLongRate** | nan | nan | 0.25 | nan | 0.25
**FalseLongRate** | nan | nan | 0.3 | nan | 0.5
**CaughtLongRate** | 0 | 0 | 0.277777778 | 0 | 0.055555556
**Best score** | 0.00255473 | 0.019878842 | 0.12518425 | 0.223713943 | 0.019047619
**Return** | 0.126534799 | 0 | -0.385444816 | 0.079374349 | -0.116343568

### Regression Models
The we continue on the regression models using the linear regression, support vector regression, decision tree, random forest, and GBDT. Here, we only show the results. The details of the process can be be checked [here](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/final%20project.ipynb).
**Model** | **Linear Regression** | **SVM** | **Decision Tree** | **Random Forest** | **GBDT**
------------ | -------------  | -------------  | -------------  | -------------  | ------------- 
**TrueShortRat** | 0.142857143 | 1 | 1 | 1 | 1
**TrueShortRat** | 0.380952381 | 0 | 0 | 0 | 0
**CaughtShortRate** | 0.3 | 0.1 | 0.2 | 0.2 | 1
**TrueLongRate** | 0 | 0.416666667 | nan | nan | 1
**FalseLongRate** | 0.375 | 0.083333333 | nan | nan | 0
**CaughtLongRate** | 0 | 0.277777778 | 0 | 0 | 1
**Return** | -0.282195759 | 0.359365558 | 0.246258158 | 0.246258158 | 2.449521635
**R-squared**** | -2.106368939 | -0.002044253 | -0.632814837 | -0.432200872 | -0.380031276

In addition, we drew the 51 nerest weeks predcition results.
![Image of Results](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/Results.png)
**Note:** The results of the Random Forest and GBDT in both classifier and regression can vary in different tests. 
## Conclusion
GBDT has bad performance in both classifier and regression models. Decision Trees, Random Forest, and SVM have bad performance in classifier models but relative better in the regression model. Therefore, we think **regression model suits our project better.** The linear regression shows a negative return which indicates the relationship between inputs and the output is not simply linear relationship. 

Another interesting findings is that for models with high positive results such as SVM regression, Random Forest regression, and Decision Tree regression, the number of deals exercised during the 51 weeks is small -  4, 3, 2 times respectively (for details of the testing results for the 51 weeks please refer to [here](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/result.xlsx). We guess, based on the fact, that the model captures find some unusual chances with high certainty and make profits. 

In the results for the regression models, some R-squred are negative. This is because we use the adjusted R-squared and the negative R-squared means there are too many number of features we use in the model. We can optimize that but since the the running time is too long to find the optimal number of the features. Therefore, we chose 13 features according to the importances and economics basics.  

We also conlude several reasons why the results of the models are not ideal. First, there are a lot of noises existing in the financial data, making it hard to detect the regular pattern. Second, some important information are obmitted as we did not further process the quantity and price data. Futher researches on the quantity and price index construction are necessary. We also think the time problems of the data we collected influence the model results. All the data of the features are weekly. Some of them are published earlier in the week and others later. However, the model treats all the data the same as published in the previous week which may make the prediction less accurate since some data are not most updated. We also use some monthly data and equaly distribute to each week within in the month. This is also another factor that could influence the accuracy of the result. 

