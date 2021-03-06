---
layout: post
title: Visualizing Twitter data with Bokeh
published: true
---

First, I made a script which merged the separate csv files (from the FiveThirtyEight dataset) into a single csv file. For these simple visualizations, I left out some of the columns from the original data: all columns with only urls were left out, a binary indicator `new_june_2018` and `harvested_date` (a column that included information on the date and time the tweet was collected by Social Studio) were also left out. Id column `tweet_id` was left out. `publish_date` column was converted into datetime format.

Now the `tweets` dataframe has 13 columns and 5 892 414 rows in total. If we explore only English tweets, there would be 4 233 734 tweets in the dataframe. By looking the columns of the dataframe, it would be interesting to see for example, what kind of update activity amount has the highest representation of unique handles. For observing the update activity (the number of "update actions" on the account that authored the tweet, including tweets, retweets and likes) of unique Twitter handles, I used `groupby` for `alt_external_id` (because the number of unique Twitter handles were pretty close to ones reported by Linvill & Warren) which will be the index and for each lat external id I got a single column of list of all updates by applying lambda function to `updates` column, collecting the unique updates. Then I created a list where I collected all the maximum update values that each external author id had and visualized that as a histogram:


<iframe width="1000" height="500" frameborder="0" scrolling="no" src="../graphs/histo_with_alt_external_id.html"></iframe>

*Which bin of total update actions has the highest representation of unique Twitter handles? Are there more low activity accounts than high activity accounts in the dataset? Can we spot some interesting patterns or anomalies when it comes to update activity?*

Since the histogram has bins of 200 (from 0 to 200, 200 to 400 and so on), we'll notice that there is higher representation of low activity accounts, but also separate accounts with very high amount of update actions: the highest representation of English-tweeting unique handles (303) is around 400-600 update actions, but there are also single handles with large amount of update actions (166113). The mean of updates was 11 293 with standard deviation of 19 141. Handles that tweeted in English had over 2 million updates (2 116 867) in total.

We can now take a look at the NBC News dataset. We can make a dataframe out of `users.csv` file and check for example what kind of tweeting (including retweeting) behaviour unique English-tweeting handles exhibit: are there more users who make 0 to 6000 tweets or retweets or more users with for example over 30 000 tweets or retweets? Let's limit our dataframe to only English tweets and do same as we did above, but for counting (max) `statuses_count` for each user id. Then we can visualize the result as histogram:

<iframe width="1000" height="500" frameborder="0" scrolling="no" src="../graphs/histogram_twitter_NBC.html"></iframe>

We'll notice that with the smaller NBC News dataset there is similar trend going on: most of the handles seem to make less than 6000 tweets or retweets, whereas single accounts seem to tweet or retweet much more.
