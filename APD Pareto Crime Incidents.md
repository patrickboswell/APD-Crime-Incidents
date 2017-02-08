Analysis of Austin Police Department Criminal Incidents August 2015 - August 2016
========================================================
author: Patrick Boswell
date: February 8, 2017
autosize: true

First Slide
========================================================

- Retrive data from Socrata data site data.austintexas.gov
- Manipulate data for pareto chart
- Plot pareto chart by Criminal Type

Slide With Code
========================================================


```r
library(plotly)
library(RSocrata)
library(dplyr)

APData <- read.socrata("https://data.austintexas.gov/OData.svc/b4y9-5x39", stringsAsFactors = FALSE)

APData <- APData[,1:2] %>%
  count(Crime.Type) %>%
  mutate(percent = n/sum(n)) %>%
  arrange(desc(n)) %>%
  mutate(cusum = cumsum(percent))

APData$Crime.Type <- factor(APData$Crime.Type, levels = unique(APData$Crime.Type))

pplot <- plot_ly(APData) %>%
  add_trace(x = ~Crime.Type, y = ~n, type = 'bar', name = 'Incidents',
            marker = list(color = '#C9EFF9'),
            hoverinfo = "text",
            text = ~paste(n, '|', Crime.Type)) %>%
  add_trace(x = ~Crime.Type, y = ~cusum, type = 'scatter', mode = 'lines', name = '% of Total Cusum', yaxis = 'y2',
            line = list(color = '#45171D'),
            hoverinfo = "text",
            text = ~paste(cusum, '%')) %>%
  layout(title = 'Pareto Chart of Austin Policy Department Incidents August 2015 - August 2016',
         xaxis = list(title = ""),
         yaxis = list(side = 'left', title = 'Count of Incidents', showgrid = FALSE, zeroline = FALSE),
         yaxis2 = list(side = 'right', overlaying = "y", title = 'Cusum: Percent (%) of Incidents', showgrid = FALSE, zeroline = FALSE),
         autosize = T)
```

Slide With Plot
========================================================



```
Error in loadNamespace(name) : there is no package called 'webshot'
```