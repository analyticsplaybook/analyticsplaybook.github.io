---

layout: responsive
title: Set and record visitors type with Adobe DTM

---
# Set and record visitors type with Adobe DTM
Author: Till Büttner

__Problem__: 

As Analyst you want to segment the website visitors. For an eCommerce website you want perhaps to segment your visitors into prospects, leads and customers. But most times you just know the type, when a visitor purchase and gets a customer. 

__Actions__: 

We will set up a data element with custom script to identify the visitor type. The explanation will be split into a example with a full data layer and one without a data layer. The first approach was a little bit different to the result now. Significant impact gave a great article from Peter O'Neill in the L3 Analytics Blog entitled [Understanding your Website Visitors: Prospects vs CustomersUnderstanding your Website Visitors: Prospects vs Customers](http://www.l3analytics.com/2016/01/18/understanding-your-website-visitors-prospects-vs-customers/).

There is a lookup if we know something about the visitor:

- If the visitor purchase once, it's a customer. 
- If the visitor logged in, it's a lead. 
- If we don't know anything, it's a prospect.

The custom script uses localStorage and sessionStorage, not cookies. The difference? Cookies are primarily for reading server-side, local storage can only be read client-side. More to read: [What is the difference between localStorage, sessionStorage, session and cookies?](http://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies)

And please, if you are located in the EU, don't forgett about the [EU's cookie law](http://ec.europa.eu/ipg/basics/legal/cookies/index_en.htm). LocalStorage and sessionStorage are treated as cookies.

__Explanation (resolution)__: 

__Data layer available__: Let's begin by creating the data element. Name the data element like the value wich it will serve: visitorType. Switch the type to custom script and open the editor. Copy and paste the following javascript:

{% highlight javascript %}

if ((typeof(Storage) !== "undefined")&&(sessionStorage.ssVisitorType === undefined)) {
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
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

Save and close the editor and enter prospect as the default value. Remember this value for pageview and save the data element.

What is done here? We use conditional statements to prove if web storage is supported by the browser and if the sessionStorage for the visitor type isn't set. Afterwards we dig into the data layer and set the localStorage and sessionStorage to the equivalent visitor type. If there is no information available we look up if the localStorage is set and if not the visitor type will be prospect.

We need two more data elements. One named visitorTypeLogin and the other one named visitorTypePurchase.

Custom script for visitorTypeLogin, the default value is lead:

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

Custom script for visitorTypePurchase, the default value is customer:

{% highlight javascript %}

if (localStorage.lsVisitorType !== "customer")) {
	sessionStorage.setItem("ssVisitorType", "customer");
	localStorage.setItem("lsVisitorType", "customer");
}
return sessionStorage.getItem("ssVisitorType");

{% endhighlight%}

Both custom scripts do the same checks as the first one, just trimmed down to the needs.

After all this we just need three page load rules:

- one wich fits on every page, where the designated eVar is set to %visitorType%.
- one wich is loaded after login, where the designated eVar is set to %visitorTypeLogin%.
- and the last wich is loaded after purchase, where the designated eVar is set to %visitorTypePurchase%.

Thats it.

__Data layer not available__: There is just a change in the first two custom scripts and as you can imagine it is not so accurate.
Custom script for visitorType, the default value is prospect:

{% highlight javascript %}

if (localStorage.lsVisitorType !== undefined)) {
	sessionStorage.setItem("ssVisitorType", localStorage.getItem("lsVisitorType"));
} else {
	sessionStorage.setItem("ssVisitorType", "prospect");
	localStorage.setItem("lsVisitorType", "prospect");
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

The page load rules have to be the same.

__View last Words__: Please remember
> The first caveat is that this data will not be, and could never be, 100% accurate. For visitors who are not logged in, it is just not possible to know definitely whether they are an existing customer or a prospect. The accuracy will improve over time as customers return and can be identified as such even if not logged in. However, this does not work if they delete their cookie or use a different device. (Peter O'Neill in "Understanding your Website Visitors")

#### References
[Understanding your Website Visitors: Prospects vs CustomersUnderstanding your Website Visitors: Prospects vs Customers](http://www.l3analytics.com/2016/01/18/understanding-your-website-visitors-prospects-vs-customers/) <br>
[What is the difference between localStorage, sessionStorage, session and cookies?](http://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies) <br>
[EU's cookie law](http://ec.europa.eu/ipg/basics/legal/cookies/index_en.htm)