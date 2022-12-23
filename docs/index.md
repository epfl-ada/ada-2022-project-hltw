---
#
# Here you can change the text shown in the Home page before the Latest Posts section.
#
# Edit cayman-blog's home layout in _layouts instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
---

> Common knowledge is shared, but people think differently to infer things.

# Introduction

Wikipedia is viewed as storage for an incredible amount of information. However, in reality, it is also a largely connected graph of articles with semantic and contextual similarities. 

An analogy in real life would be Cities and Towns representing articles and blue links in Wikipedia are the roads that connect the cities. There might be cities that are interconnected with highways so that more people can get around the cities in a more direct and faster fashion.  

With Wikipedia alone, we cannot view the traffic on these roads, since we have no knowledge of how people hop from one idea to another. That’s where the Wikispeedia dataset comes in.

Wikispeedia is an online game where players have to reach the target wiki article from an unrelated start wiki article by clicking links in the articles, where the human-clicked links can be considered as the traffic between cities. The path taken is recorded during the gameplay, which is the main subject of our analysis! 

Let’s view people's connections in the Wikispeedia game as trips between cities and towns, and start the discovery journey!

# How are the knowledge cities connected together?

There are some cities that have a lot of roads connected to them, this is the idea of the hub on the graph. Hubs are great for identifying a commonly known idea, but it doesn't show how ideas are connected and used together. So instead, we decided to look at how we get from one idea to the other, which will give us a better picture of how people think.

We come up with the idea of using common sub-paths of length greater than 1, which represents how logical ideas are linked together. We observed that we have many recurring common paths in the Wikispeedia games, which we call highways - as they are the most commonly used paths between many articles and are largely reused in games. We would like to compare it against real-world Wikipedia data.


## Using Wikispeedia dataset

We mainly focused on `paths_finished.tsv` from the Wikispeedia dataset. It contains `hashedIpAddress`, `timestamp`, `durationInSec`, `path` and `rating`, we only use `path` here. The `path` consists of the starting article and every player-clicked article. Some paths have a back sign "<", we decided to remove the sign and the related misselected articles to avoid any misleading. There is also an unfinished path file, we chose not to use it because the number of finished paths (51318) is twice as large as unfinished paths (24875), and finished paths are more valuable for our research. 

## Highways

As with any road trip, sometimes taking a highway is the fastest way to get to the destination. The common subpaths in Wikispeedia games are those highways that connect multiple clusters of cities and allow game players to travel fast.

We began our analysis by looking into the highways! To do so, we define highways in our dataset as roads reused at least 6 times from all the recorded data. As seen in the heavy tail distribution, we have thousands of “roads” used 2-5 times however, with roads used more than 6 times the variety of these “roads” is in the hundreds and making them significantly rare.

### Path counts for subpaths

We use the common subpath with length 2-6 and 7-10 as the representative. We use the length up to 6 as the cutoff because the longer the length, the less usage it has, thus, it doesn't give us meanful statistics.

The following graph shows an overview of the number of entries for the common subpaths with each length.

{% include value_counts.html %}

We can see rapid decrease from 4-5 path reusage. Technically it should make sense to the subpaths of length 2-5 but we include 6 just to draw more interesting results, as subpaths of 6 may connect more peculiar areas.

## Categorizing “highways”

Highways itself are not as interesting if we cannot categorize them. What we try to do later is to determine types of highways. The general idea is that we have 2 types of highways: local and international. Local highways connect similar articles but still allow users to travel fast through blue links. On the other hand, international highways are the highways that connect very different sets of articles and allow users to make large “contextual/semantic” jumps between start and end sets.

### Determine bag of starting articles for each highway

With the filtered highways we determine “entry points”(start) - articles that are one link away from highway in user’s game path and “exit points”(end) - articles that user ends up on after using the highway.

We have a heavy tail distribution of value counts for times each path of different length is taken.


# Common knowledge inferring processes and their connections

We try to see how these article cities are connected. We have a large variety of starting and ending articles which are connected through the same highway. The main idea is to identify the most diverse sets of articles at the start and the end to see what are the most unique highways on Wikispeedia game.

## Using Wikipedia2Vec

