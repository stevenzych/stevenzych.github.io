---
layout: post
title:      "When The Water Wells Wither"
date:       2020-08-24 01:21:07 +0000
permalink:  when_the_water_wells_wither
---


Have you ever dug a well? I sure haven’t. And consequently, I probably couldn’t tell you when one was broken. Or about to break. Or fully functional for that matter.

But I have trained a [machine learning classifier](https://github.com/stevenzych/tanzanian_wells) to do just that, and to do it not-half-badly, might I add. Boasting a 77% accuracy and 72% recall for the categories that matter most, my submission to [this DrivenData contest](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/) looks at the functionality of over fifty-nine-thousand Tanzanian water wells. More specifically, it tries to ascertain what makes for a failing well, and predict which ones those are. In layperson’s terms: The computer brain says “Hey that well is probably broken, because it’s a lot like those other broken wells.”

> *The image below shows concentrations of different well statuses: Red is nonfunctional, blue is functional but needs repairs, and green is just plain ol’ functional.*

![Scatter Plot Of Wells In Tanzania](https://i.imgur.com/apHOq5m.jpg)

Now that’s all well and good, but what I found more interesting than the outcome of this project was some of the plots that came from EDA (exploratory data analysis) along the way. While investigating the different features of this dataset--`amount_tsh`, `region_code`, `water_quality`, and so on--I started sifting through all the redundant columns. Things like `waterpoint_type` and `waterpoint_type_group` would both be present, and this pattern held true for another key feature: `extraction_type`.

While trying to decide on which of the three “extraction” columns to keep (so as to maintain a less computationally-expensive machine learning model), I threw together a couple simple plots and found this:

![Countplot Of Extraction Types With Failure Rates](https://i.imgur.com/4bYkd2l.png)

Wells with extraction methods glossed as “other” tended to have a significantly higher percentage of nonfunctional wells than all other categories of extraction, by a factor of about 4x. What are these “other” extraction methods, you may ask? Well unfortunately, the metadata doesn’t say, and searching “other type of well extraction” online isn’t particularly fruitful.

Spurred on by this curiosity, I turned to other exploratory questions that *still* pertained to well failure in relationship to extraction type. But this time, I took a look at GPS height as well, another valuable feature from this dataset. The plot below shows a “violin” (that squiggly shape) for each extraction method, where the width of the violin indicates *more instances of nonfunctionality* at any given height (demarcated on the y-axis). With that being said, have a look:

![Violin Plot Of Well Failure](https://i.imgur.com/DsUbjDw.png)

Notice, first, that blue bulge on the bottom right: Gravity wells tend to fail when at lower altitudes. Why is that? I’m not sure. I’m a fledgling data scientist, not a hydraulic expert or a geologist. But interestingly enough, *rope pumps* tend to fail *way more* at the opposite end of the spectrum, way up there at 1250-1500 meters. Perhaps ice and frost and lower atmospheric pressure are particularly destructive to these rigs? Could be.

“But wait, wait, wait,” you’re saying. “Aren’t these just the *absolute* values of the failures? Maybe there’s just more rope pumps up high, and none down low. Couldn’t that plot be missing a skew in the data?”

It could be, but I have the plot to prove that wrong! Check it out:

![Violin Plot Of Well Success](https://i.imgur.com/59dM0kd.png)

Okay so sure, rope pumps don’t make it down to altitude 0. But their distribution of failures is significantly wider up top than the distribution of success is. The inverse statement is true for gravity pumps’ distributions around the 250 meter mark. And don’t even get me started on the imbalances between submersible pump success and failure at low altitudes!

To make this *even* clearer, I’ve made this last plot for you to feast your eyes on. Red is failing wells, green is functional ones. Whichever violin wing is wider at any given altitude is which category is “winning,” per se:

![Combined Violin Plot Of Well Success And Failure](https://i.imgur.com/KTXbAxz.png)

Hand, wind, and “other” pumps all have fairly stable relationships across the board, but gravity, submersible, motor, and rope pumps have relationships that are almost inverse. At least, there are areas that show significant disparity between functioning and nonfunctioning wells.

Now, if I were you I’d be looking for a clean explanation to all of this. Some tidy little bow on this infographic gift. Maybe a terse explanation of how air pressure, aquifers, and mountainous rainfall contribute to some specific… water conditions. But that’s not here. Just some lovely little graphs, some sort of testimony to how pretty basic EDA can be.

I’m going to keep digging (HA!) and if I find anything, you can bet I’ll be making a follow up post.

