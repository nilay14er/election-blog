---
title: Election Blog 1
author: Nilay
date: '2024-09-12'
slug: election-blog-1
categories: []
tags: []
---








```
## Warning in left_join(states_map, swing_data, by = "state"): Detected an unexpected many-to-many relationship between `x` and `y`.
## ℹ Row 1 of `x` matches multiple rows in `y`.
## ℹ Row 1 of `y` matches multiple rows in `x`.
## ℹ If a many-to-many relationship is expected, set `relationship =
##   "many-to-many"` to silence this warning.
```

```
## Warning: Removed 43 rows containing missing values (`geom_text_repel()`).
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />







<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />






<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />




```
## 'data.frame':	4 obs. of  5 variables:
##  $ date        : Factor w/ 4 levels "4/30/24","7/9/24",..: 4 3 2 1
##  $ harris      : int  50 50 49 44
##  $ trump       : int  46 45 46 46
##  $ someone_else: int  3 4 5 8
##  $ skip        : int  0 1 0 1
```

```
## 'data.frame':	16 obs. of  3 variables:
##  $ date      : Factor w/ 4 levels "4/30/24","7/9/24",..: 4 3 2 1 4 3 2 1 4 3 ...
##  $ candidate : Factor w/ 4 levels "harris","trump",..: 1 1 1 1 2 2 2 2 3 3 ...
##  $ percentage: int  50 50 49 44 46 45 46 46 3 4 ...
```

```
## [1] harris       trump        someone_else skip        
## Levels: harris trump someone_else skip
```

```
##      date candidate percentage
## 1 8/27/24    harris         50
## 2 8/13/24    harris         50
## 3  7/9/24    harris         49
## 4 4/30/24    harris         44
## 5 8/27/24     trump         46
## 6 8/13/24     trump         45
## 7  7/9/24     trump         46
## 8 4/30/24     trump         46
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

 


