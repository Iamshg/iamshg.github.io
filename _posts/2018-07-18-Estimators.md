---
layout: post
title: "Estimators "
categories: DeeplearningbookNote
---
We need not only to match train data , but also can generalize to test data , so we need to a way to estimate the parameter of the real distribution of data ( named as Estimator ), also , there need a way to evaluate the Estimator accordingly .We always  make use of Bias and Variance to evaluate the quality of Estimator .( the reason why we can use Bias and Variance to evaluate is that ,from the perspective of freqentists , the $\hat{\theta}$ is a random variable , fixed and unknown .)  
The  important and used most often is Point Estimatiors .  

From Wiki :  
> Point estimation involves the use of samplle data to calculate a single value (Known as point estimate or statistic) which is best guess of an unknown population parameter ( for example , the population mean) . 

Additionally , Point estimation can be contrasted with interval estimation ; such interval estimates are typically either confidece intervals in the case of frequentist inferene ,or credible intervals in the case of Bayesian inference.   
### Estimators
- Point estimation
  - Maximum likelihood estimator MLE
  - Minimum mean syared error MSE
  - Method of moments( 矩 ) and generalized method of moments 
  - ...
- Interval estimationa


### the aim of point estimation
Through sample data to predict a value of population parameter .  
### formula

$$
\begin{align*}
\hat{\theta} = g(X)\quad X = x^1,x^2,x^3,\cdots,x^n 
\end{align*}
$$

where $X$ is the sample data and $n$ is the number of elements of sample data .the reason why we can use Bias and Variance to evaluate is that ,from the perspective of freqentists , the $\hat{\theta}$ is a random variable , fixed and unknown . Random Variable has the property of Bias and Variance . So why the $\hat{\theta}$ is a random variable ? Because the sample data $X$ is random variable and the $\hat{\theta}$ is a value of function $g$ respect to $X$ . The function $g$ can be get from the MLE , MSE , and Method of moments and so on . Now we already have some point estimation from these ways , for example , sample mean , sample variance and so on .   

### Additionally  :
Bias and variance viewed as property of point estimates $\hat{\theta}$ . I think it also can be viewed as a standard of a Estimator . 
