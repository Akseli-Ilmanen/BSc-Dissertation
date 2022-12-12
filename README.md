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

This diagram should explain the main idea of this method. Soon, I will add more details about the implementation, and how I created the graph below.  


<br />

![image](https://user-images.githubusercontent.com/107996462/206631309-72456e73-12f9-4370-ac04-d76459e46af0.png)

<br />

## Context-window co-occurence network: Example

Below is a co-occurence network with seed words determining the colour. For an interactive similar version, click [here](https://akseli-ilmanen.github.io/Online-Gephi-Test/network). The clustering algorithm applied is similar but there nodes and edges are coloured not by seed word but by their local cluster.

![alt text](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Graph1.svg?raw=true)

*This graph was created with Gephi* [^2]


## Edge filtering problem[^3]: Method 

The graph above consists of XXXXX nodges XXXXX edges. This is a subset of all the possible XXXXX nodges XXXXX edges. To create the smaller and more meaningful graph, edges were filtered based of the following criteria: 

### Step 1: Excluding edges
 1) All self-connections - ['perception', 'perception']
 2) All edges (word pairs) including a non-noun. - ['feel', 'heart']
 3) All edges including a seed word  - ['time', 'moment']
 4) All edges including a word not in the Concreteness_ratings_Brysbaert Corpus[^4] ['looong', 'moment'] - see later!

### Step 2: Relative frequency - centrality trade off

Equation 1 and 2 are used to determine which edges to exclude. 

1) Using min-max normalization, weights, degree, betweeness centrality and relative frequency were all between [0, 1].

2) Equation 1:

![image](https://user-images.githubusercontent.com/107996462/207115362-688e18b2-906e-4b3b-a817-2da0d36497ad.png)

This is a [sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function) passing through the origin. 
<br />

3) Equation 2: 

![image](https://user-images.githubusercontent.com/107996462/207112040-ee073702-7ccc-4144-a598-818770aa24e0.png)

$w^{'}$ and $w$ are the updated and original edge weight. $deg_{n}$, $bc_{n}$ and $rf_{n}$ refer to the [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)), [betweeness centrality](https://en.wikipedia.org/wiki/Centrality#Betweenness_centrality), and relative frequency of node_{n}. The relative frequency $rf$ is the fraction of a word's frequency in all the context windows compared to the entire corpus.
<br />

Using [min-max normalization](https://en.wikipedia.org/wiki/Feature_scaling) $w$, $deg_{n}$, $bc_{n}$ and $rf_{n}$ were set to between [0,1]. 

4) Exclude edges by the threshold for their updated weight $w^{'}$.

### Motivation for equations

The left part of equation 2 captures classical methods to filter edges. The problem (see also here[^3]) is that because of the heavy-tailed distribution of absolute word frequencies in the Erowid corpus, words with high weights, degree and betweeen cenetrality tend to be frequent words. Therefore, if we filter by the classical methods, we won't capture words that are significant to the phenomenology of time perception but rare overall. Similar to [tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf), the relative word frequency factor weights up words used more frequently in the time perception contexts. 

The sigmoid function $S(x)$ (equation 1) is applied to all the left factors in equation 2, so that rare words with relatively high weights, degree, and betweeness centrality are upweighted. Frequent words with high 

### Hyperparameters

In equation 1 and 2, there are also hyperparameters $k$ and $α$. $k$ controls the extent to which the weights, degree, and betweeness centrality of rare words are upweighted. $α$ determines how many rare time perception context words are included. These tend to be words at the very end of the tail of the heavy-tailed distribution, often having a weight and degree of 1. 

The following graph illustrates the influence of hyperparameters:




## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^3]: Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^4]: See pp.109 in Brysbaert, M., Warriner, A.B. and Kuperman, V., 2014. Concreteness ratings for 40 thousand generally known English word lemmas. Behavior Research Methods [Online], 46(3), pp.904–911. Available from: https://doi.org/10.3758/s13428-013-0403-5.



