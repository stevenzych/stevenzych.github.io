---
layout: post
title:      "Dungeons And"
date:       2020-07-02 16:29:49 -0400
permalink:  dungeons_and
---

## ...Dragons?

If you're a programmer, there's a pretty high likelihood you play D&D. If not D&D, you might play one of its spiritual descendants. You like Skyrim? Dark Souls? Some other game with spells and swords and a skill tree? Odds are whatever game you're picturing was inspired by the OG roleplaying game--**Dungeons & Dragons.**

If you've got no idea what I'm talking about, D&D is a collaborative storytelling game where you sit around a table for hours with your friends. You throw dice, talk in silly voices, and pretend to be a ragtag band of heroes on some nonsense epic quest--it's like heaven.

Now the cool (read: fundamental) thing about D&D is that the culture of the game is *open-source.* Sure, there are rulebooks and official adventures published, but the *real* heart of the game is the rules you make up at the table to appease (and torment) your friends. Going on a quest through a snowy tundra to retrieve some mighty, lost artifact? Time to **homebrew** some rules that turn the *cold* into a *game-mechanic!* And the list goes on.

Recently, me and my players have gotten into **mass-combat,** having an *army* of heroes in your control as opposed to just one, and we needed a ruleset to facilitate this gameplay. Many headaches later and a simple system was settled on (thanks [Matt Colville](https://shop.mcdmproductions.com/products/strongholds-followers-pdf) for the inspiration), and I saw a situation for some Python.

## ...Data Sci?

In order to *have* mass-combat in our game, we first need to have **units**--a band of dwarven fighters, some elven archers, etc. But it's tedious to make them by hand, so instead I made a function. In all its glory, it looks like this:

```
def new_unit(name):
# Create base objects
    stat_list = ['Size', 'Attack', 'Defense', 'Range', 'Magic', 'Experience']
    stats = list(np.zeros(len(stat_list)))
    size_dict = {1: '1d4', 2: '1d6', 3: '1d8', 4: '1d10', 5: '1d12'}
    exp_dict = {1: 'Green', 2: 'Regular', 3: 'Seasoned', 4: 'Veteran', 5: 'Elite'}
    
# Get user inputs
    for i in range(len(stat_list)):
        stats[i] += int(input("{}: ".format(stat_list[i])))
    
# Calculate unit stats
    siz = size_dict[stats[0]]
    atk = stats[1] + stats[5]
    dfn = 10 + stats[2] + stats[0]//2 + stats[5]
    rng = stats[3] * 30
    mag = stats[4]
    exp = exp_dict[stats[5]]
    mdf = stats[4] + dfn//2
    
# Calculate cost
    cost = 100 * (stats[1] + stats[2] + stats[3] + 3*stats[4]) * (stats[5]) * (stats[0])
    
# Return stat block
    print(f'\n'
          f'\n{exp} {name}:'
          f'\n================'
          f'\n Size:    {siz}'
          f'\n Attack:  +{mt(atk)}'
          f'\n Defense: {mt(dfn)}'
          f'\n Range:   {mt(rng)} ft.'
          f'\n Magic:   +{mt(mag)}'
          f'\n================'
          f'\n Cost:    {mt(cost)} GP')
```

Now, that was a lot of code all at once, and I'm assuming you've just skimmed it. In short, here's what's happening:

1. The function `new_unit()` takes in a name as an argument,
2. asks the user for 6 inputs that determine the combat unit's in-game stats,
3. and returns a cleaned-up stat block.

Simple enough.

## The Spell Scroll Examined

Now, we're going to break apart this function in greater detail. To start, here's the top block of code:

```
def new_unit(name):
# Create base objects
    stat_list = ['Size', 'Attack', 'Defense', 'Range', 'Magic', 'Experience']
    stats = list(np.zeros(len(stat_list)))
    size_dict = {1: '1d4', 2: '1d6', 3: '1d8', 4: '1d10', 5: '1d12'}
    exp_dict = {1: 'Green', 2: 'Regular', 3: 'Seasoned', 4: 'Veteran', 5: 'Elite'}
```
	
The first line makes a list that contains all of our stats that can be *asked* for. (There's also a cost value assigned to units by the function, but the user doesn't set that.) The line after that opens a list of zeroes the length of our `stat_list`, for storing values as they're inputted. The last two lines are dictionaries for the how a unit's *size* corresponds to its *health,* shown here with polyhedral dice, and how its *experience* level corresponds to the title it's given.

```
# Get user inputs
    for i in range(len(stat_list)):
        stats[i] += int(input("{}: ".format(stat_list[i])))
```

Now we use a single for loop to ask the user for 6 values in a row. Each value is then stored in our stats list. We end up with something like `[2, 3, 5, 1, 0, 1]`.

```
# Calculate unit stats
    siz = size_dict[stats[0]]
    atk = stats[1] + stats[5]
    dfn = 10 + stats[2] + stats[0]//2 + stats[5]
    rng = stats[3] * 30
    mag = stats[4]
    exp = exp_dict[stats[5]]
    mdf = stats[4] + dfn//2
```

This block here takes each individual stat input and assigns it to a new variable (`siz`, `atk`, etc.) which will be presented to the user in the final format, and  used to calculate cost. Each operation is unique, and there was no simpler way to handle this, as with some form of iteration.

> For those curious D&D-types out there, the calculations being done are more-or-less arbitrary, with some nods to a standard character sheet. `dfn` is similar to D&D armor class, but calculated based on size and experience. `atk` resembles a standard melee attack, but the damage mod instead comes from experience (which is effectively proficiency here). And `rng`, for example, is just multiplied by 30 since it's a clean common movement-speed for a single creature in a single turn.

Anyways, more code:

```
# Calculate cost
    cost = 100 * (stats[1] + stats[2] + stats[3] + 3*stats[4]) * (stats[5]) * (stats[0])
```

Here we're calculating cost. Attack, defense, and range are all equally weighted stats. Magic costs *three times* as much as those stats since it's more powerful and specialized within the game world. Experience and size are multiplied across *all other* stat values (along with a coefficeint of 100, which just makes the final prices more believable). Increasing the total size of your army or training soldiers is significantly more costly than just giving them better swords, at least in my loose fantasy-inspired understanding of these things.

```
# Return stat block
    print(f'\n'
          f'\n{exp} {name}:'
          f'\n================'
          f'\n Size:    {siz}'
          f'\n Attack:  +{mt(atk)}'
          f'\n Defense: {mt(dfn)}'
          f'\n Range:   {mt(rng)} ft.'
          f'\n Magic:   +{mt(mag)}'
          f'\n================'
          f'\n Cost:    {mt(cost)} GP')
```

Lastly, we return the stat block as something that's *human-readable,* and *game-friendly.* (Note: `mt()` is just `math.trunc()`.) Here's an example of a finished stat block:

```
Veteran Warriors Of Lagathan:
================
 Size:    1d6
 Attack:  +8
 Defense: 18
 Range:   30 ft.
 Magic:   +0
================
 Cost:    6400 GP
```

## Onward, Hero!

Rejoice! You have now seen the function through to its completion! Feel free to drag this code into your favorite editor, or fork the repo [here](https://github.com/stevenzych/dnd_units). And of course, I'd be more than delighted to see anyone else's homebrew mods of this generator, or similar builds of any kind. Happy gaming!
