---
title: "Blog Post 7"
author: "Nilay Ersoy"
date: "2024-10-19"
output: pdf_document
categories: []
tags: []
slug: "blog-post-7"
---




```
## Warning: package 'sf' was built under R version 4.3.3
```










```r
library(ggplot2)
library(sf)
library(readr)
library(maps)
library(ggspatial)

# Read field office data (adjust file path as necessary)
d_field_offices <- read_csv("fieldoffice_2012-2016_byaddress.csv")
```

```
## Rows: 1777 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (5): party, candidate, state, city, full_address
## dbl (3): year, latitude, longitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# Convert to an sf object for plotting
d_field_offices_sf <- st_as_sf(d_field_offices, coords = c("longitude", "latitude"), crs = 4326)

# Load USA map
us_map <- st_as_sf(maps::map("usa", plot = FALSE, fill = TRUE))

# Plot field offices on a map of the USA
ggplot() +
  geom_sf(data = us_map, fill = "gray80", color = "white") + # Map of the USA
  geom_sf(data = d_field_offices_sf, color = "red", size = 2, shape = 21, fill = "red") + # Field offices
  labs(title = "Locations of Previous Field Offices") +
  theme_minimal()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />












```r
fo_2012<- read_csv("fieldoffice_2012_bycounty.csv")
```

```
## Rows: 3113 Columns: 16
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): state
## dbl (11): fips, obama12fo, romney12fo, normal, medage08, pop2008, medinc08, ...
## lgl  (4): battle, swing, core_dem, core_rep
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
lm_obama <- lm(obama12fo ~ romney12fo + 
                 swing + 
                 core_rep + 
                 swing:romney12fo + 
                 core_rep:romney12fo + 
                 battle + 
                 medage08 + 
                 pop2008 + 
                 pop2008^2 + 
                 medinc08 + 
                 black + 
                 hispanic + 
                 pc_less_hs00 + 
                 pc_degree00 + 
                 as.factor(state), 
               fo_2012)

lm_romney <- lm(romney12fo ~ 
                  obama12fo + 
                  swing + 
                  core_dem + 
                  swing:obama12fo + 
                  core_dem:obama12fo + 
                  battle + 
                  medage08 + 
                  pop2008 + 
                  pop2008^2 + 
                  medinc08 + 
                  black + 
                  hispanic + 
                  pc_less_hs00 + 
                  pc_degree00 + 
                  as.factor(state),
                  fo_2012)

stargazer(lm_obama, lm_romney, header=FALSE, type='latex', no.space = TRUE,
          column.sep.width = "3pt", font.size = "scriptsize", single.row = TRUE,
          keep = c(1:7, 62:66), omit.table.layout = "sn",
          title = "Placement of Field Offices (2012)")
```

```
## 
## \begin{table}[!htbp] \centering 
##   \caption{Placement of Field Offices (2012)} 
##   \label{} 
## \scriptsize 
## \begin{tabular}{@{\extracolsep{3pt}}lcc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
##  & \multicolumn{2}{c}{\textit{Dependent variable:}} \\ 
## \cline{2-3} 
## \\[-1.8ex] & obama12fo & romney12fo \\ 
## \\[-1.8ex] & (1) & (2)\\ 
## \hline \\[-1.8ex] 
##  romney12fo & 2.546$^{***}$ (0.114) &  \\ 
##   obama12fo &  & 0.374$^{***}$ (0.020) \\ 
##   swing & 0.001 (0.055) & $-$0.012 (0.011) \\ 
##   core\_rep & 0.007 (0.061) &  \\ 
##   core\_dem &  & 0.004 (0.027) \\ 
##   battle & 0.541$^{***}$ (0.096) & 0.014 (0.042) \\ 
##   medage08 &  &  \\ 
##   romney12fo:swing & $-$0.765$^{***}$ (0.116) &  \\ 
##   romney12fo:core\_rep & $-$1.875$^{***}$ (0.131) &  \\ 
##   obama12fo:swing &  & $-$0.081$^{***}$ (0.020) \\ 
##   obama12fo:core\_dem &  & $-$0.164$^{***}$ (0.023) \\ 
##   Constant & $-$0.340$^{*}$ (0.196) & 0.001 (0.079) \\ 
##  \hline \\[-1.8ex] 
## \hline 
## \hline \\[-1.8ex] 
## \end{tabular} 
## \end{table}
```

```r
# Effects of field offices on turnout and vote share. 
fo_dem <- read_csv("fieldoffice_2004-2012_dems.csv")
```

```
## Rows: 9339 Columns: 18
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): candidate, state
## dbl (15): year, fips, number_fo, dummy_fo, population, totalvote, demvote, d...
## lgl  (1): battle
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
ef_t <- lm(turnout_change ~ dummy_fo_change + battle + dummy_fo_change:battle + as.factor(state) + as.factor(year), fo_dem)