Wikipedia2Vec is a tool that converts Wikipedia articles into vector representations. We downloaded the 100-feature pre-trained model provided by Wikipedia2Vec and used it in our research.

Our idea is that we will use Wikipedia2Vec as an indication of how things can be inferred from one to another, thus, giving us information on how real-world knowledge can be inferred from one idea to another.

## Classifying Highways

As mentioned previously, classifying highways into local and international is one of our objectives. However, there is no clear answer to this, therefore, we classify the highways whose start and end article sets have small cosine similarity as “International Highways” and otherwise “Local Highways”. From the distribution of similarities, which is similar to Normal distribution, the cutoff threshold for “International Highways” lies somewhere between 0.2-0.4 for average and weighted average experiments.

## Similarities

We tried 3 different approaches to find the international and local highways. We calculate the mean, and weighted mean of entry and exit point article sets and then compute cosine similarity. Or as our third approach, we produce a matrix from the start and end point article vectors and compute the euclidean distance between the generated matrices.

### Average

For computing the average vector we take all the vectors we have in entry and end sets and compute the mean vectors which are later used to compute cosine similarity.

{% include average_dist.html %}

We can see that the frequencies of similarities fall into a normal distribution. Therefore, we can try to find a cut-off at the left side of the distribution. As previously discussed we would try to cut off at around the 0.2-0.4 threshold.

{% include averaged.html %}

With threshold 0.2 we see that the table shows really disconnected sets of articles at the start and the end. However, we face one problem, the article sets are mostly composed of very few articles. The reason for this is most likely that the more articles you have, the more similar they become to each other when they are averaged out. Therefore, we tried the two other following approaches.

### Weighted Average

For computing the average vector we take all the vectors we have in entry and end sets and compute the mean vectors which are later used to compute cosine similarity.

{% include weighted_dist.html %}

The weighted average frequency distribution also falls into the normal distribution. Therefore, we can follow the same thresholding principle as for the average case.

{% include weighed.html %}

With threshold 0.2 we see that the table shows really disconnected sets of articles at the start and the end. However, we face one problem, that the article sets are mostly composed of very few articles. The reason for this is most likely that the more articles you have, the more similar they become to each other when they are averaged out. Therefore, we tried the two other following approaches.

### Euclidean Distance

Another way of viewing the similarity between entry and exit sets was to create a matrix for each of them and compute the euclidean distance between them. We concatenate vectors accordingly into start and end matrices and then compute the Euclidean distance between the two. Finally, the distance is normalized for viewing the results.

{% include euc_dist.html %}
The euclidean distance approach has a skewed distribution and makes all the articles pretty similar. In this case, the cutoff is at around 0.6-0.7.

{% include euclidean.html %}

The euclidean distance approach allows us to see highly connected sets of start and end articles. Both of the sets consist of a large variety of very different topics.

### Conclusion

The three approaches allowed us to get identify the most disconnected sets of entry and exit articles. Which lets us determine some "correlations" players make on the game play path. For example:

"Asteroid" to "Viking" is connected the same way as is "Sun" to "Ireland". However that does not mean that the two are related in any sense in Wikipedia, just that the players playing the game interpret the two start and end destinations the same way. 

Playing around with similarity threshold in the tables above, will give you a good sense of spurious correlations that can occur in the game play without the players even knowing.

# Unique paths of Wikispeedia

So far, we have only been discovering the highways, which are the common knowledge shared by people. But if we look at the data in a different way - instead of trying to see the commonly used highways, what are the ways that one can do to go from one city to another?

## Our motivation

<img align = "center" src="output/value_of_optimal_move.png" width="60%" height="60%"> 

In the graph above, we can see the decrease in the distance of each move. The x-axis means the distance change of a move, and the y-axis is the percentage of the move which decreases the distance by x overall moves.

We can see that about 68% of the edges in all the paths decrease the distance by 1, and about 28% of the edges do not change the distance. This means in general when people are moving from idea to idea, they tend to pick the fastest way. 

<img align = "center" src="output/ratio_of_optimal_move.png" width="60%" height="60%"> 

In the diagram above we focus on the edges that decrease the distance. The x-axis means the ratio of such edges in the whole path, and the y-axis is the number of paths.

