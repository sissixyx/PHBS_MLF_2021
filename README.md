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
