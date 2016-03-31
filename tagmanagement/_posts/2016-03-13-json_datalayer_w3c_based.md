---

layout: responsive
title: JSON Data Layer based on W3C norm

---
# JSON Data Layer based on W3C norm
Author: Till Büttner

__Problem__: Quote from W3C Customer Experience Digital Data Layer:

>"Collection and analysis of visitor behavioral and demographic data has become an integral part of web application design and website success, whether accessed through browsers on laptop, mobile, kiosk, tablet or another device. This data is central to site performance analysis, dynamically tailoring site content to visitor activity and interest and retargeting visitors based on their behaviors.
>
>Increasingly, multiple vendors are involved in the data collection process for a given digital property, and each has a solution to be implemented on the page. As a result, page design has
become more complex and development cycles have lengthened as different requirements for data surfacing and formatting are added to the implementation process. Further, changing or
adding vendors sometimes requires that the development team change designs to accommodate vendor-specific requirements. Common data items must be continually surfaced in different ways, and each design requirement is a custom effort."<sup>1</sup> (p. 1)

__Actions__: To get rid of the standard way of collecting data from your website, you can use a structured data layer. For this JSON works well. JSON stands for _JavaScript Object Notation_
and is a lightweight data-interchange format. JSON uses JavaScript syntax, but the JSON format is text only.

__Explanation (resolution)__: First of all think about what kind of data your want to collect. Try to segment them in a way where they belong to.

* So the page name belongs to the page, same with referrer or server name.
* Price and product name belongs to the product.
* User State and ID belongs to the user.
* And so on ...

After this we have to divide these segments in a JSON object.

>"The JSO is designed to be contained within a root object called digitalData -- this is a matter of convenience and gives a common starting point. All other objects are sub-object from this root object. There is a pageInstanceID that is used to identify the page being measured within a unique environment -- development, staging, or production, for example. Beyond that, the specification includes sub-objects such as page, product, cart, transaction, event, component, user, privacyAccessCategories, and version for collecting different types of data in the JSO. (Additional objects can be added to digitalData as part of the extension mechanism.) Within the sub-objects, the specification defines a number of standard names for properties, while custom properties can also be added through an attributes object."<sup>1</sup> (p. 9)

Lets have a look at an example:

{% highlight javascript %}


digitalData="{
	"pageInstanceID":"Example Page:PROD",
	"page":{
		"pageInfo":{
			"pageName":"Example Page",
			"server":"www.example.com",
			"referrer":"www.digitalanalyticscookbook.org"
		},
		"attributes":{
			"country":"US",
			"language":"en-US",
		}
	},
	"product":[{
		"productInfo":{
			"productName":"Nikon SLR Camera",
			"sku":"sku12345",
			"manufacturer": "Nikon"
		},
		"category":{
			"primaryCategory": "Cameras"
		},
		"attributes":{
			"productType": "Special Offer"
		}
	}],
	"product":[{
		"productInfo":{
			"productName":"Canon SLR Camera",
			"sku":"sku67890",
			"manufacturer": "Canon"
		},
		"category":{
			"primaryCategory": "Cameras"
		},
		"attributes":{
			"productType": "Special Offer"
		}
	}],
}"
{% endhighlight%}

As you can see there is no limit having sub-objects in sub-objects. Only be aware of a straight JSON structure. If you have more than one sub-object with the same segment like a product, you have to use an array.

To get more ideas of what kind of sub-objects you can use, look at page 14 an following the the W3C Customer Experience Digital Data Layer.

To get the information out of the JSON, just use a JS object as for example `digitalData.page.pageInfo.pageName` is "Example Page" or `digitalData.product[2].productInfo.productName` is "Canon SLR Camera" in the data layer example above.

#### References
<sup>1</sup> [W3C Customer Experience Digital Data Layer](https://www.w3.org/2013/12/ceddl-201312.pdf)
