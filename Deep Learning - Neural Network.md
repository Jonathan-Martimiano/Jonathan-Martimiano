# Introduction

>In this study we are going to apply Decision Tree Regression and Neural Network models in a famous dataset called Boston Housing Values in Suburbs of Boston, in order to compare the predictive capability of a Machine Learning vs a Deep Learning model.

#### Importing libraries
> MASS -> Data analysis statiscal models \
> rpart -> Classification and regression decision trees \
> neuralnet -> Neural network models
```R
library("MASS")
library("rpart")
library("neuralnet")
```
#### Set.Seed(0) function for standardize random data

```R
set.seed(0)
```

#### Importing Boston Housing Values in Suburbs of Boston [R - Dataset]

```R
data <- Boston
head(data)
```
![Boston Housing Values in Surburbs of Boston](https://imgur.com/bful7SL.png)

> crim - per capita crime rate by town. \
zn - proportion of residential land zoned for lots over 25,000 sq.ft. \
indus - proportion of non-retail business acres per town. \
chas - Charles River dummy variable (= 1 if tract bounds river; 0 otherwise). \
nox - nitrogen oxides concentration (parts per 10 million). \
rm - average number of rooms per dwelling. \
age - proportion of owner-occupied units built prior to 1940. \
dis - weighted mean of distances to five Boston employment centres. \
rad - index of accessibility to radial highways. \
tax - full-value property-tax rate per \$10,000. \
ptratio - pupil-teacher ratio by town. \
black - 1000(Bk − 0.63)2 where Bk is the proportion of blacks by town. \
lstat - lower status of the population (percent). \
medv - median value of owner-occupied homes in \$1000s. 


#### Verifying Data Missing Values 

```{r setup, include=FALSE}
data[is.na(data) == TRUE]
```
>Zero Missing Values Identified \
![Missing Values Check](https://imgur.com/dWKwLgm.png)

## Decision Tree Regression Model
### Function Concepts
> The Regression Decision Tree focus is to find the best splits in the data to predict a continuous target variable, by partitioning the feature space into different regions that correspond to different values or ranges of values for the target variable. 

### Train-Test Split 
> 80% of the sample is going destinated to train the model and 20% for testing

```{r setup, include=FALSE}
train_test_split_index <- 0.8 * nrow(data)

train <- data.frame(data[1:train_test_split_index,])
test <- data.frame(data[(train_test_split_index+1): nrow(data),])
```

## CART
> Tree Classification and Regression, supervised learning algorithm used to build decision trees for classification and regression.

### Decision Tree
> `1st Line` Creates a regression decision tree using the 'rpart' package and assigning it to the variable "fit_tree". The dependent variable is "medv" and the independent variables are all other columns in the "train" dataset. The method used is "anova".

> `2nd Line` Using the "predict" function to make predictions using the "fit_tree" model on the test dataset "test". Predictions are stored in the "tree_predict" variable.

> `3rd Line` Calculates the Mean Squared Error (MSE) between the "tree_predict" predictions and the "test$medv" actual values using the "mean" function and stores the result in the "mse_tree" variable. MSE is the average of the squared differences between the predicted and actual values. The lower the value, the better the model.
```
fit_tree <- rpart(medv ~.,method="anova", data=train)
tree_predict <- predict(fit_tree,test)
mse_tree <- mean((tree_predict - test$medv)^2)
```
> The Regression Tree MSE is 46. \
![MSE_Tree](https://imgur.com/OQpHNXG.png)

## Neural NetWork
### Scaling Data
> It scales the data to a specific range, 0->1. This method works well for datasets that do not have outliers.
```
max_data <- apply(data, 2, max) 
min_data <- apply(data, 2, min)
scaled <- scale(data,center = min_data, scale = max_data - min_data)
```

### Training the Neural NetWork model
```
index = sample(1:nrow(data),round(0.70*nrow(data)))
train_data <- as.data.frame(scaled[index,])
test_data <- as.data.frame(scaled[-index,])
```
### Ploting the Neural NetWork
```
nn <- neuralnet(medv~crim+zn+indus+chas+nox+rm+age+dis+rad+tax+ptratio+black+lstat,data=train_data,hidden=c(5,4,3,2),linear.output=T)
plot(nn)
```
> Converter formato imagem do R para PNG
```
png("Regression Decision Tree.png", width = 800, height = 600)
plot(nn)
dev.off()
```
![Regression Decision Tree](https://imgur.com/Lfz8niT.png) 
> The Neural NetWork MSE is 9. \
![image](https://user-images.githubusercontent.com/76530436/213609477-06505ff8-39e6-48d2-b2ba-3a2c9b8f822f.png) 


> Analysis Completed: We can prove that the Neural NetWork MSE predicting the Boston's Houses Prices is lower than the Regression Decision Tree. 


