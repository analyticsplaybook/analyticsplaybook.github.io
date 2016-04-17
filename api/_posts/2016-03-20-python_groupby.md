---

layout: post
title: Aggregate Sales Data using Python
date:   2016-03-20
author: Jason Thompson
category: api
tags:
  - api
  - group
  
---

### Problem
As an analyst, you are asked to provide insights into the frequency in which specific quantities of your products are ordered. To help make the insights more actionable, your boss has requested that your analysis be done at the product category level.


### Actions
We can pull our sales data into Python to quickly group and aggregate the data.

{% highlight python %}
#import sales data from csv
import pandas as pd
sales = pd.read_csv('http://analyticsplaybook.org/api/sample-sales.csv')
{% endhighlight %}

Once we have imported our sales data, we can use sales.head() to get a sense of how our data is formatted.

![Sample Sales Data](/images/pygroup_sample_sales.png)

We can see from the data format that we can use 'category' and 'quantity' to properly group the sales data. 

To get the frequency of how often specific quantities are purchased for each category, we can use the following:

{% highlight python %}
catFreq = sales.groupby(['category','quantity'])['category'].agg({'Frequency':'count'})
{% endhighlight %}

![Sample Sales Data](/images/pygroup_sample_results.png)



### Explanation (resolution)

Using a few simple commands in Python, we can quickly group, sales.groupby(['category','quantity']), and aggregate, .agg({'Frequency':'count'}), the sales data to provide the insights that were requested. 



### References
[pandas.DataFrame.groupby](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html) <br>
[Sample sales data from Practical Business Python](http://pbpython.com/extras/sample-sales.csv)
