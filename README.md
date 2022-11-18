# Is "math" related to "science"? What are the craziest connections people make in the Wikispeedia game?

# Abstract

Wikispeedia is an online game where players have to reach the target wiki article from an unrelated start wiki article by clicking links in the articles. In all finished paths, some paths are reached logically by following Wikipedia articles that are semantically similar or steer toward familiar topics. However, the Wikispeedia game wouldn’t be a game if there weren’t any unique and genuine ways of reaching the desired end goal. These ingenious ways in which very few people connect are what draw us to explore this data set. 

We want to find the paths that for everyday people may seem very unreasonable, but essentially produce the desired result. There is no exact way of telling what is reasonable and not, but we would use the idea like common paths taken, to see how people navigates the pages. The story would show how people travel through Wikispeedia games similarly as some people travel through roads and highways.


# Research Questions

## 1. What are the common inferring process and how they are used to connect ideas together?

Instead of using the idea of the hub, we use the idea of common sub-paths of length 2~4, which links logical ideas together. We observed that we have many recurring common paths in the Wikispeedia games, which we call highways - as they are the most commonly used paths between many articles and are largely reused in games. 

## 2. How differently people think?

For a given pair or starting and ending pages, we look at all successful user run paths that took a detour from the path where majority took and try to identify how semantically distant people are from each other. 

## 3. What are the spurious correlations between graphs of similar paths?

Correlation does not imply causation, however, it sometimes feels fun to see them this way. For example, [the website](https://www.tylervigen.com/spurious-correlations) has a lot of seemingly unrelated things correlated to death. We do not seek such a grim topic, however, we are eager to find if in some cases we can find fun and joyful correlations between Wikipedia articles.

# Proposed Additional Datasets

We currently don't have usage cases of additional dataset, but we would augment the dataset by taking all the finished paths and break it down in to sub-paths, according to our needs.

An additional tool instead of dataset that we might be using is [Wikipedia2Vec](https://wikipedia2vec.github.io/wikipedia2vec/).

# Methods

- Step 1: Clean data. There are some paths containing the back click sign, "<". We reduce the path by only keeping the paths without backtracking. We also perform unquoting on words, also we split the paths into lists for easier data processing.
- Step 2: Use the cleaned paths to establish the network. Each article name will be the node, and the edges will be the click path. To find the correlation better, we plan to set the weight as the semantic similarity score between the nodes' article plaintext.
- Step 3: Method 1 to find the correlation, Community Detection. Detect communities based on the chosen weight, and find what each community represents.
- Step 4: Method 2 to find the correlation. Also based on the chosen weight, calculate the Pearson Correlations on complex networks. [Pearson Correlations on Complex Networks](https://www.michelecoscia.com/wp-content/uploads/2021/10/nvd_corr.pdf)
- Step 5: Method to find the spurious graph correlations.
The idea is mainly to find spurious graph correlations. https://www.tylervigen.com/spurious-correlations
We take the ingoing and outgoing edge paths for novels like Harry Potter (HP) and find other Wikipedia articles closely related to ingoing and outgoing edges. For example:
Tabasco ->(50/300 paths)-> Harry Potter ->(10/100 paths) ->Chocolate 		
Tabasco ->(100/600 paths) Pizza ->(20/200 paths) 
If we normalize the ratios of an incoming number of edges Tabasco->X, we have ⅙ for incoming and 1/10 for outgoing. Does that maybe mean Harry Potter = Pizza? We could try to run this through for all the data and handpick the most fun ones
Or in another way, we can see what edges are used the most, see the top starting / ending notes that pass through them, and see if there are any interesting pairings coming out of them.
- Step 6: Difference between distance metrics
With the Wikispeedia dataset and other NLP models, we have several ways to measure the 'distance' between words.
We've proposed at least 3 ways for measuring the distance, i
See how semantic distances of word models(BERT, Word2Vec) relate to shortest path distances in the Wikispeedia game.
The shortest path distance would indicate the “semantic wiki distance” between Wikipedia articles. 
Later we could compare the generated distances of language models and “our Wikipedia distances” to see if Wikipedia connects very semantically distant words.
We are trying to find semantically disconnected words that are very closely connected by(user navigation blue links) Wikipedia articles.
We have a lot of data for this since we can reuse all the subpaths of the runs.
We can explore with shortest paths and with user paths

# Proposed timeline



# Team Organization

# Questions for TAs (optional): Add here any questions you have for us related to the proposed project.

N/A