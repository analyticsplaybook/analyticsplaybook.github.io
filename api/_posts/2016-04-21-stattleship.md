---

layout: post
title: Stop Scraping - Analyzing Sports Data Using Stattleship
date:   2016-04-20
author: Tanya Cashorali
category: api
tags:
  - api
  - R
  - Sports

---

### Problem

You love sports. You love data. If you've ever gone on an epic journey in search of sports data, you've probably resorted to scraping data from sites like [ESPN](www.ESPN.com) or [Baseball Reference](http://www.baseball-reference.com/) and have spent countless hours writing Python code to use the powerful web scraping library [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/). Maybe you even have a nice [Python client](https://github.com/seemethere/nba_py) that scrapes [stats.nba.com](http://stats.nba.com) or you wrote an [R package](https://github.com/BillPetti/baseballr) to scrape Baseball Reference.

However, we all know that websites get redesigned, formats change and sometimes access to specific stats is revoked (e.g. the NBA did this with [player movement tracking](http://stats.nba.com/help/whatsnew/) but attributes it to 'technical difficulties'). Suddenly, you are scrambling to update your scraping code in order to account for a few new divs or other elements on the web page that were renamed.  

### Actions
This is where the Stattleship API comes to the rescue. We've built an easy-to-access set of sports data, stats and accomplishments for multiple sports (and expanding). We've partnered with Gracenote to provide all of our NFL, NBA, NHL and MLB game data. Our service is designed for creative fans who want to use Stattleship data to build sports apps that scale.

We've cleaned, structured, and categorized all of the box score, player stats and game log data for you. We've even quantified specific performances and feats so that you can quickly identify whether a stat is a common-place occurrence or a record-breaking achievement.

In this tutorial, we will show you how to use our R wrapper to access live and up-to-date sports data via the Stattleship API. There is also a Ruby gem for all of you Rubyists: [https://github.com/stattleship/stattleship-ruby](https://github.com/stattleship/stattleship-ruby)

### Example
There are a few main endpoints that we will focus on. They are as follows:

* <b>games</b> - <i>contains game information such as date, attendance, scoreline, etc.</i>
* <b>players</b>- <i>contains player information such as name, position, draft round, school, salary, weight, birthday, etc.</i>
* <b>teams</b> - <i>contains team information such as name, division, colors, hashtags, etc.</i>
* <b>game_logs</b> - <i>contains player-level game log information such as Kevin Durant's assists from a specific game, total minutes played, etc.</i>
* <b>team_game_logs</b> - <i>contains team-level game log information such as total points, rebounds and turnovers by the Boston Celtics in a specific game</i>

Here's a basic Entity-Relationship Diagram outlining how these objects relate to one another:

![](/images/stattleship_erd.png)

In order to get started, sign up for a free API token from [here](https://www.stattleship.com). Now load up [RStudio](https://www.rstudio.com/) and start following along! The first few lines of this R script will install the [R package](https://github.com/stattleship/stattleship-r) from Github to your machine:

(If you don't have `devtools` installed you will have to install that first.)

{% highlight r %}
install.packages("devtools")
devtools::install_github("stattleship/stattleship-r")

## Load the stattleshipR package
library(stattleshipR)
{% endhighlight %}

Next you need to set your API token in the R environment:
{% highlight r %}
## Get your free token from www.stattleship.com
set_token("your-API-token-goes-here")
{% endhighlight %}

Now we need to specify the sport, league and endpoint we are interested in. In this case we will fetch all MLB regular season game logs for the Red Sox to date. This is as easy as setting three parameters: `sport`, `league` and `ep` for endpoint.

{% highlight r %}
sport <- 'baseball'
league <- 'mlb'
ep <- 'game_logs'
{% endhighlight %}

This last parameter, called `q_body` is where we can set more granular options such as requesting a specific team, stat, player or season.

{% highlight r %}
q_body <- list(team_id='mlb-bos', status='ended', interval_type='regularseason')
{% endhighlight %}

Now that all of our parameters are set, we can use `ss_get_result` to send our request to the Stattleship API. Notice the `walk=TRUE` option. This ensures that the request will walk through all pages of results and return everything. Results are returned 40 rows per page.

{% highlight r %}
gls <- ss_get_result(sport=sport, league=league, ep=ep, query=q_body, walk=TRUE)  
{% endhighlight %}

We now have a list in R that has 5 elements (there were 5 pages of results returned).

{% highlight r %}
> length(gls)
[1] 5
{% endhighlight %}

We want to combine all of the pages of results into one `data.frame`:

{% highlight r %}
game_logs<-do.call('rbind', lapply(gls, function(x) x$game_logs))  
{% endhighlight %}

Let's check out all of the data we now have access to from yesterday's games:

{% highlight r %}
colnames(game_logs)        
{% endhighlight %}

![](/images/Screen-Shot-2016-04-19-at-2-52-14-PM-1.png)

Wow, over 80 variables for each player including strikeouts, walks, doubles, runs, and more.

I want to include player information into this data set though so I have more than just `player_ids`. Let's retrieve all Boston Red Sox players by changing the `ep` to `players` and pass `team_id='mlb-bos` into the options list.

{% highlight r %}
sport <- 'baseball'  
league <- 'mlb'  
ep <- 'players'  
q_body <- list(team_id='mlb-bos')

pls <- ss_get_result(sport=sport, league=league, ep=ep, query=q_body, walk=TRUE)  
players<-do.call('rbind', lapply(pls, function(x) x$players))
{% endhighlight %}

In order to merge the two data.frames we can simply rename the `id` column to `player_id` so we can use `merge` in R like this:

{% highlight r %}
colnames(players)[1] <- 'player_id'
game_logs <- merge(players, game_logs, by='player_id')
{% endhighlight %}

Now let's do some basic player calculations using `dplyr`.

{% highlight r %}
install.packages('dplyr')
library(dplyr)

## Only include game_logs where a player actually played the game, calculate mean batting average,
## total runs, total bases and pull in salary info
stats <-
  game_logs %>%
  filter(game_played == TRUE) %>%
  group_by(name) %>%
  summarise(totalRuns = sum(runs), meanBA = mean(batting_average), totalBases=sum(total_bases), salary=max(salary))
{% endhighlight %}

![](/images/Screen-Shot-2016-04-19-at-3-24-14-PM.png)

Good! Now let's plot it all using `ggplot2`.

{% highlight r %}
ggplot(stats, aes(x=totalRuns, y=meanBA, size=totalBases, label=name, color=salary)) + geom_text()
{% endhighlight %}

This plot actually enables us to visualize 4 different variables at once. The x and y axes display total runs and mean batting average, the size of the labels indicate how many total bases the player has had, and the color indicates salary. Xander Bogaerts and Travis Shaw are looking like quite a bargain at this point in the season already.
![](/images/redsox.png)

Let us know if you build anything cool with this awesome sports data API! We also welcome contributions to our R wrapper via [pull requests in Github](https://github.com/stattleship/stattleship-r). We have a [public Slack channel](https://fanboat.stattleship.com/) as well where you can join us to talk sports, data, get R or Stattleship API help, and provide feedback.



### References / Other Examples

* [Full R script from this post](https://gist.github.com/tcash21/3a3b103386e1115aee6aa6ea6add072a)
* [Stattleship API documentation](developers.stattleship.com)
* [Oglethorpe, stat infographics otherwise known as StatShots](https://oglethorpe.stattleship.com/photos?photo_type=CurrentWinningStreakPhoto)
* [NFL Yardage by Team 2015](http://104.131.198.150:3838/statship-app/)
* [Fanduel NBA Optimizer](https://tcb-analytics.shinyapps.io/dfsoptimize/)
* [Glickman Statmoji slackbot](https://github.com/stattleship/glickman)
* [Stattleship Blog](http://blog.stattleship.com)
