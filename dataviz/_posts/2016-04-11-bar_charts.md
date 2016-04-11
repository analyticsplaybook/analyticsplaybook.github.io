---
layout: responsive
title: Making Great Bar Charts In R

---

#Making Great Bar Charts In R

Author: Jowanza Joseph

**Problem:** As data collection and aggregation grows in an organization, presenting and consuming that data can become a real challenge. A good foundation in visualization eases this burden. Using bar charts effectively can help with sw

**Actions:** We will walk through an example of how to create a useful and beautiful barchart in R. 

###Example

In R, the library ggplot2 is your best friend when it comes to creatingstatic bar charts. You can create a simple one with the following code:

{% highlight r%}
library(ggplot2)

ggplot(mpg, aes(class, fill = class)) + geom_bar()

{% endhighlight %}
![image](http://i.imgur.com/ZPrONaq.png)

This bar chart is simple and effect but we can do better. We can start simple by adding titles.

{% highlight r%}
ggplot(mpg, aes(class, fill = class)) + geom_bar() +
  labs(title = "Count of Car Classes MPG Dataset", x = "Car Class", y = "Count")
{% endhighlight %}

![image](http://i.imgur.com/56OPrfU.png)

The background space in this chart isn't really pleasing to the eyes. We can make a customize the chart and make it all grey for effect:

{% highlight r%}
library(ggplot2)
library(scales)
library(grid) 
library(RColorBrewer)
library(extrafont)
library(reshape)
library(ggthemes)

blogData = mpg

palette <- brewer.pal("Greys", n=9)
color.background = palette[2]

ggplot(mpg, aes(class, fill = class)) + geom_bar() +
  labs(title = "Count of Car Classes MPG Dataset", x = "Car Class", y = "Count")+
  theme_bw(base_size=9)+
  theme(panel.background=element_rect(fill=color.background, color=color.background)) +
  theme(plot.background=element_rect(fill=color.background, color=color.background)) +
  theme(panel.border=element_rect(color=color.background))+
  theme(legend.background = element_rect(fill=color.background))
{% endhighlight %}

![image](https://i.imgur.com/dQ3YlQZ.png)

A few more modifications can be made. 

1. Sort by count
2. Flip the axis 
3. Overlay the count
4. Change the font of the chart
5. Change the opacity of the bars 
6. Remove the legend (doesn't add any value)
7. Some other aesthetic fixes

{% highlight r%}
library(ggplot2)
library(scales)
library(grid) 
library(RColorBrewer)
library(extrafont)
library(reshape)
library(ggthemes)
library(dplyr)

blogData = mpg

palette <- brewer.pal("Greys", n=9)
color.background = palette[2]

newMPG <- group_by(mpg,class) %>%
  summarise(total = n()) %>%
  arrange(desc(total))

ggplot(newMPG, aes(x=reorder(class, +total), total)) + coord_flip()+ geom_bar(fill = "#6f00ff", stat="identity") +
  labs(title = "Count of Car Classes", x = "Car Class", y = "Count")+
  geom_text(aes(y=total - 1, label=total), position = position_dodge(.09), vjust= .5, size = 3, hjust = 1.3, fontface= "bold", family = "Hack")+
  my_theme()
  
my_theme <- function() {
  
  # Generate the colors for the chart procedurally with RColorBrewer
  palette <- brewer.pal("Greys", n=9)
  color.background = palette[2]
  color.grid.major = palette[3]
  color.axis.text = palette[6]
  color.axis.title = palette[7]
  color.title = palette[9]
  
  # Begin construction of chart
  theme_bw(base_size=9) +
    
    # Set the entire chart region to a light gray color
    theme(panel.background=element_rect(fill=color.background, color=color.background)) +
    theme(plot.background=element_rect(fill=color.background, color=color.background)) +
    theme(panel.border=element_rect(color=color.background)) +
    
    
    # Format the grid
    theme(panel.grid.major=element_line(color=color.grid.major,size=.25)) +
    theme(panel.grid.minor=element_blank()) +
    theme(axis.ticks=element_blank()) +
    
    
    # Format the legend, but hide by default
    theme(legend.position="none") +
    theme(legend.background = element_rect(fill=color.background)) +
    theme(legend.text = element_text(size=7,color=color.axis.title)) +
    
    
    # Set title and axis labels, and format these and tick marks
    theme(plot.title=element_text(color=color.title, size=15, vjust=1.25,face="bold", family="Hack")) +
    theme(axis.text.x=element_text(size=10,color=color.axis.text)) +
    theme(axis.text.y=element_text(size=10,color=color.axis.text)) +
    theme(axis.title.x=element_text(size=10,color=color.axis.title, vjust=0, face="bold", family= "Hack")) +
    theme(axis.title.y=element_text(size=10,color=color.axis.title, vjust=1.25, face="bold", family = "Hack")) + 
    theme(strip.text.y = element_text(size = 10, colour = "black", face="bold", family = "Hack")) +
    theme(strip.background = element_rect(fill=color.background, color=color.background, size=1)) +
    
    # Plot margins
    theme(plot.margin = unit(c(0.35, 0.2, 0.3, 0.35), "cm"))
}
{% endhighlight %}
![image](http://i.imgur.com/DOnynEA.png)

Now we have a chart worth sharing.


#### References:

[ggplot2](http://docs.ggplot2.org/current/)

[Max Woolf](http://minimaxir.com/2015/02/ggplot-tutorial/)


