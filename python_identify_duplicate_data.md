---

layout: minimal
title: Digital Analytics Cookbook

---
# Identifying Duplicate Sales Data using Python
Author: Jason Thompson

__Problem__: More often than not, analysts are given data that is unstructured and dirty. In this example, an analyst has been provided a 300MB Excel Workbook containing sales transactions, however it has been noted that several of the transactions have been duplicated in the data. Before doing any analysis of the sales data, the duplicate data needs to be cleaned up.

__Actions__: Our sample dataset contains every order transaction for 2015. The data is structured in such a way that each item purchased, in an order, is a unique row in the data. If an order contained three unique product SKUs, that one order would have three rows in the dataset.

We need to identify orders that contain duplicated order line items, those duplicates would have a SKU that appears in more than one row of data, for a given order_id, as shown below.

![Duplicate Data Example](/images/python_duplicate_order_example.png)


Notes about the data:

  * order_id: A unique id that serves as the key to group line items into a single order
  * order_item_cd: Product SKU
  * order_item_quantity: The number each SKU purchased for an order
  * order_item_cost_price: The individual unit price of a SKU purchased

__Explanation (resolution)__:

Let's begin begin by importing the pandas library. pandas is an open source, BSD-licensed library providing high-performance, easy-to-use data structures and data analysis tools for Python.

{% highlight python %}
In [1]:
# We will use data structures and data analysis tools provided in the pandas library
import pandas as pd
{% endhighlight %}
Next, we will use pandas to import all of the sales data from our Excel Workbook into a pandas DataFrame. The pandas DataFrame, along with Series, is one of the most important data structures you will use as a data analyst. Creating a DataFrame is one of the first things we typically do after launching Python.

A DataFrame is tabular in nature and has a ?spreadsheet like? data structure containing an ordered collection of columns and rows.

{% highlight python %}
In [2]:
# Import retail sales data from an Excel Workbook into a data frame
path = '/Documents/analysis/python/examples/2015sales.xlsx'
xlsx = pd.ExcelFile(path)
df = pd.read_excel(xlsx, 'Sheet1')
{% endhighlight %}

We have our data in a DataFrame, which resembles the structure shown in the example sales data above, now we need a way to identify all of the duplicate rows in our dataset. To accomplish this we are going to add a new column to our DataFrame, named `is_duplicated`, that will hold a boolean value identifying if the row is a duplicate or not. To populate the boolean value for each row in the dataset, we are going to use a very powerful function called `duplicated` which we will feed the columns we want to evaluate for duplication. In this case, we will mark a row as a duplicate if we find more than one row that has the same order_id and order_item_sku.

{% highlight python %}
In [3]:
# Let's add a new boolean column to our DataFrame that will identify a duplicated order line item (False=Not a duplicate; True=Duplicate)
df['is_duplicated'] = df.duplicated(['order_id', 'order_item_cd'])
{% endhighlight %}

Before we go any further, let\'s get a sense of how big of an issue we have here. We are going to sum up all of the rows that were marked as a duplicate. This will give us the number of duplicate line items in the dataset.

{% highlight python %}
In [4]:
# We can sum on a boolean column to get a count of duplicate order line items
df['is_duplicated'].sum()

Out[4]:
13372
{% endhighlight %}
Ok, so this isn't just a few records here and there that are a problem, our dataset contains 13,372 line items that have been marked as being duplicates. Before we look to do any cleanup, let's further understand the impact of this dirty data. Let's find out how many duplicate units are in our dataset, we would expect at least 13,372 units but there is a high likelihood that customers often purchase more than one unit of any given SKU.

Like we did in the previous step, let's sum up the number of items purchased that were marked as being duplicates in our dataset.

{% highlight python %}
In [5]:
# Now let's quantify the impact of duplicate line items in terms of over counted units sold
df[df['is_duplicated']].order_item_quantity.sum()

Out[5]:
63234.0
{% endhighlight %}

Let's see what the impact to revenue is. In order to get the impacted revenue, we will add a new column to our DataFrame which will be the line item total for each row in the dataset. To get this value, we will multiple quantity purchased by the item cost, for each row.

Once we have the total for each line item, we can again sum all of the duplicated line items, this time using our revenue value.

{% highlight python %}
In [6]:
# With Python, we can use 'vectorizing' to multiple two columns and place the result in a new column
# This saves us from having to loop over every row in our sales data
df['line_item_total'] = df.order_item_quantity * df.order_item_cost_price

# We can use our boolean sum shortcut again to get the sum of revenue from duplicated order line items
df[df['is_duplicated']].line_item_total.sum()

Out[6]:
4736155.8047346813
{% endhighlight %}

Let\'s clean the duplicate data up.

{% highlight python %}
In [7]:
# We are only interested in the non-duplicated order line items, so let's get those now
df_nodup = df.loc[df['is_duplicated'] == False]
{% endhighlight %}

It's always a good practice to constantly check our work. Let's do a quick sanity check to make sure we have gotten rid of the problematic duplicate data.

{% highlight python %}
In [8]:
# Let's do a quick sanity check to make sure we now have a dataset with no identified duplicate order line items
df_nodup[df_nodup['is_duplicated']].line_item_total.sum()

Out[8]:
0
{% endhighlight %}

Now we can export this cleaned up dataset so that we can do additional analysis of our 2015 sales data without having to remove all the duplicate data again.

{% highlight python %}
In [9]:
# Finally let's save our cleaned up data to a csv file
df_nodup.to_csv('2015sales_nodup.csv', encoding='utf-8')
{% endhighlight %}

#### References

[GitHub Gist](https://gist.github.com/33sticks/b092f7e45b1fc089e360)
