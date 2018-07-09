---
layout: post
title: "Deeplearningbook Linear Regression : From a single value to a distribution prediction"
categories: DeeplearningbookNote
---
### Linear Regression :

Linear Regression : to predict $y$ from $x$ by outputting a point $\hat{y}$.  

### a value prediction

Simply , the task of Linear Regression is to output a value given $x$ , so we take the MSE(mean square error) as cost funtion.

$$
\begin{align*}
MSE_{test} & = \frac{1}{m}\sum_i{(\hat{y}^{(test)}-y^{(test)})_i^2} \\
           & = \frac{1}{m}||\hat{y}^{(test)}-y^{(test)}||_2^2
\end{align*}
$$

In this example , the prediction of the model is a point , for example , $\hat{y}$ is $3.2$ . 

### a distribution  prediction 

Once have learnt MLE , we can get a distribution prediction of every $\hat{y}$ value . For example ,we can know the propability of value $3.2$ is $90.1\%$ and $3.1$ is $9\%$ ,so on . It is not only a value ,but a distribution of $\hat{y}$ . If we want to output a value only , we can select the value $\hat{y}$ of largest probability .
