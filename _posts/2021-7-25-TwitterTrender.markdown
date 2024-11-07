---
layout: post
title:  "Making a Twitter bot"
date:   2021-7-25 11:30:54 -0800
categories: Engineering 
---

You know what's wrong with Twitter and the rest of social media? Not enough real people. 

Let's see how effective the most basic bot can be.

I wrote a super simple bot with the
[`python-twitter`](https://python-twitter.readthedocs.io/en/latest/installation.html)
and
[`markovify`](https://github.com/jsvine/markovify/tree/master/markovify)
packages. 

The bot takes the top 100 trending tweets, combines them into one with the markov chain, throws out profane results, and spits out the output. It automatically posts to twitter every 1-3 hours at random.

Here is what the output looks like:

```
This would be one of the city, and now one is going to spend the weekend together

Nothing but the O... O... O... Only the Olympics.

He said its boring, he said its boring, he said its boring, he said its no fun.

Why wait?

🇵🇭 Hidilyn Diaz reacts after placing first in the semi final E. She moves to Final E scheduled for Thursday 8:54am.

```

Complete garbage!

Over 300 views in the first two hours.

5000 within 24 hours.

I took the bot down.
