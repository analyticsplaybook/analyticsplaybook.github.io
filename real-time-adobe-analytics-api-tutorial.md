---

layout: responsive
title: Real-Time Reporting Adobe Analytics API Tutorial

---
# Real-Time Reporting Adobe Analytics API Tutorial
Author: Ryan Praskievicz

**No prior API or programming knowledge is necessary to complete this tutorial.*

__Problem__: How to create a standalone Adobe Analytics real-time dashboard using the API? 


Adobe has a nice real-time dashboard built directly into Reports & Analytics (SiteCatalyst). It is easy to configure and get up an running quickly. But how do you create your own real-time dashboard outside of Adobe Analytics using the API? 

__Actions__: Use the lessons from an Adobe Summit lab session to walk-though how to create your own Adobe Analytics real-time dashboard. The dashboard uses the Adobe Analytics real-time API and D3.js for data visualization. When you are done with this tutorial your dashboard will show your company's data and look like the sample below. 

![Adobe Analytics Real-Time Dashboard](http://i2.wp.com/www.ryanpraski.com/wp-content/uploads/2016/01/adobe_analytics_real-time_dashboard_sample_.png)

![Adobe Analytics Real-Time Dashboard](images/adobe_analytics_real-time_dashboard_example.png)

__Explanation (resolution)__:

Start by [downloading the analytics realtime dashboard example](https://github.com/Adobe-Marketing-Cloud/analytics-realtime-dashboard-example/archive/master.zip) from GitHub. Find this in your downloads and unzip it. I unzipped the folder on my desktop for this example.

*I skipped Lesson 1-5 to keep the tutorial short. When you have time I highly recommend going through these lessons.* 

[Lesson 6](https://github.com/Adobe-Marketing-Cloud/analytics-realtime-dashboard-example/tree/master/lessons/lesson_6)

**Create a Snazzy Real-Time Adobe Analytics Dashboard Now**

1)  Update the config.js file in the analytics-realtime-dashboard-example-master that you downloaded above. The path to the file on my computer is: C:\Users\ryanpraski\Desktop\analytics-realtime-dashboard-example-master\js\config.js

Below is a sample config.js file. Make sure to update it with your credentials and save it on your computer.

    var config = {
       username:     "User Name:Company Name",
        secret:       "abcdefghijklmnopgrstuvwxyz",
        reportSuite:  "rsid",
        endpoint:     "api.omniture.com"
    };


2) Find the Lesson 6 folder on your computer. The path to the folder on my computer is: C:\Users\ryanpraski\Desktop\analytics-realtime-dashboard-example-master\lessons\lesson_6

3) Open the README file in the lesson_6 folder in your text editor or [on GitHub](https://github.com/Adobe-Marketing-Cloud/analytics-realtime-dashboard-example/tree/master/lessons/lesson_6).

4) Open the lesson_6.html file in your web browser and then open it a second time in a text editor. The path to the folder on my computer is: C:\Users\ryanpraski\Desktop\analytics-realtime-dashboard-example-master\lessons\lesson_6\lesson_6.html

5) When you load the lesson_6.html file in your browser you should see an animated number flash on the screen of total page views for the past 15 minutes in the top left, a continuously updating trend line with page views by minute for the past 15 minutes, and a table with the top 10 pages by page views for the last 15 minutes.

6) Follow the instructions in the lesson_6 README file to add the donut chart to your dashboard. If you have trouble getting the the donut chart included on your lesson_6.html file, you can open a complete version of the dashboard with the Lesson 6 code updates already done for you. The path to this complete “final dashboard” file on my computer is: C:\Users\ryanpraski\Desktop\analytics-realtime-dashboard-example-master\index.html

7) Your real-time dashboard should look like the screen shot below and will dynamically update with real-time Adobe Analytics data from the reporting API.

![Adobe Analytics Real-Time Dashboard](http://i2.wp.com/www.ryanpraski.com/wp-content/uploads/2016/01/adobe_analytics_real-time_dashboard_sample_.png)

8) If you’d like to create a different report that uses the same visualizations change the query parameters in the lesson_6.html or index.html file within the var params show in the code snippet below:


        var method ='Report.Run';
        //edit query here
        var params = { 
            "reportDescription":{
                "source": "realtime",
                "reportSuiteID": config.reportSuite,
                "metrics": [
                    { "id": "pageviews" }
                ], "elements": [
                    { "id": "page" }
                ],
                "dateFrom": "-15 minutes",
                "dateGranularity": "minute:1"
            }
        };

If you have any questions please see the references below or hit me up on Twitter [@ryanpraski](https://twitter.com/ryanpraski).

#### References

[Full Post on ryanpraski.com](http://www.ryanpraski.com/real-time-reporting-adobe-analytics-api-tutorial/)

[Adobe Analytics Real-Time Dashboard Example Lab Lesson on GitHub](https://github.com/Adobe-Marketing-Cloud/analytics-realtime-dashboard-example/tree/master/lessons)