ef_d <- lm(dempct_change ~ dummy_fo_change + battle + dummy_fo_change:battle + as.factor(state) + as.factor(year), fo_dem)

stargazer(ef_t, ef_d, header=FALSE, type='latex', no.space = TRUE,
          column.sep.width = "3pt", font.size = "scriptsize", single.row = TRUE,
          keep = c(1:3, 53:54), keep.stat = c("n", "adj.rsq", "res.dev"),
          title = "Effect of DEM Field Offices on Turnout and DEM Vote Share (2004-2012)")
```

```
## 
## \begin{table}[!htbp] \centering 
##   \caption{Effect of DEM Field Offices on Turnout and DEM Vote Share (2004-2012)} 
##   \label{} 
## \scriptsize 
## \begin{tabular}{@{\extracolsep{3pt}}lcc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
##  & \multicolumn{2}{c}{\textit{Dependent variable:}} \\ 
## \cline{2-3} 
## \\[-1.8ex] & turnout\_change & dempct\_change \\ 
## \\[-1.8ex] & (1) & (2)\\ 
## \hline \\[-1.8ex] 
##  dummy\_fo\_change & 0.004$^{***}$ (0.001) & 0.009$^{***}$ (0.002) \\ 
##   battle & 0.024$^{***}$ (0.002) & 0.043$^{***}$ (0.003) \\ 
##   as.factor(state)Arizona &  &  \\ 
##   dummy\_fo\_change:battle & $-$0.002 (0.002) & 0.007$^{**}$ (0.003) \\ 
##   Constant & 0.029$^{***}$ (0.002) & 0.022$^{***}$ (0.003) \\ 
##  \hline \\[-1.8ex] 
## Observations & 6,224 & 6,224 \\ 
## Adjusted R$^{2}$ & 0.419 & 0.469 \\ 
## \hline 
## \hline \\[-1.8ex] 
## \textit{Note:}  & \multicolumn{2}{r}{$^{*}$p$<$0.1; $^{**}$p$<$0.05; $^{***}$p$<$0.01} \\ 
## \end{tabular} 
## \end{table}
```

```r
# Field Strategies of Obama, Romney, Clinton, and Trump in 2016. 
fo_add <- read_csv("fieldoffice_2012-2016_byaddress.csv")
```

```
## Rows: 1777 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (5): party, candidate, state, city, full_address
## dbl (3): year, latitude, longitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
us_geo <- counties(cb = TRUE) |> 
  shift_geometry() |> 
  filter(STUSPS %in% unique(fo_add$state))
