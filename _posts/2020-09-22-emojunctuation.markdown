---
layout: post
title:      "Emojunctuation"
date:       2020-09-23 02:09:44 +0000
permalink:  emojunctuation
---


I just finished a [project](https://github.com/stevenzych/tweet-sentiment) on natural language processing (NLP) that looked at *predicting the emotion of a tweet,* provided its word choice. Three options were made available: *Positive, negative, and no emotion.* With about nine-thousand tweets I trained the neural network, and the results were just alright, with a validation accuracy just shy of a measly 80%. Towards the end of the project--the time when this always tends to happen--I had an insight that I had probably scrubbed my data too thoroughly. Or at least that it was missing something vital--*emojis.*

As anyone who’s ever flirted over text knows, emojis (as well as their emoticon cousins and often punctuation) are a crucial ingredient in perceiving *affect,* or emotion, in writing. (The same premise applies to a passive aggressive stalemate. Did that period after “K” mean they’re mad at me? Why aren’t they sending smilies back? Etc.) And sure, word choice is vital too, but where words fail to cross that emotive gap left by digital distance, emojis and their pals tend to save the day.

## What’s An Emotion?

To my neural network, it wasn’t much more than a `0`, a `1`, or a `2`. Negative, nada, positive. But there’s so much more than that, and so much more that we convey with emojis, punctuation, and emoticons. Think for a moment how much the following message changes when you sub the emoji that’s there for one of these [👼, 🥵, 🥺, 🖕🏻]:

> Thanks for stopping by! 😀

You get the point. Only 5 variations on the same sentence and we have 5 different stories. Considering there’s about 1,500 base emojis, skin color variations, and combinations of these (just as we have combinations of words), things get limitless quick.

But I’m not breaking any new ground here. There’s documentation on this. There’s dissertations and whole sections of Wikipedia dedicated to it (even a section on [controversies and miscommunications](https://en.wikipedia.org/wiki/Emoji#Emoji_communication_problems), which is quite the read). There’s even [a deep learning AI called Dango](https://getdango.com/emoji-and-deep-learning/) that can produce extremely impressive context-specific strings of emojis on the fly. This is--you type “GTFO” and it offers emojis of two pointing fingers and a door--levels of specificity and awareness.

In short, it’s *not a leap* to get similar AI to predict emotion with more complexity, and with ever-increasing granularity in the text.

But what about punctuation? What about,, me typing like: this ? can a neural net Handle It?? [doEs iT sEnS e my SaRcAsM](https://knowyourmeme.com/memes/mocking-spongebob) ???

## Breaking Down

To all my fellow “millennials” (who were probably born late 90’s but got called one anyways) and gen-Z-ers, you know how much individual keystrokes can change a feeling. Throwing a space in before a period is different than just putting it there. Could I quantify it precisely? Maybe not. But do I feel a difference, even if only just stylistically? Absolutely.

What I would like, and what I’m not seeing a *ton,* of online, is neural network architecture aimed at assessing emotion in *punctuation* specifically (I’ve found [this example](https://www.aclweb.org/anthology/Y14-1047.pdf) that wasn’t locked behind an institutional paywall). I want a neural network that gets this granular, and I’m considering making one. Expect updates on this front 😈

