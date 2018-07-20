---
layout: post
title: "Bayes theorem"
categories: DeeplearningbookNote
---

In a test experiment , there are also probability to wrong . For example , it is a **True** event , but after test the result is **negative** .  
From above , we can get a table named as confusion matrix .  

|--
| real result ><br>test result <br>^| True(cancer)(1%) | False(no cancer)(99%) |
|:-:|:-:|:-:|
| positive| 90%(1) | 5%(2) |
| nagative| 10%(3) | 95%(4) |

### example  

We make use of the above example , it says that , we have a sample where the cancer people occupy 1% , and the other who is no cancer is 99% . In %1 people , there are 90% test positive suffering from cancer , but 10% are not . (so it would be something wrong in this test , but we will repect the result of experments .) In the simarly way ,there are 5% people who isn't suffering from cancer but the test result is show that they are .  Here , how we calculate the real cancer probiblity in this test , still 1% , or maybe some compound of 90% with 5% ?
From we can conclude : 
+ The test result can be wrong also , we can't only believe in test result . 
+ However , We can make use of the piror knowledge about the test and combine with this test reuslt to get the best result .The piror knowledge in this example is we know 1% is suffer from cancer and the other is not . 


We rewrite the table above .

|--
| real result ><br>test result <br>^| True(cancer)(1%) | False(no cancer)(99%) |
|:-:|:-:|:-:|
| positive (have cancer)| 90%(1)<br>TF probability:<br>1%*90% = 0.9% | 5%(2)<br>FP prob:<br>99%*5% = 4.95% |
| negative(not have cancer)| 10%(3)<br> TN prob:<br>1%*10% = 0.1% | 95%(4)<br>FN prob:<br>99%*95% = 94.05% |

(1) is called TP probability. The test result is positive and the real result is Ture .  
(2) is called FP probability. The test result is negative but the real result is False .
The summary of (1) and (2) is the probability of all test positive , it is no relationship with True probability that is the prori knowledge .
And in the silmilar way with (3) , (4) . 

Now we get the real cancer probability :

$$
\begin{align*}
Pr(real\quad cancer \quad after \quad test) & = \frac{Pr(test \quad positive \quad and \quad have \quad cancer\quad before\quad test) }{Pr(test\quad positive\quad in\quad test )} \\
& =  \frac{0.9\%}{0.9\%+4.95\%} \\
& =  15.38 \%
\end{align*}
$$

Insteresting , a positive mammogram only means somebody has a $15.38 \%$ chance really suffer from cancer , not $90\%$ , it is refute common sense . The reason is that there are also have some FP probability , a large people without cancer and misjudgement that they suffer from cancer .

### formula 
we can turn the process anove into a equation ,suppose event $A$ is someone suffering from cancer before test , $1\%$ , and $X$ is someone viewed as suffering from cancer after test .  
The formula is :

$$
\begin{align*}
Pr(A|X) & = \frac{90\%\times1\%}{90\%\times1\%+99\%\times5\%}  \\
& = \frac{Pr(X|A)Pr(A)}{Pr(X|A)Pr(A)+Pr(X|\bar{A})Pr(\bar{A})}
\end{align*}
$$

### End
Bayes told us that test experiment maybe wrong sometimes , we should make use of the prior belief to consider the experiment results .

