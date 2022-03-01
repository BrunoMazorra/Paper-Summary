# SCRF: Template - Research Summary

---
title: [Research Summary - DO NOT RUG ON ME: ZERO-DIMENSIONAL SCAM DETECTION]
description: One or two sentences of what the post is about.
tags: [DeFi, Machine learning, Scam Detection,Rug pulls]
created: [2020-12-25]
researcher: [Bruno Mazorra and Victor Adan]
---

> ### TLDR
>
> - The reaserchers expand the dataset of Uniswap v2 scam tokens.
> - They provide  a theoretical classification of three different types of rug pulls and provide tools to identify them.
> - The authors introduce two highly accurate and precise Machine learning-based models to discriminate between malicious and nonmalicious tokens in different scenarios before the malicious manuever.

### Core Research Question

Do rug pulls in Constant Function Marker Makers (CFMM) share similar features? Can we predict if a project is a rug pull before the malicius manuver? 

### Citation

- Mazorra, Bruno, Victor Adan, and Vanesa Daza. "Do not rug on me: Zero-dimensional Scam Detection." arXiv preprint arXiv:2201.07220 (2022).

### Background

-

### Summary

1. Introduction.
2. Related Work.
3. Preliminars
    - Backround.
4. Malicious Uniswap Maneuvers
    - Classification of different type of rug pulls.
5. Data Collection:
    - Overview of the method used to extract all the necessary data.
6. Token Labelling.
    - Provide the methodology to label tokens as scams or non-scams.
    - Overview of the results obtained by the labelling methodology proposed.
7. 


### Method

- Labelling: First, we defined the maximum drop and the recovery of token prices and liquidity time series. The maximum drop (MD) is the largest drop from a peak to a trough. In our context, the maximum drop measures fall in the price or liquidity of the Uniswap listed pools. The recovery (R) is the largest pump from the bottom. To ensure that these fluctuations are not due to simple market movement, we computed the time elapsed since the last movement of the pool or token transfer to 13/09/2021. If more than one month has passed between the last movement or transaction of the token so far, we consider that the token is inactive. 
On the other hand, unlike malicious tokens, non-malicious tokens cannot be chosen from a liquidity, price, and activity analysis. Given a token, it may be considered malicious if there has been at least one rug pull at some point in its activity. However, a token that has not had any rug pull cannot be considered non-malicious, since it could experience a rug pull later on. Therefore, we take advantage of audits carried out by external companies (Certik, Quantstamp, Hacken...). The final list contains 27,588 labelled tokens, 631 labelled as non-malicious tokens and 26,957 labelled as malicious. Within the malicious, 24,870 are fast rug pulls and 2,087 do not contain LP Burn events.

<p align="center">
<img src="drawdown.png " width="40%" />
</p>

- 

We defined two methods that use Machine Learning models to discriminate between malicious and non-malicious tokens in different scenarios. In the first scenario, tokens can be evaluated at any block prior to the malicious maneuver. In the second scenario, all tokens are evaluated at a certain time after the creation of their respective pools.


<!-- Specifically, we use a new Machine Learning algorithm based on attention mechanisms for tabular data called FT-Transformer. -->
<!-- 
Describe the general approach of the paper (e.g scientific experiment, data/modeling, mathematical proofs) then outline the key steps that the researchers undertook to perform the study.

For each key step, summarize the nature of the specific approach that the researchers used to complete each step (e.g. statistical test, logic proof, data-driven model). -->

### Results

As explained above, most tokens are labelled as malicious. This implies that the final dataset will be highly unbalanced. Indeed, it would be enough to label all of them as malicious to achieve an accuracy of 97,7%. Thus, we labelled the non-malicious tokens as one and the malicious tokens as zero and tried to increase the performance in predicting non-malicious tokens. To validate both methods we used 5-fold cross-validation, therefore all the results will be presented as the mean and standard deviation of all folds.


#### Activity based Method Results

<p align="center">
<img src="Results1 method.png" width="60%" />
</p>

Both XGBoost and FT-Transformer get high metrics for accuracy, recall, precision, and F1-Score. However, XGBoost outperforms FT-Transformer in all metrics. 
In particular, XGBoost obtains an accuracy of 0.9936, recall of 0.9540 and precision of 0.9838 in distinguishing non-malicious tokens from scams. In contrast, FT-Transformer gets an accuracy of 0.9890, recall of 0.9180 and precision of 0.9752. 
Therefore, from now we will only analyse on XBoost results. 

<p align="center">
<img src="shap_values.png" width="60%" />
</p>
    
The figure above shows the feature importance in terms of SHAP value on the left side, and, on the right, the impact on the final output.
Most malicious tokens die in the first 24 hours after the pool is created; by contrast, non-malicious tokens have longer lives. This explains why features such as number of transactions or number of unique addresses have so much weight in the model. 
Also, noticed that less block difference between token and pool creation implies negative SHAP values, and negative SHAP values should correspond to malicious tokens. This conclusion coincides with (cite) since several of the malicious tokens take advantage of social trends by copying the name of official tokens and taking money from investors who get confused. This technique implies speed in the creation of the token and the pool since otherwise, the trend may be lost.

#### 24 Early Method Results

The results of the second method must be understood from another perspective since the problem posed is not the same. The difference with respect to the first method lies in the fact that we evaluate all the tokens at a particular time after the creation of their respective pools. For each labelled token, we have computed its features in each of the 24 hours after its pool creation. Note that, in this case, we are training both models for each hour. Therefore, we only have one evaluation point for each token.

<p align="center">
<img src="metric_evolution.png" width="60%" />
</p>
    
In general, XGBoost gets better metrics, except precision in some cases. Also, metrics of the first hour are lower than those of the last.
Our algorithm obtains a very high accuracy even in the first hours. However, the precision, recall and f1-score are lower than in Activity based Method. In the best of cases, i.e. 20 hours after the creation of the pool, our best algorithm obtains a recall of 0.789. This could indicate that while malicious tokens are easily detectable in the first few hours, non-malicious tokens require more time.

#### Unicrypt Results

First, from our list of labelled tokens, 745 use Unicrypt, 725 are labelled as malicious and 20 as non-malicious.
Then, from the unlabelled tokens, we compute their features up to the present time and use the Activity based Method with XGBoost algorithm to evaluate them. Based on these predictions, 2544 of non-labelled tokens using Unicrypt, 2211 are predicted to be malicious and 333 non-malicious.

### Discussion and Key Takeaways

<!-- Summarize any discussion/commentary sections included by the researchers. Identify the most concise description of any perspectives or viewpoints the researchers explored in this section.

Summarize the conclusions or key takeaways as included by the researchers. -->

### Implications and Follow-ups

<!-- Summarize the implicit or explicit implications of the study. If the researchers include such a section, include a few of the most important points discussed. If not, insert some commentary of clear-cut or obvious implications of the work.

Share any follow-up work discussed by the researchers.
 -->
 
 
### Applicability

- To better understand the methodology used by scammers to execute rug pulls.
- Provide machine learning tools to protect users from permanet losses. 
