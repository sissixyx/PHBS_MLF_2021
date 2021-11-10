# Predicting Rebar Futures Price with Low-mid Frequency Data
## Team Member
Student Number | Github ID
------------ | -------------
1901212544 | [zxc19960706](https://github.com/zxc19960706)
1901213243 | [sissixyx](https://github.com/sissixyx)
## Project Introduction
Reinforcing bar, or rebar, is a common steel bar that is hot rolled and is used widely in the construction industry, especially for concrete reinforcement. Steel rebar is most used as a tensioning device to reinforce concrete and other masonry structures to help hold the concrete in a compressed state. Since it is widely used in the construction, the price of the rebar is an important indicator of the economy health, and it is also commonly traded in the commodity market. The purpose of the project is to estimate the price trend of the rebar futures based on some fundamental data so that we can make the trading strategies accordingly in a low-mid frequency.
## Data Description
### Features Description
There are in total 16 features used in the prediction model, which can be categorized as follows.
1. The futures price of the raw materials including iron, ferrosilicon, manganese silicon (Mn-Si), coke will influence the output’s price.
2. The spot price of the raw materials including iron and coke are represented by the import price of iron and coke index.
3. The cost and gross profit of rebar reflects the cost from electricity and labor. 
4. The inventory level of the iron ore and the steel reflects the supply demand relationship. The iron ore is the supply of rebar, so its inventory reflects the demand of the rebar. If the inventory level of iron ore is high, the demand of the rebar is low and vise versa. The steel inventory reflects the demand of the rebar. If the steel inventory is low, it suggests the demand for the rebar is high. 
5. The trading volume and position of the rebar futures. If there is an abnormal change in the trading volume or the position, it suggests the price of the rebar will be volatile. 
6. The 1yr CN yield and 1yr US yield. It reflects the market liquidity and low yield suggests a large liquidity which may drive the commodity price upward.
### Output Description
Based on the goal of the project, we want to use the model to predict the changes of the rebar price the next week. Therefore, the output in this project is the change of the rebar futures price (r,%). We then can make trading decisions according to the estimated changes. Here, we set the threshold as 2%. Only when the changes are above the threshold, our corresponding trades are meaningful. If the change is above 2%, we will long the futures. If the change is below -2%, we will short the futures. If the change is between (-2%, 2%), we do not take any actions. The output of our model thus is transferred to the categorical output with three categories: 1, 0, -1. 1 means the change of the futures price next week is higher than 2% and we should long. 0 means the change of the futures price next week is between -2% and 2% and wo do nothing. -1 means the change of the futures price next week is lower than 2% and we should short. The active function is shown below.

          ![Image of the Activation Function](https://github.com/sissixyx/PHBS_MLF_2021/blob/master/Final%20Project/Activation%20Function(1).png)

where r is the output, the change in price of the rebar futures price for the next week. 
To find the well-built model, our group add another regression model to predict the return of the rebar.
## Data Preprocessing
### Null data filling
Our group fills the blanks using the linear interpolation.
### Price transformation
All the price data are fitted into the model using the weekly growth ratio as well as their original data.
### Dimension reduction
We used PCA method to reduce the dimensions. Specifically, we are concerned about the multicollinearity between the rebar cost, rebar gross profit, and raw materials’ prices. However, the results turn out that PCA did not significantly reduce the dimensions. Furthermore, the performance of the model after PCA applied is worse than without PCA. Therefore, we did not use PCA in the project. 
**The other method we use is the Random Forest and we picked 40 features according to their importance reflected in the Gini Index.**
## Model Training and Predications
### Accuracy test 
The accuracy test is based on the evaluation metric. As the purpose of the model is to support our trading decisions. We care less when the result is 0 and put more focus on the long and short decision. We will calculate the following ratio to test the accuracy of the model:
- **TrueShortRate:** the rate that the model correctly predicts the short opportunity, supposed to be maximized
- **FalseShortRate:** the rate that the model predicts the short opportunity but it turns out not to be the case. Supposed to be minimized
- **CaughtShortRate:** the rate the short opportunity comes and the model catches the opportunity, supposed to be maximized
- **TrueLongRate:** the rate that the model correctly predicts the long opportunity, supposed to be maximized
- **FalseLongRate:** the rate that the model predicts the long opportunity but it turns out not to be the case, supposed to be minimized 
- **CaughtLongRate:** the rate that the long opportunity comes, and the model catches the opportunity, supposed to be maximized
### Cross validation 


### Logistic Regression 
**Note**: all parameters used in the following models have not been optimized yet. Our group will work on it using the cross validation later on. 
```
lr= LogisticRegression(penalty='l2',
                       solver = 'lbfgs',C=0.1,class_weight={0:0.2, 1:0.4,-1:0.4},multi_class='multinomial'
                       )
```
We use l2 regularization and set C at 0.1. Because we care more about the results that indicate we can take some trading actions, we give 1 (long) and -1 (short) more weights in the model and less to 0 (no action). The model is also set to be metaclassifier because we have three categories in the output.
### SVM
```
svm = SVC(kernel='linear', random_state=0, C=10, gamma=0.01,decision_function_shape='ovr')
```
We use the RBF kernel function and set the C at 10, 𝛄 at 0.01.
### Decision Tree
```
tree = DecisionTreeClassifier(criterion='gini',max_depth=5)
```
For the decision tree model, we use the gini criteria and set the maximum depth of the tree to 5. 
### Random Forest
```
rf = RandomForestClassifier(n_estimators=10,random_state=0, oob_score=1,criterion='gini')
```
For the random forest, we set the number of trees to 20, and the criterion is also gini. We set the out-of-bag score as true, using out-of-bag samples to estimate the generalization accuracy.
### GradientBoostingClassifier
We set 100 classifiers, and the learning rate η as 0.1
### Regression

## Conclusion

We also think the time problems of the data we collected influence the model results. All the data of the features are weekly. Some of them are published earlier in the week and others later. However, the model treats all the data the same as published in the previous week which may make the prediction less accurate since some data are not most updated. We also use some monthly data and equaly distribute to each week within in the month. This is also another factor that could influence the accuracy of the result. 
