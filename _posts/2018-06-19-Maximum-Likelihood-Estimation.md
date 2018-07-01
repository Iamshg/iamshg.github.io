---
layout: post
title: "Deeplearningbook MLE(P132)"
categories: DeeplearningbookNote
---
# 当前是*草稿*，有不严谨之处
# The Note of Maximum Likelihood Estimation  
## aim :  
- Selecting right distribution('s type) of all data (given special distribution type);
- Make the right selection of parameters.  
At the first step , distribution type is selected by person using theory and priori knowledge of samples,but sometimes it would go wrong due to imature theory and wrong priori-knowledge.  
## way :  
After the work of selecting model distribution type done,next step is to learn concrete value of parameter of model distribution from sample.Maximux Likelihood Estimation is a way to do that.  
## Theory Basis :  
Given sample data $X = {x^1,x^2,\dots}$ , The Derivation is wrote below.  
\$$\Theta_{ML} = argmax_{\Theta}\sum_{i=1}^{m}\log{P_{model}(x^i,\theta)$$

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$


