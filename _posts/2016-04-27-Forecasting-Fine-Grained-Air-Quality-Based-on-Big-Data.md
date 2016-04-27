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


Multivariate Linear Regression
---
Do not cofused with multiple linear regression. An example of how we distinguish difference below according to the number of variables:

- Simple linear regression: one y and one x. For example, suppose we wish to predict house price based on house size.
- Multiple linear regression: one y and serveral x's. We could attempt to improve our prediction of house price by using more than one independent variable, for example, house size, the number of bedroom, or the number of bathroom.
- Multivariate multiple linear regression: serveral y's and serveral x's. We may wish to predict serveral y's (such as the house price in last year and before the house price bubble burst\pop).

### Implement by using Tensorflow
Softmax regression (or multinomial logistic regression) was introduced in Tensorflow's [MNIST For ML Beginners][R3] tutorial. Here we would like to try to implement a multivariate multiple linear regression by using Tensorflow.






Reference
---
[Forecasting Fine-Grained Air Quality Based on Big Data][R1]<br />
["Chapter 10, Multivariate regression – Section 10.1, Introduction", Methods of Multivariate Analysis][R2]

[R1]: http://research.microsoft.com/apps/pubs/?id=246398
[R2]: https://books.google.co.jp/books?id=0g-PAuKub3QC&pg=PA19&redir_esc=y#v=onepage&q&f=false
[R3]: https://www.tensorflow.org/versions/r0.8/tutorials/mnist/beginners/index.html#softmax-regressions
