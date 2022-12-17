# A semantic network analysis of altered time perception in 21,548 psychoactive substance reports

Consider these quotes: 
> "As to the 'present', well ... perhaps the true line of our perceived travel in time through reality is just going outwards, from the centre of a circle towards     the circumference, on an infinite lollipop candy-like spiral; lines in the spiral all featuring eternities of time itself in various repetitions, and the travel in     reality prime just being a movement from one candy-line of infinite possibilities to the next, along only one given instance of each realised possibility."
 
 > "At one point I asked how long it had been and I received the answer "5 minutes", that was after some eternities."

These are quotes from a psychoactive substance report from the [Erowid Experience Vault](https://erowid.org/experiences/exp_front.shtml/).[^1] Within, the total of nearly 40,000 reports, there are ten-to-hundred of thousands mentions of phrases indicative of altered time perception. Let's go explore them!

## Overview

All the code used for this (ongoing) dissertation is accessible in Jupyter Notebook format and supplemented with extensive notes. Feel free to use it. 

- [x] AI_Diss1_erowid_data_collection.ipynb: Web scraping of 38,836 reports, with information about title, substances and url and documentent text.
- [x] AI_Diss2_diss_pre-processing1.ipynb: Exclusion of reports and categorizing into psychoactive substances and classes. 
- [x] AI_Diss2_diss_pre-processing2.ipynb: Tokenization, lemmatization, removal of word types
- [x] AI_Diss4_Get Time Corpus.ipynb: :arrow_down: for explanation
- [ ] Code for co-occurence network (finalising): :arrow_down: for explanation
- [ ] More stuff!
 
## Context-window co-occurence network: Method

This diagram explains the main idea of this method briefly.

<br />

![image](https://user-images.githubusercontent.com/107996462/207780805-37ac0f2a-c52e-4607-b775-ca9e181e3d57.png)

<br />

## Context-window co-occurence network: Example

Below is a co-occurence network with seed words determining the colour. For an interactive similar version, click [here](https://akseli-ilmanen.github.io/Online-Gephi-Test/network). The interactive version works only on a browser not on a phone. The clustering algorithm applied is similar but there nodes and edges are coloured not by seed word but by their local cluster.

#### Graph 1
![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Images/All_Classes_th1%3D500_th2%3D1000_weights%3Dyes_.svg)

![image](https://user-images.githubusercontent.com/107996462/207780649-8a6e5feb-7ece-47ef-a606-95caf77fab72.png)

These graphs are created in Gephi[^4], using the modularity clustering algorithm[^5] and the 'Circle Pack' layout plugin. The colour gradient labelling was created using a Sigmoid function, passing through the origin. (More details coming soon). The size of the nodes corresponds to their [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)).


The graph above consists of 799 nodges 9,288 edges. This is a subset of all the possible 13,870 nodges 1,091,046 edges that were collected using the method above (context window = 4). A 1,091,046 edges graph (Graph 2) looks like a 'hairball'[^2] with lots of insignificant links. 

#### Graph 2 (don't do this)




## Edge filtering problem[^2] 

To create the smaller and more meaningful graph, edges have to be filtered by some approach. One could filter all edges with a weight below a global threshold. Yet, because of the heavy-tailed distribution of word frequencies (see also [Zipf's law](https://en.wikipedia.org/wiki/Zipf%27s_law)), this would disproportionately affect rare words, potentially important for the phenomeology of time perception. 

Here, I solved this problem through the following steps: 

### Step 1: 

#### Equation 2

The assumption for this step is that words co-occuring frequently nearby the seed words but less frequently in other context are the most important words related to time. This is losely inspired by [tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

$$rf = \frac{f_{Time corpus}}{f_{Erowid corpus}$$

The relative frequency $rf$ captures how frequent a word is in the Time corpus (in one of the seed word context windows) relative to its frequency in the entire Erowid corpus. 

- [x] Filter all the edges where neither node is in the top xxth percentile of words in the time corpus sorted by their $rf$ scores. (A stronger version of this filters all edges where if any one of the two nodes is not top xxth percentile).

### Step 2: 

Step 1 tends to disproportionately favour very rare words, as they may stochastically appear close by time words and never outside the Time corpus. Yet, if these rare words reappear in similar word contexts, we may assume they represent a semantically related phenomena. 

To capture words central to this phenomena and potentially connecting rare words with high $rf$, calculate the [betweeness centrality](https://en.wikipedia.org/wiki/Centrality#Betweenness_centrality) for all the nodes remaining after step 1. The betweeness centrality was calculated using NetworkX[^3]. For formulae and code see [here](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.centrality.betweenness_centrality.html).

- [x] Filter all the edges where either of the two nodes is not in the top xxth percentile sorted by their betweeness centrality scores (This step is similar to the 'strong version' of step 1).

**Why step 1 before step 2**: As you can see in the ['hairball'] graph 2, some of the major nodes are 'drug', 'feel', 'life' or 'experience'. They are very frequent across the Erowid corpus. If one were to calculate the betweeness centrality for a network of all the words in the Erowid corpus or Time corpus, they would have the highest scores. However, if one calculates the betweeness centrality for a subset of words (with high $rf$), the words 'heart', 'peak' and 'space' have the highest 

### Step 3: 

Equation 2 considers the weight of an edge relative to the degree and and strength of a node, thus filter all edges that do "not carry a disproportionate fraction of a node's strength"[^2].

#### Equation 2[^2]:
$$p_{ij} = (1 - \frac{w_{ij}}{s_{i}})^{k_{i} - 1}$$

$w_{ij}$ is the weight of an edge. $k_{i}$ and $s_{i}$ are the [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory))and strength of a node_{i}. The strength is a weighted version of degree by multiplying the sum of all the weights of edges to/from that node.

- [x] Filter all the edges where the probability $p_{ij}$ of an edge_{ij} (of node_{i} and node_{j}) was above a certain threshold. 

This step was particularly important to remove many hub node - island node pairs. These were pairs where a central word such as 'heart' co-occurred with an obscure word 'xxx', and 'xxx' never co-occured with any other node in the network. Since, they majority of the words are island nodes ([Zipf's law](https://en.wikipedia.org/wiki/Zipf%27s_law)), not excluding these pairs, results in a network structure where every hub node is surrounded by and island of island nodes. This makes it difficult to understand how intermediate or hub nodes relate to each other.

## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: See pp.109 in Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^3]: Hagberg, A.A., Schult, D.A. and Swart, P.J., 2008. Exploring Network Structure, Dynamics, and Function using NetworkX.
[^6]: Brysbaert, M., Warriner, A.B. and Kuperman, V., 2014. Concreteness ratings for 40 thousand generally known English word lemmas. Behavior Research Methods [Online], 46(3), pp.904–911. Available from: https://doi.org/10.3758/s13428-013-0403-5.
[^4]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^5]: Vincent D Blondel, Jean-Loup Guillaume, Renaud Lambiotte, Etienne Lefebvre, Fast unfolding of communities in large networks, in Journal of Statistical Mechanics: Theory and Experiment 2008 (10), P1000


