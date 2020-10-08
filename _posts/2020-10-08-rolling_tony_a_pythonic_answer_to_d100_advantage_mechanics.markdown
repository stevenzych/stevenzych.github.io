---
layout: post
title:      "Rolling Tony: A Pythonic Answer To d100 Advantage Mechanics"
date:       2020-10-08 23:09:36 +0000
permalink:  rolling_tony_a_pythonic_answer_to_d100_advantage_mechanics
---


If you’ve played any table-top roleplaying games (TTRPG’s), it’s likely they were built around one of two **fundamental dice mechanics:** You were rolling a **d20** (a 20-sided die) or a **d100** (100-sided, you get it). If you were rolling that sweet, sweet d20, it’s likely it was in the context of **Dungeons & Dragons,** the world’s foremost roleplaying game. If it was a d100, well, my guess is gonna be less accurate but I’d wager it was either [**Call of Cthulhu**](https://www.chaosium.com/call-of-cthulhu-rpg/) or [**Mythras.**](https://www.drivethrurpg.com/product/191475/Mythras) At least, those are the two d100 systems I’ve rolled with.

And if you *have* played one of these games, you’re probably familiar with **”advantage.”** It’s handled differently in every system, but the general idea is this: *You, the player--your character really--are in an *advantageous* situation, and your dice rolls oughta reflect that.* Attacking an enemy from behind cover? Advantage. Tricking a noble into an unfavorable deal while she’s only half-awake? Advantage. The same goes for the opposite--Got crap luck and nead some dice rolls to reinforce that? Meet the cousin: *Dis*advantage!

## Rolling With Advantage

**In mechanical terms** (at least in 5th edition D&D), this is made manifest by making **two** rolls of the same nature (you roll twice to swing your sword, twice to sneak past that sleeping guard, etc.) and **take the better roll.**

> Example: Ol’ Johnny the wizard wants to launch a fireball at that enemy monster while it’s distracted eating his friend. The gamemaster determines Johnny has advantage. Johnny rolls a d20 *twice* to hit with his spell. He gets a `19` and a measly `7`. The `7` is tossed out as if it never happened. The `19` is kept as the “true” roll, the fireball connects, and the heroes beat the hideous beast. Huzzah!

We immediately see the benefit here. Two outcomes were presented: One were Johnny’s friend could’ve died, and another where Johnny saves all the day. But how do we quantify this advantage? What is the *numerical significance* of getting advantage on a d20 roll?

Lucky for us, [this has been studied before](https://statmodeling.stat.columbia.edu/2014/07/12/dnd-5e-advantage-disadvantage-probability/), and we know that advantage on a d20 gives you roughly **50%** better chances at scoring numbers above `15`. That’s huge! But it’s also d20-system-exclusive. How does this math stack up for d100 advantage? And more interestingly, how does it compare to **rolling Tony?**

## Rolling With Tony

One week ago I was running D&D for my two best buds. One of them, a bit of a rules lawyer (and literally an actual lawyer-in-training), proposed a way to make advantage more interesting in d100 systems. But first, a little explaining: Nobody has a 100-sided die. (Okay they [do exist](https://www.mathartfun.com/thedicelab.com/index.html) but c’mon now.) Instead of looking for such a many-face object, we roll **two d10’s** and assign one to the tens-place and the other to the ones-place. **If they come up as `3` and `7`, that’s a `37`.** This concatenation is what gives rise to what I’ve started calling **”rolling Tony.”**

***You take two d10’s, roll em, and rearrange them to be the best possible combination if you have advantage (or worst for disadvantage).*** A roll of `9` and `1` becomes either a `19` or a `91` depending on what you have at the time.

My friend, Anthony, thought this was fun enough as is, but wanted to know the *math* behind it. Was it actually mechanically meaningful? **Yes,** and here’s the plots to prove it.

## The Dice Code

The full code is [here](https://github.com/stevenzych/d100_alternate_advantage_mechanic/blob/master/d100_alternate_advantage_mechanic.ipynb) if you wanna read all of it, but I’ll just hit the highlights here.

> And one last note, in some d100 systems, rolling *lower* is better. That is what is treated as advantage herein.

I wrote some simple little simulations to roll dice a couple million times:

```
ten_lower_list = []
for i in range(1000000):
    a = rn.randint(0, 10)
    b = rn.randint(0, 10)
    first_combo = (int(str(a)+str(b)))
    second_combo = (int(str(b)+str(a)))
    if first_combo < second_combo:
        ten_lower_list.append(first_combo)
    else:
        ten_lower_list.append(second_combo)
```

Basically what this does is:
- Rolls two dice.
- Assigns them to variables `a` and `b`.
- Turns those rolled numbers into strings.
- Concatenates those strings in both combinations.
- Turns those combinations back into integers.
- Checks which combo is bigger.
- Keeps that.
- Repeat 1,000,000 times.

After running the simulation, we get a list of 1,000,000 values where the frequency of any value on the list is the same (or approaching same) as it’s frequency of being produced by rolling a d100 with this alternate advantage system. When visualized it looks like this:

![Adv Plot](https://i.imgur.com/5EUXu2Z.png)

Obviously, this distribution ***heavily*** favors the lower end of this number line. Some of the higher numbers aren’t even possible to roll! Everything from `90` to `98`, for example, gets switched to the lower number combinations that form the list `[09, 19, 29 … 89]`. The inverse holds for the disadvantage distribution, shown below:

![Disadv Plot](https://i.imgur.com/g3McwZR.png)

Now the numbers `01` to `09` are impossible and form the list `[10, 20, 30 … 90]`. When overlaid, this *beautiful* inverse relationship is shown even more clearly. I could look at it all day:

![Together Plot](https://i.imgur.com/bj0g47E.png)

## Rolling Tony In Practice

So we know how *likely* you are to roll any given number with either mechanic, but that doesn’t actually tell us how likely you are to roll *below* any given number to succeed on an in-game challenge. Knowing you likelihood to roll a `30` with Tony advantage, for example, doesn’t actually tell you how likely you are to pass a skill check **at the difficulty.** What we *want* to know is:

> *”What are my odds of rolling *under* a given number with Tony advantage?”*

The following loop creates a dictionary that answers this question:

```
adv_dict = {}
for x in range(100):
    skill_level = x
    skill_list = []
    for i in range(100000):
        a, b = rn.randint(0, 10, 2)
        first_combo = (int(str(a)+str(b)))
        second_combo = (int(str(b)+str(a)))
        if first_combo < second_combo:
            winner = first_combo
        else:
            winner = second_combo
        skill_list.append(winner < skill_level)
    adv_dict[x] = np.average(skill_list)
```

In short, this creates a dictionary where the key is a skill level and the value is your likelihood to roll *under* it with Tony advantage. It uses the same loop as the previous bit of code, with the addition of storing only a `True` or `False` value on whether or not we got under the target skill. A second dictionary was made for disadvantage, and when plotted together they look like this:

![Almond Plot](https://i.imgur.com/eoaIz9G.png)

Wowza! An almond! Those little areas where each plot plateaus are the areas where no numbers are possible, per the previous observations. At the midpoint--rolling under a 50--there is a 50% gap between the likelihoods depending on advantage. This is the greatest disparity, and naturally dwindles towards the ends of the almond. Moreover, the worse your character is at a skill check (i.e., the *lower* they need to roll to succeed) the **more** this system punishes you! It’s like double disadvantage, and we owe it to the almost-logarithmic shapes of the graph.

If anyone uses this system, let me know! I’m gonna, but honestly it’s less about the math, and more about the fun of telling a player “Nah, you gotta switch those dice around.”

