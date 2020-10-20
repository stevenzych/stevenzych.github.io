---
layout: post
title:      "Not Another Twitter Project: Fully-Automated Tweet Streaming And Outreach"
date:       2020-10-20 04:14:57 +0000
permalink:  not_another_twitter_project_fully-automated_tweet_streaming_and_outreach
---


So, I made a Twitter bot. Yes, another Twitter bot, another data science kid doing sentiment analysis for their final. **But,** this project ends in a **fully-automated livestream** that uses that *same* sentiment analysis to **start and manage conversations with Twitter users.** A little data-science-meets-the-app-world, if you will. This project in its entirety begin as my [capstone](https://github.com/stevenzych/capstone_project) at [Flatiron School](https://flatironschool.com/career-courses/data-science-bootcamp/online), but I’ve got some updates coming already. This blog will address what got us here.

## The Basics

To lay a little groundwork, I want to show you a flowchart I made to break down the project’s **moving parts.** Read from top-to-bottom, it shows the process of livestreaming “acceptable” tweets (in this case I was pulling anything about Amazon, since they’re talked about a ton), checking for both sentiment and the user’s follower count, and then automating an interaction in their DM’s.

![Flowchart](https://i.imgur.com/JyqRvkc.jpg)

Behind the scenes there’s three key players:

- The Twitter stream
- The neural network
- The chatbot

And I’ve got a section for each of them to show you *exactly* what’s going on. And don’t worry, I’ll keep the code brief. Anyone who wants to see the nitty-gritty can dig into the full repo on GitHub [here](https://github.com/stevenzych/capstone_project).

## The Streamer

The Twitter stream’s job is simple. It sees *everything*--and I mean ***everything***--that’s getting tweeted right now. (God forgive my eyes for the things I’ve seen in data cleaning.) I come into the picture to reign the stream in. I pass some parameters, some wants and don’t-wants and end up with a narrow slice of all the data being made. Instead of getting *all* tweets being made live, I set it up to **only** get tweets about `”@amazon”` that were *not* retweets and *were* in English. The gist of this looks like:

```
stream.statuses.filter(language = 'en',
                       track = '@amazon',
                       tweet_mode = 'extended')
```

From here, and well out of sight, the stream starts **making decisions,** and it starts **calling on its friends:** The neural net, and the chatbot. The first call is to the neural net.

## The Predictor

A [neural network](https://en.wikipedia.org/wiki/Neural_network), for the unfamiliar, can be thought of like a computer brain. *Ex Machina*, Cortana from the *Halo* games, pick a sentient sci-fi computer and you’re halfway there. In *this case,* a neural network is being used to **predict sentiment.** In other words, it can read a tweet that’s been transformed into computer-friendly inputs and go “Oh! This one’s positive!” or “Yikes, negative…” It’s this deciding power that makes a neural network absolutely **vital** for this whole thing to work. 

Now remember when I said the stream is making a call to the neural net? Here’s how that happens: The stream sees a tweet about Amazon (again, just using them as an example), and *also* sees that it was tweeted by someone with a good number of followers. An **opportunity** for brand outreach! But was this person speaking positively? We’re unlikely to be successful in striking some sort of partnership if not, so we better double check. In short, we send the neural net the message and say “Looks good?” The neural net ideally says “Looks good 😎” and we carry on with the process, by initiating conversation! The lines that actually do this look like:

```
if tweet_sentiment(data['text']) == 3:
    c = Conversation(user_id = data['id'], t = t)
```

In human speak: “If the sentiment of the tweet is positive, start a conversation with the user who tweeted it, and name the conversation **c**.”

## The Talker

Alright, we’re in. At this point, we’re in the green section from the image above, and **the chatbot will start DM’ing the human user.** A function called `greet()` produces a message like this:

![Greeting](https://i.imgur.com/E1QNHaf.png)

And then **the bot waits** (very respectfully) for about two minutes for them to reply. The bot will then check if there’s any new messages and, if there are, add them to its own personal list of messages from that user! From there, the bot can **respond** to one of the hard-coded options listed in the greeting text like this:

![Yes Reply](https://i.imgur.com/U1Ymffd.png)

Or **offer a suggestion** if the user is stuck or being difficult:

![Help](https://i.imgur.com/QNI11U0.png)

Ideally it’s the first that happens, where the user says `”YES”` out the gate and a human is contacted to handle the specifics of any brand-deal or money negotiations. (I wouldn’t trust a bot to do that, would you?) If this happens, or if the user asks for `”HELP”` three times, or if they reply with `”STOP”`, their conversation ends and the stream moves on.

This process repeats effectively forever, slowly aggregating new potential contacts for outreach, without nearly *any* need for human input on the application side. For the foreseeable future I’m working on executing a bunch of these streams and conversations *asynchronously.* Or rather, I’m working on having a whole fleet of them running at once to *maximize opportunities.*

## The Use Cases

**So is this useful?** If you want to cut the work needed to initiate brand outreach and find ambassadors, spokespersons, potential avenues for advertising, and so on, then **absolutely.** But it doesn’t *have* to be a monetary incentive that fuels this process.

The *same exact* protocols could be used for **political** and/or **social services outreach.** You could re-train the neural network to look for tweets with a certain political sentiment and then send messages with links to resources that align with a campaign. Similarly, you could use a re-trained NN to distribute user-specific housing resources based on location. The list goes on, and on, and on. The business-y framing was just easiest to get data for under the time crunch of a capstone.

In closing, I hope I’ve proved to you the value (and honestly just the sheer coolness) that using a neural network coupled with some Twitter functionality can bring to any project that includes outreach. Be it for social justice, political, or economic reasons, I encourage you to explore this. And more than that, I encourage you to reach out to me on [LinkedIn](https://www.linkedin.com/in/steven-zych-973466157/) or [GitHub](https://github.com/stevenzych). Thanks :)

