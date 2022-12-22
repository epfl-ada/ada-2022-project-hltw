---
#
# Here you can change the text shown in the Home page before the Latest Posts section.
#
# Edit cayman-blog's home layout in _layouts instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
---

# Introduction

Wikipedia is viewed as a storage for an incredible amount of information. However in reality it is also a largely connected graph of articles with semantic and contextual similarities. An analogy in real life would be Cities/Towns represent articles and blue links in wikipedia are the roads that connect the cities. However, with wikipedia alone we cannot view the traffic of these old roads, that’s where the Wikispeedia dataset comes in. Wikispeedia is an online game where players have to reach the target wiki article from an unrelated start wiki article by clicking links in the articles, where the human clicked links can be considered as the traffic between cities. 

Let’s view the connections people make in the Wikispeedia game as trips between towns, and start the discovery journey!

# How are the knowledge cities connected together?

Instead of using the idea of the hub, we use the idea of common sub-paths of length greater than 1, which represents how logical ideas are linked together. We observed that we have many recurring common paths in the Wikispeedia games, which we call highways - as they are the most commonly used paths between many articles and are largely reused in games. We would like to compare it against real-world wiki data.

# Data
## Explain wikispeedia dataset

## Highways
As with any road trip, sometimes taking a highway is the fastest way to get to the destination. The common subpaths in Wikispeedia games are those highways which connect multiple clusters of cities and allow game players to travel fast.

In our dataset we define highways as roads reused at least 6 times, by user. As seen in the heavy tail distribution, we have thousands of “roads” used 2-5 times however, with roads used more than 6 times the variety of these “roads” is in the hundreds and makes them significantly more rare.

## Path counts for subpaths

We use the common subpath with length 2-6 and 7-10 as the representative.
[insert plot In[42]]
In the graph above we compare value counts of paths of length 2-6 and 7-10. We can see that 2-6 almost constantly holds a higher value count ratio than 7-10. 2-6 Has almost 80 times more data compared to 7-10.
An overview of the number of entries for the common subpaths with each length:
[insert plot In[44]]
We can see rapid decrease from 4-5 path reusage. Technically it should make sense to to subpaths of length 2-5 but we include 6 just to draw more interesting results, as subpaths of 6 may connect more peculiar areas.

## Categorizing “highways”

Highways itself are not as interesting if we cannot categorize them. What we try to do later is to determine types of highways. The general idea is that we have 2 types of highways: local and international. Local highways connect similar articles but still allow users to travel fast through blue links. On the other hand, international highways are the highways that connect very different sets of articles and allow users to make large “contextual/semantic” jumps between start and end sets.

## Determine bag of starting articles for each highway

With the filtered highways we determine “entry points”(start) - articles that are one link away from highway in user’s game path and “exit points”(end) - articles that user ends up on after using the highway.
[insert plot Out[47]]
We have a heavy tail distribution of value counts for times each path of different length is taken.

## Data of Highways


# Common knowledge inferring process and their connections
Some info here about what we seek to achieve

## Using Wikipedia2Vec

Wikipedia2Vec is a tool that converts Wikipedia articles into vector representations. We downloaded the 100 feature pretrained model provided by Wikipedia2Vec and used it in our research.

## Classiying Highways

As mentioned previously, classifying highways into local and international is one of our objectives. However, there is no clear answer to this, therefore, we classify the highways whose start and end article sets have small cosine similarity as “International Highways” and otherwise “Local Highways”. From the distribution of similarities, which is similar to Normal distribution, the cutoff threshold for “International Highways” lies somewhere between 0.2-0.4 for average and weighted average experiments.

## Similarities
We tried 3 different approaches to find the international and local highways. We calculate mean, weighted mean of entry and exit point article sets and then compute cosine similarity. Or as our third approach we produce a matrix from the start and end point article vectors and compute euclidean distance between the generated matrices.

### Average
For computing the average vector we take all the vectors we have in entry and end sets and compute the mean vectors which are later used to compute cosine similarity.
{% include averaged.html %}

With threshold 0.2 we see that the table shows really disconnected sets of articles at the start and the end. However, we face one problem, that the article sets are mostly composed of very few articles. The reason for this is most likely that the more articles you have, the more similar they become to each other when they are averaged out. Therefore, we tried the two other following approaches.
### Weighted Average

For computing the average vector we take all the vectors we have in entry and end sets and compute the mean vectors which are later used to compute cosine similarity.
{% include weighed.html %}

With threshold 0.2 we see that the table shows really disconnected sets of articles at the start and the end. However, we face one problem, that the article sets are mostly composed of very few articles. The reason for this is most likely that the more articles you have, the more similar they become to each other when they are averaged out. Therefore, we tried the two other following approaches.

### Euclidean Distance

Another way of viewing the similarity between entry and exit sets was to create a matrix for each of them and compute the euclidean distance between them. We concatenate vectors accordingly into start and end matrices and then compute the Euclidean distance between the two. Finally, the distance is normalized for viewing the results.

{% include euclidean.html %}

### Similarities between start and end points / Conclusion?
[insert plot In[60]]
We now have frequencies of similarity values between start and end points of highways. 


Now we need to choose a threshold to separate local(similar) highways between country highways.
[insert plot In[68]]

### Interesting findings

## Context category changes RQ3?Opt?

# Unique paths of Wikispeedia

Going on a roadtrip with friends is fun, but everyone has that one friend who always interprets things differently, takes the wrong turn, or plays songs everyone hates. What we are trying to see here is if when that friend is in the driving seat, can he actually reach the destination with his way of thinking. We would try to see how their unique trips complete the journey. Or simply take a scenic ride through wikipedia articles that no one else took.

....
