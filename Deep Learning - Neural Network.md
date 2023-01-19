# Introduction

>In this study we are going to apply Linear Regression and Neural Network models in a famous dataset called Boston Housing Values in Suburbs of Boston, in order to compare the predictive capability of a Machine Learning vs a Deep Learning model.

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

## Linear Regression Model
### Function Concepts
> The goal of linear regression is to find the best-fitting line through the data points, it assumes that the relationship between the predictor variables and the outcome variable is linear, which means that the change in the outcome variable is proportional to the change in the predictor variables. 
#### y = f(x) 
#### y = a + b1*x1 + b2*x2...

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

a



![Image of Yaktocat](https://octodex.github.com/images/yaktocat.jpeg)
