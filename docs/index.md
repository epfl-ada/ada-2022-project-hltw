---
#
# Here you can change the text shown in the Home page before the Latest Posts section.
#
# Edit cayman-blog's home layout in _layouts instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
---

> Common knowledge are shared, but people think differently to infer on things.

# Introduction

Wikipedia is viewed as a storage for an incredible amount of information. However, in reality, it is also a largely connected graph of articles with semantic and contextual similarities. 

An analogy in real life would be Cities and Towns representing articles and blue links in wikipedia are the roads that connect the cities. There might be cities that are interconnected with highway, so more people can get around the cities in a more direct and faster fashion.  

With wikipedia alone we cannot have a view the traffic of these roads, since we have no knowledge of how people hop from one idea to another. That’s where the Wikispeedia dataset comes in.

Wikispeedia is an online game where players have to reach the target wiki article from an unrelated start wiki article by clicking links in the articles, where the human clicked links can be considered as the traffic between cities. The path taken is recorded during the gameplay, which is the main subject of our analysis! 

Let’s view the connections people make in the Wikispeedia game as trips between cities and towns, and start the discovery journey!

## How are the knowledge cities connected together?

There are some cities that has a lot of roads connected to them, this is the idea of hub on the graph. Hubs are great for identifying the commonly known idea, but it doesn't show how ideas are connected and used together. So instead, we decided to look at how we get from one idea to the other, which will give us a better picture of how people think.

We come up with the idea of using common sub-paths of length greater than 1, which represents how logical ideas are linked together. We observed that we have many recurring common paths in the Wikispeedia games, which we call highways - as they are the most commonly used paths between many articles and are largely reused in games. We would like to compare it against real-world wikipedia data.

# Data

## Explain Wikispeedia dataset

## Highways

As with any road trip, sometimes taking a highway is the fastest way to get to the destination. The common subpaths in Wikispeedia games are those highways which connect multiple clusters of cities and allow game players to travel fast.

We began our analysis by looking into the highways! To do so, we define highways in our dataset as roads reused at least 6 times from all the recorded data. As seen in the heavy tail distribution, we have thousands of “roads” used 2-5 times however, with roads used more than 6 times the variety of these “roads” is in the hundreds and makes them significantly more rare.

**insert heavy tail distribution image here**

### Path counts for subpaths

We use the common subpath with length 2-6 and 7-10 as the representative.

**insert plot In 42**

In the graph above we compare value counts of paths of length 2-6 and 7-10. We can see that 2-6 almost constantly holds a higher value count ratio than 7-10. 2-6 Has almost 80 times more data compared to 7-10.
An overview of the number of entries for the common subpaths with each length:

**insert plot In 44**

We can see rapid decrease from 4-5 path reusage. Technically it should make sense to to subpaths of length 2-5 but we include 6 just to draw more interesting results, as subpaths of 6 may connect more peculiar areas.

## Categorizing “highways”

Highways itself are not as interesting if we cannot categorize them. What we try to do later is to determine types of highways. The general idea is that we have 2 types of highways: local and international. Local highways connect similar articles but still allow users to travel fast through blue links. On the other hand, international highways are the highways that connect very different sets of articles and allow users to make large “contextual/semantic” jumps between start and end sets.

### Determine bag of starting articles for each highway

With the filtered highways we determine “entry points”(start) - articles that are one link away from highway in user’s game path and “exit points”(end) - articles that user ends up on after using the highway.

**insert plot Out 47**

We have a heavy tail distribution of value counts for times each path of different length is taken.

### Data of Highways

**TBD**

# Common knowledge inferring process and their connections

Our next analysis will try to determine if the players of the game think differently or similarity to the real world.

## Using Wikipedia2Vec

Wikipedia2Vec is a tool that converts Wikipedia articles into vector representations. We downloaded the 100-feature pretrained model provided by Wikipedia2Vec and used it in our research.

Our idea is that we will use Wikipedia2Vec as a indication on how things can be inferred from one to another, thus, giving us information on how the real-world knowledge can be inferred from one idea to another.

## Classiying Highways

As mentioned previously, classifying highways into local and international is one of our objectives. However, there is no clear answer to this, therefore, we classify the highways whose start and end article sets have small cosine similarity as “International Highways” and otherwise “Local Highways”. From the distribution of similarities, which is similar to Normal distribution, the cutoff threshold for “International Highways” lies somewhere between 0.2-0.4 for average and weighted average experiments.

## Similarities

