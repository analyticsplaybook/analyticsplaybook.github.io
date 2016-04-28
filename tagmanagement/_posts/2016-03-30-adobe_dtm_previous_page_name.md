---

layout: post
title: Adobe DTM - Previous Page Name
date:   2016-03-30
author: Till Buettner
category: tagmanagement
tags:
  - adobedtm
  - tms
  - tracking
  - adobeanalytics

---

### Problem

You want to track the previous page name via Adobe DTM but you can't use a Plugin (getPreviousValue).

### Actions

You can use Session Storage to track the previous page name.

### Explanation (resolution)

You need two Page Load Rules (PLR) to track the previous page name. First PLR:

- Name: Save prior page name to session storage
- Trigger Rule at "DOM Ready"
- Rule Condition: Path ".*" (regex enabled)
- Custom Code "None-Sequential Javascript":

{% highlight javascript %}
if(typeof(Storage) != "undefined") {
  if (typeof(s) !== "undefined") {
    sessionStorage.pagename = s.pageName;
  }
}
{% endhighlight%}

Second PLR:

- Name: Save prior page name to prop
- Trigger Rule at "Bottom of Page"
- Rule Condition: Path ".*" (regex enabled)
- Rule Condition - Custom:

{% highlight javascript %}
if(typeof(Storage) !== "undefined") {
  if(typeof(sessionStorage.getItem("pagename")) === "string") {
    var prevPageName = sessionStorage.getItem("pagename");
    _satellite.setVar("previousPage",prevPageName);
    return true;
  }
}
{% endhighlight%}

- Adobe Analytics: prop2 set as %previousPage% //Or any other prop you want to use, just enable pathing on it.

There you go, the previous pageName is implemented.

### References
[Previous page name - apastebin](http://apastebin.tumblr.com/post/139627646180/adobe-analytics-dtm-previous-pagename)
