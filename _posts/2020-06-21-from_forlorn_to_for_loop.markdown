---
layout: post
title:      "From Forlorn To For Loop"
date:       2020-06-21 19:17:49 +0000
permalink:  from_forlorn_to_for_loop
---


## In The Beginning

Until recently, the **for loop and all its iterative glory** had evaded me. I understood that it was used for iteration, that it would greatly increase the speed and space-efficiency of my coding, and that it would be my bread-and-butter if I could master it... But, the finesse required to pull off tidy for loops over and over evaded me. I would write something. Test it. See that it works until hitting a certain index and breaking. Or, see that it works for my current situation but falls apart the moment its initial conditions are changed.

I needed to master this slippery fiend, and I needed to master it quick.

## Movies For Microsoft

Picture yourself in project week. Picture yourself bald, tired, and sweating over your laptop. Got it? Good--you're me now.

You've been assigned a simple task: Tell Microsoft (yes, *that* Microsoft) how to structure their new film studio. What movies should they make? What will be successful? What will bring in **da big bucks?**

Not only are you tasked with answering this question, but so are some thirty-odd other classmates. Knowing there's others pining for ol' M-soft's attention, you decide to take a strange approach. Sure we can look at general profit margins, what star actors and directors bring in big crowds, and what genres are typically most profitable but c'mon--everyone's asking these questions. What about **runtime?** What about **title length?** *How are these oft-overlooked factors a determinant in a movie's success?*

## The Bin Problem

Fast forward a day and you've cleaned your data. You're looking first at ROI (return on investment) and runtime. Your data has 191 unique time values, and that sounds clunky. Instead of doing a scatter plot (Sidenote: You already made one. It was less than perfect.) you've decided to make a bar chart showing what **groups** of runtimes do the best. **For this, you'll have to bin the data.**

To pick a meaningful bin size, let's settle on an easy-common denomination of time--30 minutes. We always talk in 30 minute chunks, we *know* what 30 minutes of movie feels like, so why not bin off that? Fair enough. We're gonna need to add a column to our dataset for `bin_list` and populate that with integers for whatever bin the film falls into. A 10min film (They exist!) falls into bin 1 (0-29min), a 45min film falls into bin 2 (30-59min), and so on.

But how will we go through our whole dataset and do this computation? You know the answer... **A for loop.** We'll have to iterate over each row, divide its time, and add its new value to `bin_list`. Here's how we do it:

```
bin_list = []
for i in range(len(df)):
    time = df['runtime_minutes'][i]
    bin_list.append((time//30)+1)
```

If you don't speak Python, let me explain what those lines are doing:
1. We create an empty list named `bin_list`. Our new group ID's live here.
2. We open the for loop, where `i` is any individual row, and `range(len(df))` is as many rows of data as we have. In other words, "Hey computer! Do this stuff one time for every row!"
3. Assign `time` to the runtime of any given film, accessed by repeating `i` as an index in our dataset.
4. Take that time from above, floor divide (`//`) it by 30 (divide by thirty, but drop any decimals), add 1 (I didn't want my bins to start at 0), and add (`append`) that new value to our list.

Oofta. **One loop down.** Now we can just tack this list on as a new column and voila, bins! But we're not done just yet, pardner. We've yet another for loop ton conquer--all we've done is group the data, we haven't *calculated* anything about these new groups.

## For Loop 2: Return Of The For Loop 3: Return Of The For Loop 4: Return Of The...

So what next? You decided earlier to calculate **ROI** and **runtime** in bins, and already handled the bins. Now, you need to calculate ROI *by bin.* But how? Make seven new tables--one for each bin--and do our calculations on those? Sounds cumbersome...

And it is! There's a tidier way to do this. And gosh will you feel smart when you figure it out. We're going to use another for loop and a little bit of tricky indexing to get the job done. But first, *how* will we get the job done without splitting up our dataset?

Like above, we'll be iterating through the dataset, but not row by row (I'll explain in a moment). The general idea, though, is the same: Iterate through the data, grab a value on each pass, and add a new value to our list, this time named `med_roi_by_bin`. Here's now:

```
med_roi_by_bin = []
for i in range(1,7):
    val = np.median(df[df['bin'] == i]['roi_w'])
    med_roi_by_bin.append(val)
```

Now again, for those not fluent in Python, here's an explanation:
1. We open a new list again. Ready and waiting to accept our **median ROI for all films in the same bin.**
2. We iterate through a range 1-7, where `i` is our location in that range, as opposed to individually by row. This is because *this operation* wouldn't work so smoothly by row. We want groups! Written like this we can...
3. Assign a new value `val` to the **median of each bin** with some slick indexing. Inside those median parentheses we're passing it a *version* of our dataset that's been culled out considerably. The `df['bin'] == i]` means we only want rows where the `bin` column has a value equal to `i`, the number iteration we're on. Tagging on `[roi_w]` means we only want the `roi_w` (worldwide ROI) column from those same-bin rows. We take that list of ROI's, hand it back to the `np.median()` function and voila! New value.
4. Add that value to our new list.

Whew, that's a lot happening in a little bit of space, but think of the time it saves! By creating this new list of median ROI's by bin using a for loop, we've cut the amount of work we had to do in... Half? Quarters? Into decimal slivers of what it would be manually? And think how easy it would be to repeat this with 8 bins, 20 bins, 1,000 bins!!! Simple as a couple clicks.

## In The End

I hope this journey was as fortuitous for you as it was for me, dear reader. It's not an expert-level crash course in for loops, but by God it sure got my head above water. Happy coding!