We tried 3 different approaches to find the international and local highways. We calculate mean, weighted mean of entry and exit point article sets and then compute cosine similarity. Or as our third approach we produce a matrix from the start and end point article vectors and compute euclidean distance between the generated matrices.

### Average

For computing the average vector we take all the vectors we have in entry and end sets and compute the mean vectors which are later used to compute cosine similarity.

{% include average_dist.html %}

{% include averaged.html %}

With threshold 0.2 we see that the table shows really disconnected sets of articles at the start and the end. However, we face one problem, that the article sets are mostly composed of very few articles. The reason for this is most likely that the more articles you have, the more similar they become to each other when they are averaged out. Therefore, we tried the two other following approaches.

### Weighted Average

For computing the average vector we take all the vectors we have in entry and end sets and compute the mean vectors which are later used to compute cosine similarity.

{% include weighted_dist.html %}

{% include weighed.html %}

With threshold 0.2 we see that the table shows really disconnected sets of articles at the start and the end. However, we face one problem, that the article sets are mostly composed of very few articles. The reason for this is most likely that the more articles you have, the more similar they become to each other when they are averaged out. Therefore, we tried the two other following approaches.

### Euclidean Distance

Another way of viewing the similarity between entry and exit sets was to create a matrix for each of them and compute the euclidean distance between them. We concatenate vectors accordingly into start and end matrices and then compute the Euclidean distance between the two. Finally, the distance is normalized for viewing the results.

{% include euc_dist.html %}

{% include euclidean.html %}

### Similarities between start and end points / Conclusion?

**insert plot In 60**

We now have frequencies of similarity values between start and end points of highways. 

Now we need to choose a threshold to separate local(similar) highways between country highways.

**insert plot In 68**

### Interesting findings

# Unique paths of Wikispeedia

So far, we have only been discovering the highways, which are the common knowledge shared by people. But if we look at the data in a different way - instead of trying to see the commonly used highways, what are the ways that one can do to go from one city to another?

## Our motivation

<img align = "center" src="output/value_of_optimal_move.png" width="60%" height="60%"> 

In the graph above, we can see the decrease in distance of each move. The x-axis means the distance change of a move, and the y-axis is the percentage of the move which decreases the distance by x over all moves.

We can see that about 68% of the edges in all the paths decrease the distance by 1, and about 28% of the edges do not change the distance. This means in general the when people are moving from idea to idea, they tend to pick the fastest way. 

<img align = "center" src="output/ratio_of_optimal_move.png" width="60%" height="60%"> 

In the diagram above we focus on the edges that decrease the distance. The x-axis means the ratio of such edges in the whole path, and the y-axis is the number of paths.

We can see that the majority of paths contains more than 50% edges that decrease the distance, and a large portion of the paths contains only distance-decreasing edges. As we can already see some interesting things from here - there are some players that take a detour! We'll then focus on these players, and find out how do they think differently from the majority.

It's like in real life, taking the highway might be the fastest option, but we might sometimes want to make a detour and sightsee a bit. So in the next section, we will present our findings on how differently people think, to get from one idea to another.

## How we apprach it 

In general, the basis of the classification is the change in distance to the destination of each step. Specifically, when moving from one page to another through the link, the player may get closer to the destination if he or she chooses the correct link, which will decrease the distance to the destination by 1. On the contrary, the player may also get further to the destination. We then count in the whole path, how many edges have made the distance decreases, how many edges have not change the distance, and how many edges have increase the distance by 1, 2 or more. We can then get a table like this:

| Distance Change | inf (move to pages that can't reach the destination) | -1 | 0 | 1 | 2 | 3 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| count | 0 | 4 | 2 | 2 | 0 | 0 |

With such information, we can characterize each path with a vector of distance change count. For example, the path of the above table can be characterize with the vector [0, 4, 2, 2, 0, 0]. If we normalize the vector, we can get [0, 0.5, 0.25, 0.25, 0, 0].

With each path characterized with a vector, we can apply data analyzing methods like clustering on the vector space, and get different groups of paths.

**TBD**

## 3 Categories

We discovered that there are mainly 4 types of people in the game, the one that tries to play optimially, **TBD**. 

# In a nutshell

Common knowledge are shared, but people think differently to infer on things.

We can see from the first part of the data story that highways, which are the common knowledge, bridges a lot of the ideas together. But we can also see that not everyone thinks the same way, as in the second part we discovered that for the same starting and ending ideas, people can have drastically different approach to inferring between them.

United in diversity.
