---
title: Final Prediction
author: Nilay Ersoy
date: '2024-11-03'
slug: final-prediction
categories: []
tags: []
---


# Final Prediction for the Presidential Election 2024


_This blog is part of an ongoing assignment for Gov 1347: Election Analytics, a course at Harvard College taught by Professor [Ryan Enos](https://www.ryandenos.com). Throughout the semester, we will explore historical election data and use it to forecast the outcome of the 2024 election._




### Forecasting the 2024 U.S. Presidential Election: Electoral College and Popular Vote Analysis

For this analysis, I combined two models to predict the outcome of the 2024 Presidential Election. The Linear Regression Model estimates each state’s Electoral College vote share, using a range of demographic and polling predictors to forecast whether a state will lean Democratic or Republican. The ARIMA(3) Model, on the other hand, forecasts popular vote trends over time, tracking weekly shifts in public sentiment on a state-by-state basis. By employing these complementary approaches, I created a forecast that accounts for both the static electoral structure and the evolving trends in voter preferences.




*Model Selection and Rationale:*


The Linear Regression Model was chosen for the Electoral College vote prediction due to its suitability for estimating binary outcomes (Republican or Democratic) based on stable state-level predictors. Each state’s electoral history, demographic profile, and current polling data were used as inputs, allowing the model to capture the factors that typically influence electoral behavior. Conversely, the ARIMA(3) Model captures the time-sensitive nature of popular vote data by focusing on trends and momentum in polling support. This model’s three-lag structure makes it particularly effective at adjusting predictions based on recent changes in public sentiment, an essential feature for a responsive popular vote forecast. Together, these models provide a nuanced picture of the election, balancing historical voting patterns with real-time shifts in voter opinion




### Coefficients, Model Weights, and Interpretation:

In the Linear Regression Model, each coefficient indicates the effect of a predictor for each state
`$$\text{state poll difference}(t) = 
 \mathbb{I}[\text{Dem_popvote}(\text{last election})] + \text{National poll difference}(t) + \text{GDP_Growth}(t-Q)$$ on a state’s electoral outcome. For instance, states with high coefficients on polling support demonstrate that even minor fluctuations in polling can influence the final electoral outcome, which is often the case in swing states. In contrast, states with more entrenched voting patterns tend to have smaller coefficients, reflecting their predictable leanings. The ARIMA Model’s weights on lagged terms reveal the impact of recent polling data on current support levels. This three-lag setup emphasizes the role of recent sentiment, helping the model adjust as public opinion shifts in response to campaign events. While the linear regression coefficients highlight static influences, the ARIMA weights capture the dynamic shifts in voter behavior over time.

 



### Validation and Performance Assessment:


The in-sample and out-of-sample validation of these models reveals their respective strengths and limitations. The Linear Regression Model performs well in-sample, with a strong fit to historical voting patterns. This suggests that it captures the consistent elements of electoral behavior effectively. Cross-validation further confirms its predictive power, especially in states with steady partisan tendencies. Out-of-sample validation demonstrates that the model generalizes well across most states, although states with high political volatility, like Arizona and Georgia, show wider confidence intervals due to the inherent uncertainty in their voting patterns. The ARIMA Model’s validation across states resulted in an average R-squared of about 0.647, indicating moderate accuracy in capturing trends. For instance, Pennsylvania’s R-squared of 0.43 suggests that the model performs reasonably well in stable states, while the negative R-squared in Georgia highlights its limitations in more volatile regions. This variation suggests that while the ARIMA model is effective in capturing general trends, it may struggle in states where public opinion is highly erratic.




### Uncertainty and Confidence Intervals:


To incorporate uncertainty into the forecasts, I calculated prediction intervals for both models. The Linear Regression Model’s intervals reflect the range of expected electoral votes for each state. In highly contested states such as Nevada and Georgia, these intervals are wider, indicating greater uncertainty in predicting the electoral outcome. The ARIMA Model’s confidence intervals account for the variability in polling trends, with states exhibiting high volatility showing broader intervals. This approach acknowledges the possibility of last-minute shifts in voter sentiment, particularly in battleground states. By including these confidence intervals, the forecast communicates both the likely outcomes and the uncertainty associated with each prediction, providing a more comprehensive view of the election dynamics.





```
##               State           Outcome ElectoralVotes
## 502         Alabama Red (Republicans)              9
## 369          Alaska Red (Republicans)              3
## 692         Arizona Red (Republicans)             11
## 288        Arkansas Red (Republicans)              6
## 582      California  Blue (Democrats)             54
## 5021       Colorado  Blue (Democrats)             10
## 463     Connecticut  Blue (Democrats)              7
## 359        Delaware  Blue (Democrats)              3
## 632         Florida Red (Republicans)              3
## 694         Georgia Red (Republicans)             30
## 160          Hawaii  Blue (Democrats)             16
## 262           Idaho Red (Republicans)              4
## 2881       Illinois  Blue (Democrats)              4
## 407         Indiana Red (Republicans)             19
## 5022           Iowa Red (Republicans)             11
## 5023         Kansas Red (Republicans)              6
## 298        Kentucky Red (Republicans)              6
## 264       Louisiana Red (Republicans)              8
## 490           Maine  Blue (Democrats)              8
## 471        Maryland  Blue (Democrats)              4
## 400   Massachusetts  Blue (Democrats)             10
## 698        Michigan Red (Republicans)             11
## 498       Minnesota  Blue (Democrats)             15
## 474     Mississippi Red (Republicans)             10
## 501        Missouri Red (Republicans)              6
## 5024        Montana Red (Republicans)             10
## 101        Nebraska Red (Republicans)              4
## 683          Nevada Red (Republicans)              5
## 529   New Hampshire  Blue (Democrats)              6
## 5025     New Jersey  Blue (Democrats)              4
## 429      New Mexico  Blue (Democrats)             14
## 531        New York  Blue (Democrats)              5
## 681  North Carolina Red (Republicans)             28
## 320    North Dakota Red (Republicans)             16
## 536            Ohio Red (Republicans)              3
## 440        Oklahoma Red (Republicans)             17
## 238          Oregon  Blue (Democrats)              7
## 6941   Pennsylvania Red (Republicans)              8
## 1011   Rhode Island  Blue (Democrats)             19
## 5026 South Carolina Red (Republicans)              4
## 1012   South Dakota Red (Republicans)              9
## 433       Tennessee Red (Republicans)              3
## 6321          Texas Red (Republicans)             11
## 5027           Utah Red (Republicans)             40
## 176         Vermont  Blue (Democrats)              6
## 519        Virginia  Blue (Democrats)              3
## 390      Washington  Blue (Democrats)             13
## 5028  West Virginia Red (Republicans)             12
## 6921      Wisconsin Red (Republicans)              4
## 1013        Wyoming Red (Republicans)             10
```

### Table Comparison and Turnout Analysis:


The results table generated by the Linear Regression Model displays each state’s predicted outcome (Democratic or Republican) along with the allocated electoral votes. In traditionally Republican states like Texas and Florida, the model forecasts a clear Republican win, with significant electoral votes contributing to the total. Democratic strongholds such as California and New York are similarly predicted to yield substantial electoral votes for the Democrats. Swing states, however, show varying outcomes that reflect the polling data’s influence. For instance, Arizona and Nevada are projected to lean Republican, indicating a shift in these previously competitive states, while Michigan, a traditionally Democratic state, shows potential for a narrow Republican lead, highlighting the close competition.

When comparing turnout trends across models, we can observe that the ARIMA Model, which captures popular vote dynamics, suggests a different picture in terms of voter enthusiasm and support shifts. For example, states like Michigan and Pennsylvania show early Democratic support that gradually declines, while Republican support gains momentum. This contrasts with the more binary results from the Electoral College model, where the outcome is ultimately simplified to a win or loss. The ARIMA Model reveals the underlying volatility and changes in support that might not be fully visible in the static electoral prediction. In states with close polling data, such as Arizona and Nevada, the ARIMA Model’s trends suggest fluctuating voter sentiment, which could lead to higher or lower turnout depending on campaign developments.

 

```
## [1] "Average R-squared across states: 1.48023509852585"
```

```
## [1] "Outcome of Forecast for Pennsylvania: 0.175901106576993"
```

```
## [1] "Outcome of Forecast for Georgia: -1.05804268439771"
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-2.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-3.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-4.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-5.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-6.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-7.png" width="672" />


### Graphical Representation of Predictions:


The predictions are illustrated through both tabular and graphical formats. The table, generated from my Linear Regression Model, provides a clear breakdown of expected electoral outcomes across states. Each state is marked red or blue, representing the Republican and Democratic predictions, respectively, and is accompanied by the number of electoral votes allocated to each state. This table is particularly useful for identifying partisan strongholds and potential swing states, showcasing where each candidate is likely to perform well.

In contrast, my ARIMA Model offer a more detailed view of popular vote trends in individual swing states. For instance, Michigan’s graph displays a Democratic lead early in the campaign season, followed by a shift towards Republican support as the election approaches. Arizona’s graph reveals a steady decline in Democratic support, suggesting a potential Republican gain. Meanwhile, Nevada’s graph shows significant fluctuations, reflecting the volatile polling trends in the state. Each graph includes a dashed line at zero, marking the threshold between Democratic and Republican support, and highlights how voter sentiment evolves over time.

Compared to my Linear Regression, the ARIMA shows a win for Harris in swing states such as Nevada, Wisconsin, and Michigan. These battleground states show a likelihood for Trump to win in my regression model, which is interesting to observe, as these states will determine the presidency. However, it’s important to note that my ARIMA model purely predicts the popularity of each candidate in those states, whereas my regression forecasts the electoral college vote. Pennsylvania, in particular, may be the state that decides if we’ll see the first female president in the United States or another term for Donald Trump. 

<img src="ElectoralCollege.png" width="938" />
(Image created by ABCNews & 538 Interactive Map)




### Final Reflections and Overall Forecast:


This approach combines two distinct but complementary models—the linear regression model for **_Electoral College vote predictions_** and the ARIMA model for **_popular vote trends_**—to offer a thorough yet nuanced forecast for the 2024 Presidential Election. **The Linear Regression Model** is designed to give a structured prediction for each state’s outcome in terms of electoral votes. By analyzing polling data, demographic factors, and historical voting patterns, this model assigns each state either to the Democratic or Republican candidate. Importantly, this model predicts definitive outcomes, meaning that each state is categorized as either Republican or Democratic with no "lean" or "toss-up" labels, which inherently assumes a level of certainty that might not reflect the fluid nature of voter preferences. This contrasts with approaches like those used by [FiveThirtyEight](https://www.270towin.com/maps/538-forecast-2024-presidential-election), which often include probabilities and consider states as "leaning" or "toss-up" if they show significant polling variability or close margins. By omitting such classifications, my predictions convey more certainty about which party will win each state, but this also means the actual outcome could differ, especially in highly competitive states.

The **ARIMA Model**, in contrast, offers a more fluid perspective on popular vote trends by tracking shifts in weekly polling data. This model’s focus on recent polling dynamics helps capture the momentum of voter sentiment over time, reflecting how public opinion can change in response to campaign events, news cycles, or other influences. However, because the ARIMA model is also based on weekly polling data, it’s particularly sensitive to recent trends, which means that any abrupt shifts in public sentiment close to the election could still alter the final outcome. While this model doesn’t predict Electoral College votes directly, it provides valuable insights into which states might see fluctuating support levels, hinting at possible areas of electoral volatility that the more rigid predictions of the linear regression model might not fully capture.

The absence of "lean" or "toss-up" states in my forecast means that I’ve produced a binary outcome for each state, a method that offers clarity but potentially oversimplifies the true variability of the election. States like Arizona, Georgia, and Pennsylvania, known for close polling, are categorized definitively here, but in reality, these are likely battlegrounds with considerable uncertainty which explains the 312 electoral college votes for Trump. For example, FiveThirtyEight’s model incorporates a range of possible outcomes for these states by calculating the probability of each party winning and allowing for states to be classified as "lean Democratic," "lean Republican," or "toss-up" depending on the degree of polling uncertainty. My approach, by predicting clear winners in each state, removes that probabilistic layer, leading to an outcome that could be less resilient to last-minute changes in voter sentiment.

In essence, while my both models provide a structured and responsive forecast by combining Electoral College predictions with real-time popular vote trends, it inherently involves some limitations due to its deterministic classification of states. This means the actual election outcome may vary, especially in competitive states, due to the lack of probabilistic shading that would indicate uncertainty.
