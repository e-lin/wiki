---
layout: post
title:  "Forecasting Fine-Grained Air Quality Based on Big Data"
date:   2016-04-27 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->

Introduction
---
An interesting paper from KDD conference (2015).

Data sources:

- The air quality of current time and the past few hours
- The meteorological data of current time and past few hours
    * Humidity, temperature,..
    * Sunny, foggy, overcast, cloudy…
    * Minor rainy,  moderate rainy, heavily rainy, rain storm
    * Wind direction, wind speed
- Weather forecast

Models used in this paper:

- Temporal predictor: Multivariate Linear Regression
- Spatial predictor: Neural Network


Multivariate Linear Regression[*][R2]
---
Do not cofused with multiple linear regression. An example of how we distinguish difference below according to the number of variables:

- Simple linear regression: one y and one x. For example, suppose we wish to predict house price based on house size.
- Multiple linear regression: one y and serveral x's. We could attempt to improve our prediction of house price by using more than one independent variable, for example, house size, the number of bedroom, or the number of bathroom.
- Multivariate multiple linear regression: serveral y's and serveral x's. We may wish to predict serveral y's (such as the house price in last year and before the house price bubble burst\pop).

### Implement by using Tensorflow
Softmax regression (or [multinomial logistic regression][R4]) was introduced in Tensorflow's [MNIST For ML Beginners][R3] tutorial. However, here we would like to try to implement a multivariate multiple linear regression by using Tensorflow.

#### Simple linear regression: [(Code)][C1]

``` python
W = tf.Variable(0.0, name="weight")
b = tf.Variable(0.0, name="bias")

activation = tf.add(tf.mul(X, W), b)
cost = tf.reduce_sum(tf.pow((activation - Y), 2)) / (2*m)
optimizer = tf.train.GradientDescentOptimizer(FLAGS.learning_rate).minimize(cost)
```

Minimize cost function so that the sum of squared differences at each point is minimum.

If learning rate is too big, may not converge, if too small may converge slowly. At global minimum the partial derivative of Gradient descent formula is zero, hence the process may converge with fixed learning rate.


#### Multiple linear regression: [(Code)][C2]

For multiple features it works the same, except variables are vectors. *Feature scaling* is important to make the method converge faster. To do it subtract mean and devide by range (or standard deviation).

``` python
def feature_normalize(train_X):

    return (train_X - np.mean(train_X, axis=0)) / np.std(train_X, axis=0)
```

Change learning rate to optimize for convergence. Change by half an order of magnitude and observe how cost function changes in dependence on number of iterations.

I used [Portland housing prices][data_set] data set for confirming the implemetation was right.

