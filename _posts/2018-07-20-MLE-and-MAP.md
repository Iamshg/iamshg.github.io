---
layout: post
title: "MLE(P132) and MAP(P138)"
categories: DeeplearningbookNote
---
The relationship between MLE and MAP , you should refer to [MLE vs MAP: the connection between Maximum Likelihood and Maximum A Posteriori Estimation](https://wiseodd.github.io/techblog/2017/01/01/mle-vs-map/).

From the formula ,   

$$
\begin{align*}
\Theta_{MLE} & = argmax_{\theta}P(X|\theta) \\
& = argmax_{\theta}\sum_ilogP(x_i|\theta) \\
\Theta_{MAP} & = argmax_{\theta}\frac{P(X|\theta)P(\theta)}{P(X)} \\
& = argmax_{\theta}P(X|\theta)P(\theta) \\ 
& = argmax_{\theta}\sum_ilogP(x_i|\theta)P(\theta) \\
& = argmax_{\theta}\left(\left(\sum_ilog(x_i|\theta)\right)+logP(\theta)\right)
\end{align*}
$$

Comparing both MLE and MAP equation ,the only thing differs is the inclusion of prior $logP(\theta)$ in MAP ,otherwise ,they are indentical .It means that the likelihood is now weighted with some weight coming from the prior ,we can conclude it from last two equations .

Many reularized estimation strategies ,such as maximum likelihodd regularized weith weight decay ,can be interpreted as making the MAP approximation to bayesian inference .  

From deeplearningbook P(139)
>As an example , a linear regression model with a Faussian prior on the weights $\omega$ ,.if this prior is given by $\omega \sim \mathcal{N}(\omega;0,\frac{1}{\lambda^2}I^2)$ , then the log-prior term $logP(\theta)$ is proportional to the familiar $\lambda\omega^T\omega$ weight decay penalty ,puls a term that doest depend on $\omega$ and does not affect the learning process . MAP Bayesian inference with a Gaussia prior on the weights thus corresponds to weight decay .

对于MLE和MAP的区别,可以参考一下这个[中文博客](https://www.cnblogs.com/easoncheng/archive/2012/11/08/2760675.html).里面将先验概率的加入看成了是对于模型本身的一种概率分布的考虑.