We can see that the majority of paths contain more than 50% edges that decrease the distance, and a large portion of the paths contains only distance-decreasing edges. As we can already see some interesting things from here - there are some players that take a detour! We'll then focus on these players, and find out how they think differently from the majority.

It's like in real life, taking the highway might be the fastest option, but we might sometimes want to make a detour and sightsee a bit. So in the next section, we will present our findings on how differently people think, to get from one idea to another.

## How we approach it 

In general, the basis of the classification is the change in distance to the destination of each step. Specifically, when moving from one page to another through the link, the player may get closer to the destination if he or she chooses the correct link, which will decrease the distance to the destination by 1. On the contrary, the player may also get further to the destination. We then count in the whole path, how many edges have made the distance decrease, how many edges have not changed the distance, and how many edges have increased the distance by 1, 2, or more. We can then get a table like this:

| Distance Change | inf (move to pages that can't reach the destination) | -1 | 0 | 1 | 2 | 3 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| count | 0 | 4 | 2 | 2 | 0 | 0 |

With such information, we can characterize each path with a vector of distance change count. For example, the path of the above table can be characterized with the vector [0, 4, 2, 2, 0, 0]. If we normalize the vector, we can get [0, 0.5, 0.25, 0.25, 0, 0].

With each path characterized with a vector, we can apply data analyzing methods like clustering on the vector space, and get different groups of paths.

Let's focus on the paths with the least portion of distance-decreasing edges, i.e. the paths that take the most detours.

## Analyzing the detoured paths

For inspecting the paths with the most detours, we sort all the paths with the portion of distance-decreasing edges and select the 50 paths with the least portion of distance-decreasing edges. After that, we manually inspect these paths and check if there are some similarities. Note that there are some very lucky paths in which the starting point and the destination are exactly the same, and thus leads to no distance-decreasing edges, of course, we filter out them in this part.

Here are some examples of the detoured paths.

{% include detoured_paths.html %}

In the following sections, we will try to catogrize these detoured paths, and compare them with both other players' paths and the shortest paths computed by algorithm.

### 3 categories of detoured paths

Based on our observations, in general, we can categorize the detoured paths into three groups: the **sightseeing** paths, the **sequential retrival** paths, and the **lingering** paths.

#### Sightseeing paths

These paths seem to be deliberately taking detours since they contain many pages that do not relate to both the starting point and the destination. Here are some examples.
{% include sightseeing_paths.html %}

#### Sequential retrieval paths

These paths go through many similar pages sequentially, and finally reach the destination. For example, navigating through chemical elements, the names of kings, or simply different centries. Here are two examples.
{% include sequential_retrieval_paths.html %}

#### Lingering paths

These paths quickly reach the pages that are semantically close to the destination, but, unfortunately, they are just semantically related (without a hyperlink). In the following example, we can see that the first path goes to 'football' quickly, but failed to find a way from 'football' to 'sport'. Finally, it takes detours to find the correct way. In the second path, it goes through Asian countries, regions, and cultures, but failed to find a way to the page 'Asia' itself.
{% include lingering_paths.html %}

In fact, the boundaries of the three types are not absolute, a detoured path can be both a sequential retrieval path and also lingering path.

### Comparison among detoured paths, shortest paths, and other's paths

We then compare them with the shortest paths and other players' paths. 

Since the shortest path is computed by an algorithm, it doesn't consider any semantic information. In some cases, it's hard to come up with such paths for humans. Here are some examples.

{% include shortest_path_demo.html %}

The human player's shortest path sometimes makes wiser choices, so that they succeed in the game. For example, instead of navigating sequentially, they go to hub pages or take highways. Here are some examples.

{% include shortest_path_wiser.html %}

But sometimes maybe they are just luckier than the detoured paths. In the following example, we highly suspect that the players have taken similar strategies.

{% include shortest_path_lucky.html %}

# In a nutshell

Common knowledge is shared, but people think differently to infer things.

We can see from the first part of the data story that highways, which are common knowledge, bridge a lot of ideas together. But we can also see that not everyone thinks the same way, as in the second part we discovered that for the same starting and ending ideas, people can have a drastically different approaches to inferring between them.

United in diversity.