```

```
## Retrieving data for the year 2022
```

```
## 
  |                                                                            
  |                                                                      |   0%
  |                                                                            
  |                                                                      |   1%
  |                                                                            
  |=                                                                     |   1%
  |                                                                            
  |=                                                                     |   2%
  |                                                                            
  |==                                                                    |   2%
  |                                                                            
  |==                                                                    |   3%
  |                                                                            
  |==                                                                    |   4%
  |                                                                            
  |===                                                                   |   4%
  |                                                                            
  |===                                                                   |   5%
  |                                                                            
  |====                                                                  |   5%
  |                                                                            
  |====                                                                  |   6%
  |                                                                            
  |=====                                                                 |   6%
  |                                                                            
  |=====                                                                 |   7%
  |                                                                            
  |=====                                                                 |   8%
  |                                                                            
  |======                                                                |   8%
  |                                                                            
  |======                                                                |   9%
  |                                                                            
  |=======                                                               |   9%
  |                                                                            
  |=======                                                               |  10%
  |                                                                            
  |=======                                                               |  11%
  |                                                                            
  |========                                                              |  11%
  |                                                                            
  |========                                                              |  12%
  |                                                                            
  |=========                                                             |  12%
  |                                                                            
  |=========                                                             |  13%
  |                                                                            
  |==========                                                            |  14%
  |                                                                            
  |==========                                                            |  15%
  |                                                                            
  |===========                                                           |  15%
  |                                                                            
  |===========                                                           |  16%
  |                                                                            
  |============                                                          |  17%
  |                                                                            
  |============                                                          |  18%
  |                                                                            
  |=============                                                         |  18%
  |                                                                            
  |=============                                                         |  19%
  |                                                                            
  |==============                                                        |  20%
  |                                                                            
  |==============                                                        |  21%
  |                                                                            
  |===============                                                       |  21%
  |                                                                            
  |===============                                                       |  22%
  |                                                                            
  |================                                                      |  22%
  |                                                                            
  |================                                                      |  23%
  |                                                                            
  |=================                                                     |  24%
  |                                                                            
  |=================                                                     |  25%
  |                                                                            
  |==================                                                    |  26%
  |                                                                            
  |===================                                                   |  27%
  |                                                                            
  |===================                                                   |  28%
  |                                                                            
  |====================                                                  |  28%
  |                                                                            
  |====================                                                  |  29%
  |                                                                            
  |=====================                                                 |  29%
  |                                                                            
  |=====================                                                 |  30%
  |                                                                            
  |=====================                                                 |  31%
  |                                                                            
  |======================                                                |  31%
  |                                                                            
  |======================                                                |  32%
  |                                                                            
  |=======================                                               |  32%
  |                                                                            
  |=======================                                               |  33%
  |                                                                            
  |========================                                              |  34%
  |                                                                            
  |========================                                              |  35%
  |                                                                            
  |=========================                                             |  35%
  |                                                                            
  |=========================                                             |  36%
  |                                                                            
  |==========================                                            |  36%
  |                                                                            
  |==========================                                            |  37%
  |                                                                            
  |==========================                                            |  38%
  |                                                                            
  |===========================                                           |  38%
  |                                                                            
  |===========================                                           |  39%
  |                                                                            
  |============================                                          |  39%
  |                                                                            
  |============================                                          |  40%
  |                                                                            
  |============================                                          |  41%
  |                                                                            
  |=============================                                         |  41%
  |                                                                            
  |=============================                                         |  42%
  |                                                                            
  |==============================                                        |  42%
  |                                                                            
  |==============================                                        |  43%
  |                                                                            
  |===============================                                       |  44%
  |                                                                            
  |===============================                                       |  45%
  |                                                                            
  |================================                                      |  45%
  |                                                                            
  |================================                                      |  46%
  |                                                                            
  |=================================                                     |  47%
  |                                                                            
  |=================================                                     |  48%
  |                                                                            
  |==================================                                    |  48%
  |                                                                            
  |==================================                                    |  49%
  |                                                                            
  |===================================                                   |  50%
  |                                                                            
  |===================================                                   |  51%
  |                                                                            
  |====================================                                  |  51%
  |                                                                            
  |====================================                                  |  52%
  |                                                                            
  |=====================================                                 |  52%
  |                                                                            
  |=====================================                                 |  53%
  |                                                                            
  |=====================================                                 |  54%
  |                                                                            
  |======================================                                |  54%
  |                                                                            
  |======================================                                |  55%
  |                                                                            
  |=======================================                               |  55%
  |                                                                            
  |=======================================                               |  56%
  |                                                                            
  |========================================                              |  57%
  |                                                                            
  |========================================                              |  58%
  |                                                                            
  |=========================================                             |  58%
  |                                                                            
  |=========================================                             |  59%
  |                                                                            
  |==========================================                            |  59%
  |                                                                            
  |==========================================                            |  60%
  |                                                                            
  |==========================================                            |  61%
  |                                                                            
  |===========================================                           |  61%
  |                                                                            
  |===========================================                           |  62%
  |                                                                            
  |============================================                          |  62%
  |                                                                            
  |============================================                          |  63%
  |                                                                            
  |=============================================                         |  64%
  |                                                                            
  |==============================================                        |  65%
  |                                                                            
  |==============================================                        |  66%
  |                                                                            
  |===============================================                       |  67%
  |                                                                            
  |================================================                      |  68%
  |                                                                            
  |================================================                      |  69%
  |                                                                            
  |=================================================                     |  70%
  |                                                                            
  |=================================================                     |  71%
  |                                                                            
  |==================================================                    |  71%
  |                                                                            
  |==================================================                    |  72%
  |                                                                            
  |===================================================                   |  72%
  |                                                                            
  |===================================================                   |  73%
  |                                                                            
  |====================================================                  |  74%
  |                                                                            
  |====================================================                  |  75%
  |                                                                            
  |=====================================================                 |  75%
  |                                                                            
  |=====================================================                 |  76%
  |                                                                            
  |======================================================                |  77%
  |                                                                            
  |======================================================                |  78%
  |                                                                            
  |=======================================================               |  78%
  |                                                                            
  |=======================================================               |  79%
  |                                                                            
  |========================================================              |  79%
  |                                                                            
  |========================================================              |  80%
  |                                                                            
  |=========================================================             |  81%
  |                                                                            
  |=========================================================             |  82%
  |                                                                            
  |==========================================================            |  82%
  |                                                                            
  |==========================================================            |  83%
  |                                                                            
  |===========================================================           |  84%
  |                                                                            
  |===========================================================           |  85%
  |                                                                            
  |============================================================          |  85%
  |                                                                            
  |============================================================          |  86%
  |                                                                            
  |=============================================================         |  87%
  |                                                                            
  |=============================================================         |  88%
  |                                                                            
  |==============================================================        |  88%
  |                                                                            
  |==============================================================        |  89%
  |                                                                            
  |===============================================================       |  89%
  |                                                                            
  |===============================================================       |  90%
  |                                                                            
  |===============================================================       |  91%
  |                                                                            
  |================================================================      |  91%
  |                                                                            
  |================================================================      |  92%
  |                                                                            
  |=================================================================     |  92%
  |                                                                            
  |=================================================================     |  93%
  |                                                                            
  |=================================================================     |  94%
  |                                                                            
  |==================================================================    |  94%
  |                                                                            
  |==================================================================    |  95%
  |                                                                            
  |===================================================================   |  95%
  |                                                                            
  |===================================================================   |  96%
  |                                                                            
  |====================================================================  |  96%
  |                                                                            
  |====================================================================  |  97%
  |                                                                            
  |===================================================================== |  98%
  |                                                                            
  |===================================================================== |  99%
  |                                                                            
  |======================================================================|  99%
  |                                                                            
  |======================================================================| 100%
