---
title: "Blog Post 5"
author: "Nilay Ersoy"
date: "2024-10-03"
output: pdf_document
categories: []
tags: []
slug: "blog-post-5"
---


*Demographic Impacts on the 2016 U.S. Election and Forecasting the 2024 Outcomes*


_This blog is part of an ongoing assignment for Gov 1347: Election Analytics, a course at Harvard College taught by Professor [Ryan Enos](https://www.ryandenos.com). Throughout the semester, we will explore historical election data and use it to forecast the outcome of the 2024 election._


*Introduction:*


In the world of political campaigning, utilizing demographic insights allows campaigns to pinpoint where and how to engage specific communities, adapting to trends like shifts in the racial composition of suburbs or changes in economic status. This strategic application of demographic data not only boosts voter turnout by ensuring relevance but also deepens the electorate's connection with candidates, showcasing the profound impact of demographics in shaping political landscapes.

By harnessing this detailed understanding of the electorate, political campaigns can engage in microtargeting—delivering tailored messages through the appropriate channels to resonate deeply with specific demographic groups. This strategy not only maximizes voter turnout but also fosters a sense of connection and understanding between political candidates and their constituents, proving that in the arena of political campaigning, demographics are much more than mere numbers—they are the key to the hearts of the voters.




*The 2016 Election: A Colorful Analysis of Electoral Votes*


The 2016 U.S. presidential election was a pivotal moment, reflecting profound demographic and political shifts across the country. It was big shock when the [Hillary Clinton lost the election](https://www.scientificamerican.com/article/why-polls-were-mostly-wrong/) against Donald Trump. The most interesting part of the polls was that it was mainly focusing on the votes already done via mail. No one expected that Trump's partisans would show up on the day of election and vote in favor. Especially, since Clinton build a certain narrative trying to prove the voters that Trump was the "bad guy", which later turned out to be her downfall. As it was not evident yet, that demographics can easily change due to certain values a party might be associated with, as well as, the difference in persona Trump brought with him in 2015. 

In the following graph, I will analyse using a color-coded map, where each state's color intensity corresponds to its number of electoral votes, reveals interesting insights into electoral distribution. Darker shades represent states with a higher number of electoral votes, such as California and Texas, crucial battlegrounds due to their substantial influence over the election's outcome. Lighter shades indicate states with fewer electoral votes, often overlooked yet collectively significant.


<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-1-1.png" width="672" />

In this visual depiction, states like California, Texas, and Florida stand out with their deeper hues, indicating electoral votes ranging from 29 to 55. This distribution is critical as candidates focus campaign efforts on states with higher electoral stakes, sometimes at the expense of those with fewer votes but significant strategic value in tight races. 




*Demographic Shifts in Battleground States:*


Florida and Georgia are states experiencing significant demographic shifts, particularly with growing Hispanic and Black communities. These changes have the potential to alter political dynamics, as these groups traditionally tend to vote Democratic. In Florida, the increase in diverse populations is not just limited to one or two cities but is spread across the state, including significant increases in areas like Miami-Dade and Orange counties. These areas are known for their higher Democratic turnout, which could lead to stronger performances for the Democratic Party in statewide elections.

Similarly, in Georgia, the expansion of the Atlanta metropolitan area and its increasing diversity is transforming the state's political landscape. As more Black and Hispanic residents become politically active, areas that were once solidly Republican are seeing more competitive races. The 2020 presidential election and subsequent Senate runoffs highlighted this shift, with Democrats making significant gains.

However, there is this certain phenomenon that can be observed also in areas with a high population in migrants where they no longer want [refugees or other foreigners coming to the U.S](https://www.sciencedirect.com/science/article/abs/pii/S0049089X20300570). We follow a shift towards the right political spectrum as there is a certain differentiation people with a migration background claim to have more rights or maybe more social integrity within the U.S to consider themselves worthy of living here than incoming refugees/migrants. This makes it even more difficult to determine the outcomesof the election 2024 due to demographics and voter habits changing rapidly.



*Transitioning to 2024: Predictive Modeling*


For the upcoming 2024 election, advanced statistical models harness past polling data and electoral college trends to forecast outcomes. By merging state-level poll data with electoral vote counts and applying time series analysis, we can predict the shifting political landscape. The 2016 demographics highlighted the critical role of suburban voters and minority groups, whose growing electoral influence could reshape future elections.


```
## Warning: package 'forecast' was built under R version 4.3.3
```

```
## Series: national_data$mean_diff 
## ARIMA(1,0,0) with non-zero mean 
## 
## Coefficients:
##          ar1     mean
##       0.9048  -1.0401
## s.e.  0.0692   1.5391
## 
## sigma^2 = 1.141:  log likelihood = -47.34
## AIC=100.67   AICc=101.53   BIC=105.07
## 
## Training set error measures:
##                      ME     RMSE       MAE      MPE     MAPE     MASE
## Training set -0.1479575 1.034248 0.7592952 3.360952 45.93609 1.006989
##                     ACF1
## Training set -0.02364378
```

```
##    Point Forecast     Lo 80      Hi 80     Lo 95     Hi 95
## 34      -2.392204 -4.238315 -0.5460926 -5.215588 0.4311798
```

```
## Time Series:
## Start = 34 
## End = 34 
## Frequency = 1 
## [1] Republicans
```
The ARIMA model output provides a detailed forecast for the 2024 election, indicating a strong likelihood that past voting trends will continue into the next election. The model shows a preference toward the Republicans, predicting they will lead over the Democrats by an average of about 1.0401 points. This forecast is backed up by statistical indicators such as standard errors and sigma values, which tell us how reliable these estimates are. The model's fit to past data is also deemed reasonably good, as suggested by the AIC and BIC scores.

Looking deeper into the forecast, the model specifically predicts that the Republicans will lead by 2.39204 points on average. The provided confidence intervals, which help us understand the range within which the actual outcome is likely to fall, further support this. The 80% confidence interval stretches from -4.238 to -0.546, and the 95% interval from -5.216 to 0.431. These ranges indicate a stronger Republican lead but also show a slim chance for a Democratic lead, according to the wider 95% interval. While these insights are crucial for campaign strategies, it's important to consider them as one part of a larger set of tools, especially given the unpredictable nature of political landscapes.




*Conclusion: Demographics and Campaign Strategy*


This predicted outcome demonstrate the power of demographic trends in shaping electoral results. As the U.S. becomes more diverse, particularly in states like Georgia, Texas, and Florida, both major parties will need to adapt their strategies to appeal to shifting populations.


Democrats should focus on urban and suburban areas with growing minority populations, emphasizing issues such as healthcare, immigration reform, and voting rights.
Republicans will likely concentrate their efforts on rural and white voters, as well as conservative-leaning suburban areas, by emphasizing economic growth, deregulation, and law and order.
Although, by this first prediction it claims that so far Republicans have a head start in the upcoming elections. The democratic party still runs close by to have a possibilty to win.

Ultimately, the 2024 election will likely come down to a few key swing states where demographic changes and voter turnout will determine the outcome. Understanding these dynamics will be crucial for both parties as they navigate the road to the White House.

