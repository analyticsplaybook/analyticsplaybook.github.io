---

layout: post
title: Adobe DTM - Set and Record Visitor type
date:   2016-03-13
author: Till Buettner
category: tagmanagement
tags:
  - adobedtm
  - tracking
  - adobeanalytics
  - tms

---

### Problem

As an analyst you want to segment website visitors. For an eCommerce website, perhaps you want to segment your visitors into prospects, leads and customers. But many times, you just know the type, if a visitor purchased (and thus, becomes a "customer").

### Actions

We will set up a data element using custom JavaScript to identify the visitor type. The explanation will be split into a example with a full data layer and one without a data layer. The first approach was a little bit different to the result now. Peter O'Neill outlines this technique in a great L3 Analytics Blog post titled [Understanding your Website Visitors: Prospects vs Customers](http://www.l3analytics.com/2016/01/18/understanding-your-website-visitors-prospects-vs-customers/).

Here is our visitor behavior hierarchy:

- If the visitor has purchased, they are a customer
- Otherwise, if the visitor is logged in, they are a lead
- Otherwise, if we don't know anything, they are a prospect.

This custom script uses `localStorage` and `sessionStorage`, not cookies. What's the difference? Cookies are primarily for reading server-side, whereas local storage can only be read client-side. More information: [What is the difference between localStorage, sessionStorage, session and cookies?](http://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies) And, if you are located in the EU, don't forgett about the [EU's cookie law](http://ec.europa.eu/ipg/basics/legal/cookies/index_en.htm). LocalStorage and sessionStorage are treated as cookies.

### Explanation (resolution)

#### Data layer available
Let's begin by creating the data element. Name the data element in reference to the value which it will serve: visitorType. Copy and paste the following javascript into your data layer code:

{% highlight javascript %}

if (typeof(Storage) !== "undefined") {
	if(sessionStorage.ssVisitorType === undefined) {
		if (digitalData.user.segment.loggedIn === "yes") {
			if (digitalData.user.segment.customer === "yes") {
				sessionStorage.setItem("ssVisitorType", "customer");
				localStorage.setItem("lsVisitorType", "customer");
			} else {
				sessionStorage.setItem("ssVisitorType", "lead");
				localStorage.setItem("lsVisitorType", "lead");
			}
		} else {
			if (localStorage.lsVisitorType !== undefined)) {
				sessionStorage.setItem("ssVisitorType", localStorage.getItem("lsVisitorType"));
			} else {
				sessionStorage.setItem("ssVisitorType", "prospect");
				localStorage.setItem("lsVisitorType", "prospect");
			}
		}
	} else {
		if (localStorage.lsVisitorType === undefined) {
			localStorage.setItem("lsVisitorType", sessionStorage.getItem("ssVisitorType"));
		}
	}
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

In this code snippet, prospect is the default value (i.e. the absence of other information to place the user higher up on the hierarchy). Then, remember this value for a pageview and save the data element.

We use conditional statements to determine if web storage is supported by the browser and if the sessionStorage for the visitor type isn't set. If so, we dig into the data layer and set the localStorage and sessionStorage to the equivalent visitor type. If there is no information available, we look up if the localStorage is set and if not the visitor type will be `prospect`.

We can also create two more data elements: visitorTypeLogin and visitorTypePurchase.

Here is the custom script for visitorTypeLogin, where the default value is `lead`:

{% highlight javascript %}

if (localStorage.lsVisitorType === "prospect")) {
	if (digitalData.user.segment.customer === "yes") {
		sessionStorage.setItem("ssVisitorType", "customer");
		localStorage.setItem("lsVisitorType", "customer");
	} else {
		sessionStorage.setItem("ssVisitorType", "lead");
		localStorage.setItem("lsVisitorType", "lead");
	}
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

Custom script for visitorTypePurchase, the default value is `customer`:

{% highlight javascript %}

if (localStorage.lsVisitorType !== "customer")) {
	sessionStorage.setItem("ssVisitorType", "customer");
	localStorage.setItem("lsVisitorType", "customer");
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

All custom scripts basically do the same thing, with slightly different business logic.

After this, we create three page load rules:

1) On every page, an eVar is set to %visitorType%. (It's also possible to track this in the Adobe DTM properties, so then you wouldn't even need an extra page load rule).
2) After a login, the eVar is set to %visitorTypeLogin%.
3) After a purchase, where the eVar is set to %visitorTypePurchase%.

The conditions should be evaluated after the data layer exists in the DOM. As an example, if the data layer is loaded after the opening `<body>` tag, the rule should be triggered at the bottom of page. If you're using Adobe Analytics call, the tracking script should be after these page load rules to avoid two pageviews.

#### Data layer not available
If a data layer is not available, there is a slight change in the first two custom scripts and as you can imagine it will make tracking less accurate.

Custom script for visitorType, the default value is prospect:

{% highlight javascript %}

if (typeof(Storage) !== "undefined") {
	if (sessionStorage.ssVisitorType === undefined) {
		if (localStorage.lsVisitorType !== undefined) {
			sessionStorage.setItem("ssVisitorType", localStorage.getItem("lsVisitorType"));
		} else {
			sessionStorage.setItem("ssVisitorType", "prospect");
			localStorage.setItem("lsVisitorType", "prospect");
		}
	} else {
		if (localStorage.lsVisitorType === undefined) {
			localStorage.setItem("lsVisitorType", sessionStorage.getItem("ssVisitorType"));
		}
	}
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

Custom script for visitorTypeLogin, the default value is lead:

{% highlight javascript %}

if (localStorage.lsVisitorType === "prospect")) {
	sessionStorage.setItem("ssVisitorType", "lead");
	localStorage.setItem("lsVisitorType", "lead");
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

The page load rules in your tag manager would be the same.

#### View last Words
Please remember:

> The first caveat is that this data will not be, and could never be, 100% accurate. For visitors who are not logged in, it is just not possible to know definitely whether they are an existing customer or a prospect. Your accuracy will improve over time as customers return and can be identified, even if they are not logged in. However, this does not work if they delete their cookie or use a different device. (see Peter O'Neill in "Understanding your Website Visitors")

### References
[Understanding your Website Visitors: Prospects vs CustomersUnderstanding your Website Visitors: Prospects vs Customers](http://www.l3analytics.com/2016/01/18/understanding-your-website-visitors-prospects-vs-customers/) <br>
[What is the difference between localStorage, sessionStorage, session and cookies?](http://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies) <br>
[EU's cookie law](http://ec.europa.eu/ipg/basics/legal/cookies/index_en.htm)