```

```r
obama12 <- subset(fo_add, year == 2012 & candidate == "Obama") %>%
  select(longitude, latitude)
romney12 <- subset(fo_add, year == 2012 & candidate == "Romney") %>%
  select(longitude, latitude)
clinton16 <- subset(fo_add, year == 2016 & candidate == "Clinton") %>%
  select(longitude, latitude)
trump16 <- subset(fo_add, year == 2016 & candidate == "Trump") %>%
  select(longitude, latitude)

obama12transformed <- st_as_sf(obama12, coords = c("longitude", "latitude"), crs = 4326) |> 
  st_transform(crs = st_crs(us_geo)) |> 
  shift_geometry()
romney12transformed <- st_as_sf(romney12, coords = c("longitude", "latitude"), crs = 4326) |>
  st_transform(crs = st_crs(us_geo)) |>
  shift_geometry()
```

```
## Warning: None of your features are in Alaska, Hawaii, or Puerto Rico, so no geometries will be shifted.
## Transforming your object's CRS to 'ESRI:102003'
```

```r
clinton16transformed <- st_as_sf(clinton16, coords = c("longitude", "latitude"), crs = 4326) |>
  st_transform(crs = st_crs(us_geo)) |>
  shift_geometry()
trump16transformed <- st_as_sf(trump16, coords = c("longitude", "latitude"), crs = 4326) |>
  st_transform(crs = st_crs(us_geo)) |>
  shift_geometry()