The curve of the cost funciton looks like this:
![Curve](http://i.imgur.com/vvFXqe7.png)
with learning rate 0.1, 50 iterations and plot at every step.

And I wound up getting the same results as the [solution][data_set] suggested.

```
Training Cost= 2.04328e+09 W= [[ 109447.78125   ]
 [  -6578.36279297]] b= [ 340412.65625]

Predict.... (Predict a house with 1650 square feet and 3 bedrooms.)
House price(Y) = [[ 293081.46875]]
```

#### Normal Equations: [(Code)][C3]
We can also use the closed-form solution for linear regression.

``` python
# theta = (X'*X)^-1 * X' * y
theta = tf.matmul(tf.matmul(tf.matrix_inverse(tf.matmul(tf.transpose(X), X)), tf.transpose(X)), Y)
```

*Gradient descent:*

- Need to choose learning rate
- Need many iterations
- Works well for large n (switch to it for n > 10,000)

*Normal equation:*

- No need for learning rate and many iterations
- Feature scaling is not nescessary
- Need to compute (X' * X)^-1 which has O(n^3). Switch to gradient descent for large n.

#### Multivariate multiple linear regression: [(Code)][C4]


### Gain an insight into data
Period of data collection: 2014/05/01~2015/04/30

The dataset is comprised of six parts (with data number in total):

- city data: (43)
- district data: (380)
- air quality station data: (437)
- air quality data: (2,891,393)
- meteorological data: (1,898,453)
- weather forecast data: (910,576)

and were all provided in .cvs file format. Schema and example in each data are showed below:

#### City Data
Schema:<br />
City ID | Chinese Name | English Name | Latitude | Longitude | Cluster ID

Example:<br />
001,北京,BeiJing,39.904210,116.407394,1<br />
004,深圳,ShenZhen,22.543099,114.057868,2

#### District Data
Schema:<br />
District ID | Chinese Name | English Name | City ID

Example:<br />
00101,海淀区,HaiDianQu,001<br />
00102,石景山区,ShiJingShanQu,001

#### Air Quality Station  Data
Schema:<br />
Station ID | Chinese Name | English Name | Latitude | Longitude | District ID

Example:<br />
001001,海淀北部新区,HaiDianBeiBuXinQu,40.090679,116.173553,00101<br />
001003,石景山古城,ShiJingShanGuCheng,39.914409,116.184239,00102

#### Air Quality Data
Schema:<br />
Station ID | Time | PM25 | PM10 | NO2 | CO | O3 | SO2

Example:<br />
001001,2014-05-01 00:00:00,138,159.4,56.3,0.9,50.8,17.2<br />
001001,2014-05-01 01:00:00,124,163.9,38.7,0.9,51.1,17.9

Missing value is represented by NULL in the data files. AQI (air quality index) can be calculated based on [HJ633-2012][AQI].

#### Meteorological Data
Schema:<br />
ID | Time | Weather | Temperature | Pressure | Humidity | Wind Speed | Wind Direction

Example:<br />
001,2015-04-30 22:00:00,5,24.4,1006,50,3.5,13<br />
001,2015-04-30 23:00:00,1,20.5,1007,67,1.9,23

#### Weather Forecast Data
Schema:<br />
ID | Forecast Time | Future Time | Temporal Granularity | Weather | Up temperature | Bottom Temperature | Wind Level | Wind Direc

Example:<br />
001,2015-04-30 07:00:00,2015-04-30 08:00:00,3,1,28,21,3.5,3<br />
001,2015-04-30 18:00:00,2015-04-30 20:00:00,3,14,25,22,3.5,3

### Temporal predictor
The temporal predictor models the trend of air quality of a station based on four types of data:

- the AQIs of the past h hours at the station
- the local meteorology at the current time 𝑡𝑐
- time of day and day of the week
- the weather forecasts of the time interval we are going to predict.


Reference
---
[Forecasting Fine-Grained Air Quality Based on Big Data][R1]<br />
["Chapter 10, Multivariate regression – Section 10.1, Introduction", Methods of Multivariate Analysis][R2]<br />
[What is multinominal logistic regression model?][R4]<br />
[Use attribute and target matrices for TensorFlow Linear Regression Python][R5]<br />
[Multiple Linear Regression Model by using Tensorflow][R6]<br />
[Treating quantity as constant in TensorFlow][R7]<br />
[How to implement multivariate linear stochastic gradient descent algorithm in tensorflow?][R8]<br />


[R1]: http://research.microsoft.com/apps/pubs/?id=246398
[R2]: https://books.google.co.jp/books?id=0g-PAuKub3QC&pg=PA19&redir_esc=y#v=onepage&q&f=false
[R3]: https://www.tensorflow.org/versions/r0.8/tutorials/mnist/beginners/index.html#softmax-regressions
[R4]: https://en.wikipedia.org/wiki/Logistic_regression
[R5]: http://stackoverflow.com/questions/33698510/use-attribute-and-target-matrices-for-tensorflow-linear-regression-python
[R6]: http://stackoverflow.com/questions/37159070/multiple-linear-regression-model-by-using-tensorflow
[R7]: http://stackoverflow.com/questions/33858785/treating-quantity-as-constant-in-tensorflow
[R8]: http://stackoverflow.com/questions/36031324/how-to-implement-multivariate-linear-stochastic-gradient-descent-algorithm-in-te?rq=1
[C1]: https://github.com/e-lin/tensorflow_practices/blob/master/examples/linear_regression.py
[C2]: https://github.com/e-lin/tensorflow_practices/blob/master/examples/multiple_linear_regression.py
[C3]: https://github.com/e-lin/tensorflow_practices/blob/master/examples/multiple_linear_regression_closedform.py
[data_set]: http://openclassroom.stanford.edu/MainFolder/DocumentPage.php?course=MachineLearning&doc=exercises/ex3/ex3.html
[AQI]: http://kjs.mep.gov.cn/hjbhbz/bzwb/dqhjbh/jcgfffbz/201203/W020120410332725219541.pdf