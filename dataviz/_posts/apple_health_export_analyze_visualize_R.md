---

layout: post
title: Real-Time Reporting Adobe Analytics API Tutorial
date:   2016-08-10
author: Ryan Praskievicz
category: api
tags:
  - R
  - visualization
  - quantified self

---

### Problem
The Apple HealthKit App data visualization sucks.
How to export, analyze and visualize Apple Health Kit Steps data using R?

### Actions
Export Apple HealthKit data from your iPhone.
Run R Script for analysis and visualizations.

![Apple Health Steps by Day of Week R Visualization](http://i2.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_kit_steps_by_day_of_week_r_graph.png)

### Explanation (resolution)

How to Export Apple Health App Data

1) Launch the Apple Health App on your iPhone. The app icon is a heart.

2) Tap the Health Data icon in the bottom navigation. This will launch a list of all your Apple Heath data. In the List view, tap the “All” item which is the first item in the list.

![Apple HealthKit Export 1](http://i0.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_export_1-e1464963113156-173x300.png?resize=231%2C400)

3) Tap the send arrow icon in the top right. This will launch an alert that says “Exporting Health Data Preparing…” The export preparation took 4 minutes for my health data, so be patient.

![Apple HealthKit Export 2](http://i2.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_export_2-e1464963298330-172x300.png?resize=229%2C400)

4) Once the data is ready to send you’ll see an overlay where you can select how to send the health data export. I chose to send the file via email. The file name is export.zip.

![Apple HealthKit Export 3](http://i0.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_export_3-e1464963456151.png)
![Apple HealthKit Export 4](http://i0.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_export_4-e1464963494135.png)

How to user R to Analyze and Visualize your Apple Health Data

1) Make sure to install the packages used in the R script if you don't already have them installed `> install.packages(c("dplyr","ggplot2","lubridate","XML"))`
2) Run the [R Script](https://gist.github.com/ryanpraski/ba9baee2583cfb1af88ca4ec62311a3d)
3) Page through the 4 plots and see the R Console for data tables. 

A boxplot showing my steps data by month and a bar graph showing my steps data by month are shown below.

![Apple Health Steps Boxplot]()http://i0.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_kit_steps_by_month_by_year_r_boxplot.png)
![Apple Health Steps Bar Graph](http://i2.wp.com/www.ryanpraski.com/wp-content/uploads/2016/06/apple_health_kit_steps_by_year_by_month__r_bargraph.png)

My summary monthly steps statistics for 2015 are shown below. This data is output to the R Console.

`#Steps summary stats by month for 2015 Output to R Console   
   month     mean      sd  median   max   min      25%     75%
   <chr>    <dbl>   <dbl>   <dbl> <dbl> <dbl>    <dbl>   <dbl>
1     01  6928.45 3499.36  5924.0 15173  2286  4007.00  8581.0
2     02 12000.07 5727.69 11977.5 22675  4097  6853.25 15649.5
3     03 11271.26 2579.44 11662.0 15199  6269  9723.50 12667.0
4     04 14846.27 5825.21 14257.0 25357  4445 10925.75 19322.0
5     05 13119.45 5139.61 12405.0 25031  2971  9829.50 16222.0
6     06 11457.70 5083.92  9904.5 25643  4301  7424.25 14225.0
7     07 16419.06 7369.98 14750.0 35582  4546 11243.50 20911.5
8     08 13968.32 6855.27 12189.0 32019  2897 10469.00 14561.0
9     09 13096.07 5272.44 12753.0 29838  5737 10155.50 15987.0
10    10 12150.16 5163.45 11227.0 27174  3906  8952.00 15359.5
11    11 10442.80 4405.78  9476.5 22683  3814  8233.75 12669.5
12    12  8331.03 3933.16  8098.0 15450  1556  5192.00 11396.5`


### References

[Full Post on ryanpraski.com](http://www.ryanpraski.com/apple-health-data-how-to-export-analyze-visualize-guide/)

If you have any questions hit me up on Twitter [@ryanpraski](https://twitter.com/ryanpraski).
