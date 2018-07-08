---
layout: post
title: "Deeplearningbook MLE(P132)"
categories: DeeplearningbookNote
---
# The Note of Maximum Likelihood Estimation  

<h2> Aim : </h2> 

- Selecting right distribution('s type) of all data (given some special distribution types);
- Make the right selection of parameters.  
At the first step , distribution type is selected by person using theory and priori knowledge of samples,but sometimes it would go wrong due to imature theory and wrong priori-knowledge.  

## Way :  
  
After the work of selecting model distribution type done,next step is to learn concrete value of parameter of model distribution from sample.Maximux Likelihood Estimation is a way to do that.  

## Theory Basis :  

Given sample data $X = {x^{(1)},x^{(2)},\cdots}$ , The Derivation is wrote below.

$$
\begin{align*}
\Theta_{ML} & = argmax_{\Theta}\sum_{i=1}^m \log_{P_{model}}{(x^{(i)},\theta)} \\
            & = \frac{1}{m} argmax_{\Theta}\sum_{i=1}^m \log_{P_{model}}{(x^{(i)},\theta)}\\
            & = \frac{1}{m} argmax_{\Theta}\sum_x \log_{P_{model}}{(x,\theta)}\times\#x\\
            & = argmax_{\Theta}\sum_x \log_{P_{model}}{(x,\theta)}\times\frac{\#x}{m}\\
            & = argmax_{\Theta} \mathbb{E}_{x\sim\hat{P}}\log_{P_{model}}\left(x,\theta\right)
\end{align*}
$$

$\hat{p}$ is empirical distribution of $x$.Where $#x$ is the number of x in sample set and the $x$ is different with $x^i$ . You can understand $x$ and $x^i$ in [this way](https://stats.stackexchange.com/a/320503/213737) :  
For example : $X = {1,2,3,1,2,5}$ and $x^{(1)}$ is the first element $1$ ,$x^{(2)}$ is the second element $2$ and so on . There will be $x^{(1)},\cdots,x^{(6)}$ six elements ,but the $x$ is can only take four value 1,2,3,5 ,which is four . $#1$ and $#2$ both are 2,and anothers are 1 .