```

```
## Warning: None of your features are in Alaska, Hawaii, or Puerto Rico, so no geometries will be shifted.
## Transforming your object's CRS to 'ESRI:102003'
```

```r
(ob12 <- ggplot() +
  geom_sf(data = us_geo) + 
  geom_sf(data = obama12transformed, color = "dodgerblue4", alpha = 0.75, pch = 3) +
  ggtitle("Obama 2012 Field Offices") +
  theme_void())
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
(ro12 <- ggplot() +
  geom_sf(data = us_geo) + 
  geom_sf(data = romney12transformed, color = "firebrick", alpha = 0.75, pch = 3) +
  ggtitle("Romney 2012 Field Offices") +
  theme_void())
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-2.png" width="672" />

```r
(cl16 <- ggplot() + 
  geom_sf(data = us_geo) + 
  geom_sf(data = clinton16transformed, color = "dodgerblue4", alpha = 0.75, pch = 3) +
  ggtitle("Clinton 2016 Field Offices") +
  theme_void())
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-3.png" width="672" />

```r
(tr16 <- ggplot() +
  geom_sf(data = us_geo) + 
  geom_sf(data = trump16transformed, color = "firebrick", alpha = 0.75, pch = 3) +
  ggtitle("Trump 2016 Field Offices") +
  theme_void())
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-4.png" width="672" />

```r
ggarrange(ob12, ro12, cl16, tr16, nrow = 2, ncol = 2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-5.png" width="672" />

```r
# Clinton 2016 Field Offices - Obama 2008 Field Offices. 
fo_add %>%
  subset(candidate %in% c("Clinton", "Obama") &
           state %in% c("CO", "FL", "IA", "MI", "NV", "NH", "NC", "OH", "PA", "VA", "WI")) %>%
  group_by(state, candidate) %>%
  summarize(fo = n()) %>%
  spread(key = candidate, value = fo) %>%
  mutate(diff = Clinton - Obama) %>%
  select(state, diff) %>%
  ggplot(aes(y = diff, x = state, fill = (diff > 0))) +
  geom_bar(stat = "identity") +
  coord_flip() +
  ylim(-50, 15) +
  scale_y_continuous(breaks=seq(-50,10,10)) +
  xlab("State") +
  ylab("Clinton Field Offices - Obama '08 field offices")+
  theme_minimal() +
  theme(legend.position = "none",
        text = element_text(size = 15))
```

```
## `summarise()` has grouped output by 'state'. You can override using the
## `.groups` argument.
## Scale for y is already present. Adding another scale for y, which will replace
## the existing scale.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-6.png" width="672" />






```
## 
## \begin{table}[!htbp] \centering 
##   \caption{Placement of Field Offices (2012)} 
##   \label{} 
## \scriptsize 
## \begin{tabular}{@{\extracolsep{3pt}}lcc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
##  & \multicolumn{2}{c}{\textit{Dependent variable:}} \\ 
## \cline{2-3} 
## \\[-1.8ex] & obama12fo & romney12fo \\ 
## \\[-1.8ex] & (1) & (2)\\ 
## \hline \\[-1.8ex] 
##  romney12fo & 2.546$^{***}$ (0.114) &  \\ 
##   obama12fo &  & 0.374$^{***}$ (0.020) \\ 
##   swing & 0.001 (0.055) & $-$0.012 (0.011) \\ 
##   core\_rep & 0.007 (0.061) &  \\ 
##   core\_dem &  & 0.004 (0.027) \\ 
##   battle & 0.541$^{***}$ (0.096) & 0.014 (0.042) \\ 
##   medage08 &  &  \\ 
##   romney12fo:swing & $-$0.765$^{***}$ (0.116) &  \\ 
##   romney12fo:core\_rep & $-$1.875$^{***}$ (0.131) &  \\ 
##   obama12fo:swing &  & $-$0.081$^{***}$ (0.020) \\ 
##   obama12fo:core\_dem &  & $-$0.164$^{***}$ (0.023) \\ 
##   Constant & $-$0.340$^{*}$ (0.196) & 0.001 (0.079) \\ 
##  \hline \\[-1.8ex] 
## \hline 
## \hline \\[-1.8ex] 
## \end{tabular} 
## \end{table}
```

```
## Warning in lm(romney12fo ~ obama12fo + swing + core_dem + swing:obama12fo + :
## longer object length is not a multiple of shorter object length
```













