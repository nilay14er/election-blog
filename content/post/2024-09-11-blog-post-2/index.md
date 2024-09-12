---
title: Blog Post 2
author: Nilay Ersoy
date: '2024-09-11'
slug: blog-post-2
categories: []
tags: []
---


```r
# Load necessary libraries
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.3     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
# Load the datasets (assuming you already have them loaded)
d_popvote <- read_csv("popvote_1948-2020.csv")
```

```
## Rows: 38 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): party, candidate
## dbl (3): year, pv, pv2p
## lgl (4): winner, incumbent, incumbent_party, prev_admin
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
d_fred <- read_csv("fred_econ.csv")
```

```
## Rows: 387 Columns: 14
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (14): year, quarter, GDP, GDP_growth_quarterly, RDPI, RDPI_growth_quarte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# Filter relevant columns from popvote and fred datasets
d_popvote_filtered <- d_popvote %>%
  select(year, pv2p, incumbent_party)

d_fred_filtered <- d_fred %>%
  filter(quarter == 2) %>%  # Use Q2 data only
  select(year, unemployment)

# Merge datasets by year
d_inc_econ <- d_popvote_filtered %>%
  left_join(d_fred_filtered, by = "year")

# Fit the linear regression model
reg_econ <- lm(pv2p ~ unemployment, data = d_inc_econ)

# Print model summary to understand the results
summary(reg_econ)
```

```
## 
## Call:
## lm(formula = pv2p ~ unemployment, data = d_inc_econ)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -11.789  -3.295   0.000   3.295  11.789 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  5.000e+01  2.545e+00   19.65   <2e-16 ***
## unemployment 2.527e-16  4.025e-01    0.00        1    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.525 on 36 degrees of freedom
## Multiple R-squared:  1.837e-31,	Adjusted R-squared:  -0.02778 
## F-statistic: 6.615e-30 on 1 and 36 DF,  p-value: 1
```

```r
# Visualize the relationship with a regression line
ggplot(d_inc_econ, aes(x = unemployment, y = pv2p, color = as.factor(incumbent_party))) +
  geom_point(size = 3) +
  geom_smooth(method = "lm", se = TRUE, color = "blue") +
  labs(title = "Effect of Unemployment on Incumbent Party Vote Share",
       x = "Unemployment Rate (%)",
       y = "Incumbent Party's Popular Vote Share (%)",
       color = "Incumbent Party") +
  theme_minimal()
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-1-1.png" width="672" />

