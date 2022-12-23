# Is "pyramid" related to "bean"? What are the craziest connections people make in the Wikispeedia game?

# Data story
Please visit our website [here](https://epfl-ada.github.io/ada-2022-project-hltw/)

# Abstract

Wikispeedia is an online game where players have to reach the target wiki article from an unrelated start wiki article by clicking links in the articles. In all finished paths, some paths are reached logically by following Wikipedia articles that are semantically similar or steer toward familiar topics. However, the Wikispeedia game wouldn’t be a game if there weren’t any unique and genuine ways of reaching the desired end goal. These ingenious ways in which very few people connect are what draw us to explore this data set. 

We want to find the paths that for normal people may seem very unreasonable, but essentially produce the desired result. There is no exact way of telling what is reasonable and not, but we would use ideas like common paths taken, to see how people navigate the pages. The story would show how people travel through Wikispeedia games similarly as some people travel through roads and highways.

# Research Questions

## 1. What are the common knowledge inferring process and how they are used to connect ideas together?

Instead of using the idea of the hub, we use the idea of common sub-paths of length greater than 1, which represents how logical ideas are linked together. We observed that we have many recurring common paths in the Wikispeedia games, which we call highways - as they are the most commonly used paths between many articles and are largely reused in games. We would like to compare it against real-world wiki data. 

## 2. How differently do people think?

For a given pair of starting and ending pages, we look at all successful user-run paths that took a detour from the path that the majority took and try to identify how semantically distant people's thoughts are from each other. 

## 3. What are the spurious correlations between graphs of similar paths?

Correlation does not imply causation, however, it sometimes feels fun to see them this way. For example, [the website](https://www.tylervigen.com/spurious-correlations) has a lot of seemingly unrelated things correlated to death. We do not seek such a grim topic, however, we are eager to find if in some cases we can find fun and joyful correlations between Wikipedia articles.

# Proposed Additional Datasets

- Augmented all finished paths and breaking it down into sub-paths
- [Wikipedia page dump](https://dumps.wikimedia.org/)
- An additional tool instead of the dataset that we might be using is [Wikipedia2Vec](https://wikipedia2vec.github.io/wikipedia2vec/). 

# Methods

Our initial exploration and some prototyping of the ideas are in the [notebook](https://github.com/epfl-ada/ada-2022-project-hltw/blob/main/P2.ipynb).

## Data pre-processing 

- There are some paths containing the back click sign, "<". We reduce the path by only keeping the paths without backtracking. 
- Perform unquoting on words and splitting the paths into lists for easier data processing.
- Compute commonly used sub-paths of length from 2 to 6
- Compute commonly appearing starting and ending pairs of pages from sub-paths of length greater than 1

## Compare common knowledge - how does the gamer's world differ from the real-world people

We would use Wikipedia2Vec to aid us with comparing the common knowledge used within the game to the real world based on the knowledge from the latest Wikipedia pages. For example, maybe the commonly used knowledge, like `(Brain, Mind, Linguistics, Language, Communication, Telephone)` is not how the real world usually infers from the brain to the telephone. 

## Extract information on how people think by comparing distinct paths taken from page A to page B

For a given popular starting and ending pair of pages, we analyze how many distinct paths are there. We would compute the distances (using the methods below) between all the distinct paths, or, cluster those into groups by the number of common nodes, etc., to see how differently people think of linking 2 different subjects.

### Distance

With the Wikispeedia dataset and other NLP models, we have several ways to measure the *distance* between pages.

We've proposed at least 3 ways for measuring the distance, including semantic distances, the shortest path distances, and the players' distances.

#### Semantic distances

With some commonly used NLP models like BERT and Word2Vec, we can calculate the distance between words and concepts. This can serve as a baseline for further comparison.

#### Shortest path distances

The shortest path matrix contains the shortest path between every pair of pages in the Wikispeedia game. We expect that it may have some inconsistencies with the semantic distances. For example, semantically distant words may be connected in a few steps in Wikispeedia, or vice versa. We will find out the inconsistencies and then try to understand the reasons behind them.

#### Players' distances

When playing the Wikispeedia game, players may have a model or strategy in their minds for navigation (like the A* algorithm). When players choose the next page at each step, they must have thought that the chosen page is closer to the destination than other options. Based on these comparison data, we can try to recover the graph model in players' minds, and compare them with the semantic distances or shortest path distances. We expect this will reveal how people think when playing the game.

Other possible distance metrics include real-world Wikipedia distances (based on full Wikipedia other than the subset used by Wikispeedia), and shortest-path with missing links (in some pages, many words are just plain text without a link, add links for them to obtain a new graph)

## Finding the spurious graph correlations

Based on the commonly used edges of different lengths, we accumulate the count of starting and ending pages from both ends of the path, and we analyze the potential spurious correlations. 

![](https://i.imgur.com/uhXLoUz.png)
![](https://i.imgur.com/d0o3MCZ.png)

# Proposed timeline

* 25/11/2022: Research question 1
* 2/12/2022: Research question 2
* 9/12/2022: Research question 3
* 16/12/2022: Design and develop a website
* 23/12/2022: Final deliverable

# Team Organization

## Members

* Teammate 1: Liudvikas 
* Teammate 2: Chenyu
* Teammate 3: Henry
* Teammate 4: Lili

## Contribution

* Explore data: Teammate 3,4
* Arrange proposal: Teammate 1,2,3,4
* Explore Wiki2Vec: Teammate 1
* Explore research question 1: Teammate 1
* Explore research question 2: Teammate 4
* Explore research question 3: Teammate 2
* Arrange data story: Teammate 1,2,3,4
* Website development: Teammate 1,3

