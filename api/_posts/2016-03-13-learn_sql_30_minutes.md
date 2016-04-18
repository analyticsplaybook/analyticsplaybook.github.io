---

layout: post
title: Learn SQL in 30 Minutes Using R and sqldf
date:   2016-03-13
author: Randy Zwitch
category: api
tags:
  - api
  - sql
  - video

---

### Problem
Data rarely arrives "clean". You may have [duplicate data in your transactional files](http://analyticsplaybook.org/api/python_identify_duplicate_data.html), a customer table without a unique key, or you may be dealing with data that has no firm relationship (such as several time series datasets for overlapping time periods).

While it's possible to use fancy Excel tricks to merge and clean data, using the proper tool for the job ensures accuracy and repeatability. That tool is SQL, or [Structured Query Language](https://en.wikipedia.org/wiki/SQL).

### Actions

In this 30-minute video, I cover the most common operations that are done for data preparation. For simplicity of setup, I use R, RStudio and sqldf, which automatically sets up a SQLite database environment when you call the package. Viewing this tutorial, and especially following along with the code examples and practicing, will make you a more valuable analyst.

The [example data files](http://randyzwitch.com/wp-content/uploads/2013/11/r-sql-demo-files.zip) are available for download.

<iframe width="640" height="480" src="https://www.youtube.com/embed/s2oTUsAJfjI" frameborder="0" allowfullscreen>
</iframe>

### References
[Video: SQL Queries in R using sqldf](http://randyzwitch.com/sqldf-package-r/)<br>
[RStudio](https://www.rstudio.com/)<br>
[sqldf](https://github.com/ggrothendieck/sqldf)<br>
