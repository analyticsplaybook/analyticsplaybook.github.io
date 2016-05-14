---

layout: post
title: Creating Interactive Dashboards in R 
date:   2016-05-16
author: Jowanza Joseph
category: dataviz
tags:
  - R
  - dashboard
  - visualization

---


### Problem

Once painstakingly collected, data is most value when actionable steps are taken from it. Methods of safely distributing and communicating results from data are not all created equally. Dashboards are a safe and effective way to communicate results from data.

### Actions

We’ll walk through an example of how to create a dashboard in R using *flexdashboard*, an R framework for creating dashboards with R and Markdown.

### Example

For this example we'll grab data from Google Analytics and use it to make 3 charts on a dashboard. 

1. A histogram
2. A time series chart 
3. Sortable table


To get this all to work, we'll need to wrap our code in a flexdashboard template. You can find out how to do that [here](http://rmarkdown.rstudio.com/flexdashboard/). We'll use the row template to get our charts displaying well on the page and allow for vertical scrolling.

```R
---
title: "My Google Analytics Dashboard"
output: 
  flexdashboard::flex_dashboard:
    orientation: rows
    vertical_layout: scroll
---
```

For chart 1 we'll use DT, a data table library to create a searchable and sortable table for our dashboard. I used data from the in-market segment in Google Analytics. Plotting the chart with default stylings is easy:

```R
datatable(my_data_table)
```

Chart 2 will be our histogram. The code for creating a histogram in Highcharter is very simple as well:

```R
hchart(dataset$Sessions, color = "#B71C1C", name = "Sessions") %>% 
  hc_title(text = "Sessions May 2015 - May 2016")
  
```

This should plot a histogram of traffic to my website from May 2015 through May 2016.  This chart is also zoomable, which is a nice feature to get for free.


Chart 3 is a time series chart. I will also use Highcharter for this as well:

```R
highchart() %>%
  hc_add_series_times_values(as.Date(dataset$Day.Index) , dataset$Sessions, name = "Sessions")

```

This gives us visits by time over the same range.

From here you can click "Knit" in Rstudio and it will compile this markdown document into HTML. You will have a working, interactive and responsive dashboard. I've published a working version to rPubs for reference [here](http://rpubs.com/josep2/example-ga-dashboard).

![image](http://i.imgur.com/FvPLUmS.png)

### References

[flexdashboard](http://rmarkdown.rstudio.com/flexdashboard/index.html)
[highcharter](http://jkunst.com/highcharter/index.html)
[datatable]()
[DT](http://rstudio.github.io/DT/)