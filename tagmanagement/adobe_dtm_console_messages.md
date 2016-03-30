---

layout: responsive
title: Adobe DTM - Using Browser Console Messages

---
# Adobe DTM - Using Browser Console Messages
Author: Jason Thompson

__Problem__: Error messaging, or messaging in general, is often overlooked in the development process causing difficulties in debugging and managing code deployed by a TMS.

__Actions__: You can leverage a function called _satellite.notify(), a cross browser supported function, contained within DTM to send informative messages to the browser console. This is especially helpful when deploying 3rd party scripts. 

__Explanation (resolution)__:

At it's most basic level, _satellite.notify() takes two arguments to send a message to the browser console:

- Descriptive message text
- Numeric level of intensity

Let's use a working example to understand how we can use _satellite.notify() to send messages to the browser about the status of a 3rd party marketing pixel. 

**Intensity 1 - A message that the pixel fired**
_satellite.notify("DoubleClick tag fired.", 1);
![DTM Level 1 Notification](dtm_notify_1.png)

**Intensity 3 - A message about the end of the campaign**
_satellite.notify("The DoubleClick campaign ends in 5 days.", 3);
![DTM Level 3 Notification](dtm_notify_3.png)

**Intensity 4 - A message that the campaign has ended**
_satellite.notify("The DoubleClick campaign has ended. Please remove the pixel from Adobe DTM.", 4);
![DTM Level 4 Notification](dtm_notify_4.png)

**Intensity 5 - A message alerting a critical issue**
_satellite.notify("The DoubleClick servers are taking longer than 3 seconds to respond.", 5);
![DTM Level 5 Notification](dtm_notify_5.png)

#### References
[Avoid the Marketing Pixel Wasteland](http://33sticks.com/avoid-the-marketing-pixel-wasteland/)
