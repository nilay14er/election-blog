---
title: 'Post-Election Reflection'
author: Nilay Ersoy
date: '2024-11-13'
slug: post-election-reflection
categories: []
tags: []
---





# Post-Election Reflection on Model: A Mixed Success in Electoral Predictions


_This blog is part of an ongoing assignment for Gov 1347: Election Analytics, a course at Harvard College taught by Professor [Ryan Enos](https://www.ryandenos.com). Throughout the semester, we will explore historical election data and use it to forecast the outcome of the 2024 election._




### Recap of My Models and Predictions:

For the 2024 presidential election, I used two different models to make my forecasts: a Multi Linear Regression Model for predicting state-level Electoral College outcomes and an ARIMA(3) Model for tracking national popular vote trends. The Linear Regression Model focused on estimating whether each state would vote Republican or Democratic. To do this, it pulled in factors like past voting patterns, demographic data, polling averages, and economic indicators. This approach helped the model capture the more predictable, long-term voting trends, especially in states that tend to lean heavily toward one party. At the same time, it was flexible enough to account for shifts in swing states where the outcome was less certain.

The ARIMA(3) Model, on the other hand, was designed to handle the more dynamic nature of national popular vote trends. By incorporating a three-lag structure, it adjusted its predictions based on recent polling shifts, capturing changes in voter sentiment as they happened. This made it useful for understanding broader momentum in the race, especially in the weeks leading up to Election Day.

Together, these models gave me a strong framework for forecasting the election. The Linear Regression Model did a great job predicting the Electoral College outcome, accurately capturing both the solid partisan states and the battlegrounds. Meanwhile, the ARIMA model provided insight into the popular vote, though it had some trouble capturing the finer details of last-minute voter shifts. While the overall predictions aligned well with the actual results, there were still nuances—particularly in the popular vote—that the models didn’t fully capture.

Overall, these two models worked well together, balancing historical patterns with real-time trends. That said, the areas where they fell short showed me where I could improve, especially when it comes to incorporating late-stage voter behavior and other unpredictable factors.

<img src="ElectoralCollege.png" width="1023" />


### Assessing Model Accuracy:

The Linear Regression Model performed incredibly well in predicting the Electoral College results, achieving an RMSE (root mean square error) of 0. This means it perfectly predicted the winning party in every state, which is a pretty remarkable outcome. The RMSE calculated the difference between my prediction and the actual results, in which I labled a blue state with 1 and a red state with 0. It nailed Republican strongholds in the South and Mountain West, as well as Democratic dominance in the Northeast and along the West Coast, which was expected given the entrenched voting patterns in these regions. But what really stood out was its performance in battleground states like Michigan, Wisconsin, and Pennsylvania. These states ultimately flipped Republican, just as the model predicted, showing its ability to pick up on subtle shifts in voter sentiment based on polling, historical trends, and economic data. For battlegrounds that are notoriously difficult to forecast, this level of accuracy was impressive.


```
## [1] 0
```

The ARIMA(3) Model, which focused on national popular vote trends, also performed well in its own way. It forecasted Kamala Harris’s popular vote share at 49.3%, with a margin of error of just 0.53%. This suggests that the model was really effective at capturing national-level momentum and broader trends in voter sentiment leading up to the election. By incorporating recent shifts in polling data through its three-lag structure, the ARIMA model was able to reflect the short-term dynamics in public opinion.

That said, the ARIMA model did have some blind spots. It relied on polling data collected two to three weeks before Election Day, which meant it didn’t fully capture late turnout surges for Donald Trump in key states. This likely explains some of the gaps between the national trends it reflected and the final Electoral College outcome. Factors like late-stage campaign events, voter mobilization efforts, or last-minute shifts in sentiment—things that polls closer to Election Day might have picked up—were outside the model’s scope.

Together, the two models worked well as complementary tools. The Linear Regression Model excelled at state-level precision, getting every Electoral College prediction right, while the ARIMA model provided a broader view of the race’s momentum. While the ARIMA model wasn’t perfect, it added important context to the national popular vote trends that the linear model didn’t focus on. Combining these approaches made it possible to get both a detailed state-level understanding and a sense of the bigger picture. Ultimately, the Linear Regression Model’s perfect predictions stood out, but the ARIMA model’s insight into voter trends helped round out the overall analysis.




### Inaccuracies: Hypotheses and Grounded Reasoning

The ARIMA(3) Model did a solid job capturing national polling trends but had some limitations when it came to predicting final outcomes in battleground states. One major issue was that it relied on polling data from two to three weeks before Election Day, which means it didn’t pick up on late-stage changes in voter sentiment or turnout surges. This is a big deal in states like Wisconsin and Michigan, where small shifts in turnout or undecided voters breaking one way can completely change the result. The model predicted 49.82% for Kamala Harris in Michigan and 50.12% in Wisconsin, and while these numbers were close, slight Republican turnout surges ended up flipping both states.

It’s important to note that the graphs for Wisconsin and Michigan show the difference in polling support between Democrats and Republicans over time, not percentages. So while the numbers the model predicted aren’t far off from what the graphs suggest, the tight margins in both states meant that even small turnout variations on Election Day could swing the outcome. In Michigan, for example, the model predicted a narrow Democratic win based on polling trends, but the gap between the parties closed significantly in the final weeks, and strong Republican turnout ultimately tipped the state. Similarly, in Wisconsin, the prediction reflected a slim Democratic lead that didn’t hold up when Republican voters showed up in larger-than-expected numbers.

Another challenge was the ARIMA model’s focus on polling trends without factoring in localized influences like economic and demographic shifts. For instance, national GDP growth might have looked neutral or slightly favorable for Democrats, but voters in manufacturing-heavy areas of the Midwest, like Wisconsin and Michigan, may have responded differently based on their local economies. This kind of regional variation wasn’t something the model accounted for directly.

On top of that, last-minute campaign events—like endorsements, controversies, or policy announcements—can have a big impact on undecided voters, especially in competitive states. These kinds of nonlinear effects are hard for a time-series model like ARIMA to pick up, especially since they often happen after the last round of polling included in the data. The close predictions for Michigan and Wisconsin show that it was directionally accurate, but the slight differences highlight the need to account for these unpredictable factors when trying to forecast state-level results.



<img src="MichiganArima.png" width="1016" /><img src="WisconsinArima.png" width="1008" />




### Proposed Model Improvements:

Reflecting on these challenges, there are several ways the models could be improved for future use. A key area of improvement would be incorporating late-stage polling data with greater weight to account for last-minute shifts in voter sentiment. This could include real-time updates from early voting data or turnout projections, which often provide critical insights in the final days before an election. Additionally, integrating dynamic economic modeling that captures state-level variations in how factors like unemployment or industry-specific GDP growth affect voting behavior would make the models more regionally accurate. For example, understanding how voters in manufacturing-heavy states like Michigan respond to economic recovery metrics could significantly improve predictions in competitive states.

Another important enhancement would be to include [demographic trend data](https://www.forwardpathway.us/analyzing-voter-behavior-and-trumps-impact-in-2024-election), such as Census updates on migration patterns and age distribution, to better capture long-term shifts in voting behavior. This could be particularly helpful in states like Arizona and Georgia, where younger, more diverse populations are changing the political landscape. Finally, adopting a hierarchical or multilevel modeling approach would allow for better integration of state-level predictions with national trends. This would make it easier to account for the interplay between localized dynamics and broader voter sentiment shifts, especially in battleground states where small changes can swing outcomes. Together, these refinements would make the models more robust and adaptable to the complexities of future elections.

To evaluate why my models performed as well as they did—and where they could improve—I think two areas stand out: polling timing and regional economic influences. These seem to line up the most with the dynamics that shaped the election and the accuracy of my predictions, especially in competitive states like Michigan and Wisconsin.

First, late-deciding voters are probably a big part of the story. A lot can change in the final weeks of a campaign, and polling data from two or three weeks out doesn’t always capture those shifts. Doing a rolling average analysis of polls leading up to Election Day could help show how last-minute swings impacted the results. My models picked up the general trends in these states, but the last-minute surges for Donald Trump—likely driven by final campaign events or turnout pushes—ultimately flipped states like Michigan and Wisconsin. Adding more late-stage polling data or incorporating early voting numbers might make the predictions even sharper next time.

Second, the role of regional economic factors is worth digging into. My Linear Regression Model did a good job because it included economic indicators, but I think some of the regional nuances were harder to capture. For instance, in manufacturing-heavy states like Michigan and Wisconsin, voters might have been especially sensitive to perceptions of job growth or economic recovery. In other regions, like the Sun Belt, those same indicators may not have carried as much weight. A deeper regression analysis of how state-level economic factors impacted voting could help refine the models. Overall, these two areas—polling timing and economic variation—offer a lot of potential for understanding why the models worked well and how they could get even better.


### Lessons for Future Forecasting:

Overall, to evaluate why my models performed as well as they did—and where they could improve—I think two areas stand out: polling timing and regional economic influences. These seem to line up the most with the dynamics that shaped the election and the accuracy of my predictions, especially in competitive states like Michigan and Wisconsin.

First, late-deciding voters are probably a big part of the story. A lot can change in the final weeks of a campaign, and polling data from two or three weeks out doesn’t always capture those shifts. Doing a rolling average analysis of polls leading up to Election Day could help show how last-minute swings impacted the results. My models picked up the general trends in these states, but the last-minute surges for Donald Trump—likely driven by final campaign events or turnout pushes—ultimately flipped states like Michigan and Wisconsin. Adding more late-stage polling data or incorporating early voting numbers might make the predictions even sharper next time.

Second, the role of regional economic factors is worth digging into. My Linear Regression Model did a good job because it included economic indicators, but I think some of the regional nuances were harder to capture. For instance, in manufacturing-heavy states like Michigan and Wisconsin, voters might have been especially [sensitive to perceptions of job growth or economic recovery](https://www.forwardpathway.us/economic-issues-and-voter-concerns-in-the-2024-election). In other regions, like the Sun Belt, those same indicators may not have carried as much weight. A deeper regression analysis of how state-level economic factors impacted voting could help refine the models. These two areas—polling timing and economic variation—offer a lot of potential for understanding why the models worked well and how they could get even better.

Going forward, integrating richer data sources, such as real-time voter sentiment on social media or turnout projections, could further enhance model accuracy. This election demonstrated that while models are invaluable tools for understanding electoral dynamics, they must continuously evolve to account for the complexities of modern campaigns.

By refining these models and incorporating lessons from 2024, I aim to build even more accurate forecasting tools in the future.
