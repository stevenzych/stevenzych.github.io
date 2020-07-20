---
layout: post
title:      "Linear Regression & Race: King County Housing"
date:       2020-07-20 23:31:39 +0000
permalink:  linear_regression_and_race_king_county_housing
---


While there's much to be said of how socially problematic machine learning models can be when used thoughtessly (such as these examples of [systematically mis-diagnosing and failing to provide medical service to Black folks](https://www.nature.com/articles/d41586-019-03228-6) and [failing to represent women in the hiring process](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)), this little post is only going to tackle a specific instance of this problem. A machine-learning model I developed and recently used to assess [this](https://www.kaggle.com/harlfoxem/housesalesprediction) data from Kaggle--which contains informations on homes sold in and around Seattle frrom May 2014 to May 2015--was reflective of a racial bias present in the city's contemporary housing market. In other words, the linear regression model I made had the goal of recommended cheap homes for first-time buyers. A large portion of these homes clustered in areas that were predominantly Black. These areas were:

1. Far from the city center (either downtown Seattle or the wealthy tech hub of Bellevue).
2. Away from desirable waterfront property.
3. Lacking basements (AKA, generally smaller).

Let me be very clear. The United States has a disgusting and racist history of [redlining](https://en.wikipedia.org/wiki/Redlining), [broken-window policing](https://en.wikipedia.org/wiki/Broken_windows_theory), [environemntal racism](https://en.wikipedia.org/wiki/Environmental_racism#:~:text=Environmental%20racism%20is%20a%20concept,both%20in%20practice%20and%20policy.), and generalized white supremacy that contribute to this problem--The problem that situates white people in desirable homes near the heart of the economy, and that pushes Black and Brown folx to the edges of cities, to their industrial districts, etc. This is not a novel take. My point is to illustrate that using a model like this one (which I, as evidenced here, do **not** find to be sufficiently advisory when it comes to buying a home), while unaware of the staus quo that created its training data, is *insufficient* for decision making in the real world. With that being said, I'd like to go into some examples of the three points illustrated above.

## South Of Mercer Island,

First of all, the model showed a negative correlation between price and living in "sectors 3 and 4," which, in my model were defined by a four-sector system which had its (0, 0) point just south of Mercer Island. For those unaware of Seattle's geography, putting the four-corners point here means that sector 1 gets downtown Seattle, sector 2 gets glitzy Bellevue, and sectors 3 and 4 get Renton, some other smaller cities, and two of the city's three airports. For a sense of racial makeup, turn your attention to this map from the [Seattle Civil Rights & Labor History Project's](https://depts.washington.edu/civilr/segregation_maps.htm) segregation maps:

![2010 Seattle Segregation Map](https://i.imgur.com/YSKazZb.png)

Mercer Island is that hunk of land floating in the central body of water, by the way. You'll notice right away that most of the area directly south of it, and somewhat to the west, is the most predominantly Black area in the city (though technically it's mostly out of the city proper). This next graphic plots homes that cost $300K or less over the same area as the map above. Every blue dot is a house from the overall dataset. Every red dot is a house under the $300K price threshold.

![Seattle Homes < $300K](https://i.imgur.com/7Jma2Gn.png)

Notice anything? It'd be nice to assume that Black people are just randomly getting better deals on their houses, but I think we both know enough about history to sadly dismiss that fantasy.

## Off The Water,

In a similar trend, we notice that the majority of waterfront homes (while admittedly sparse to begin with) are in predominantly white areas. The plot below follows the same rules as the previous scatter plot:

![Seattle Waterfront Homes](https://i.imgur.com/5L27yaF.png)

Aside from a small pocket in downtown Renton, most of these highly-desirable, highly-expensive waterfront properties are on Mercer Island, Vashon Island (that blob out west), on the coast *near* Vashon Island (which, while solidly in sector 3, is *not* predominantly Black), and in Bellevue. Again, this same pattern repeats itself.

## And Basementless

Does having a basement make or break a home? Maybe not, but it definitely provides more storage space, a respite from the elements on blistering summer days or during a tornado watch (though that could just be my Illinois brain talking), and overall increases the square footage of a home, which is correlates positively with its sale price. This third and final plot illustrates the spread of basement-ed homes in and around Seattle:

![Seattle Homes With Basements](https://i.imgur.com/eS4Jl2l.png)

Now this plot is a little less obvious than the previous two--Basements abound, regardless of direction. Remember however, that we're checking the *density* of the red dots on our map. And here, as above, they are less dense south of Mercer Island than they are to the Northwest and East of it.

This map doesn't crack open the housing disparity even half as much as the $300K-and-cheaper map does (hence it being included last), but it compounds the inequities displayed in the previous ones. Moreover, I think it encourages us (or at least me) to mine oft-overlooked features of data. In this case, it helped to further prove a point when I didn't think it could.

## In Conclusion, A Brief Disclaimer

I am not an expert. I'm a fledgling data scientist, and a white kid from the suburbs. I'm still learning, but I felt this was a salient example that could highlight social issues I care about in the field I'm about to enter, and I felt I could at least do it (a little) justice.

**Housing inequality is violence.** If this blog post in any way moved you, I encourage you to research your city or the one nearest your heart, and see what organizations are around. I, for one, have an affinity to Chicago's [Little Village Environmental Justice Organization](http://www.lvejo.org/), which is admittedly not the *exact* same issue, but surely a tangentially-related one. It's a great place to look for further education, and to understand the social intersections of this issue.
