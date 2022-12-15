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

This diagram explain the main idea of this method briefly.

<br />

![image](https://user-images.githubusercontent.com/107996462/207780805-37ac0f2a-c52e-4607-b775-ca9e181e3d57.png)

<br />

## Context-window co-occurence network: Example

Below is a co-occurence network with seed words determining the colour. For an interactive similar version, click [here](https://akseli-ilmanen.github.io/Online-Gephi-Test/network). The clustering algorithm applied is similar but there nodes and edges are coloured not by seed word but by their local cluster.

![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Images/All_Classes_th1%3D500_th2%3D1000_weights%3Dyes_.svg)

![image](https://user-images.githubusercontent.com/107996462/207780649-8a6e5feb-7ece-47ef-a606-95caf77fab72.png)

## Edge filtering problem[^2] 

The graph above consists of XXXXX nodges XXXXX edges. This is a subset of all the possible XXXXX nodges XXXXX edges. To create the smaller and more meaningful graph, edges were filtered following step 1 and 2. 

### Step 1: Excluding edges based of word types
 1) All self-connections - ['perception', 'perception']
 2) All edges (word pairs) including a non-noun. - ['feel', 'heart']
 3) All edges including a seed word  - ['time', 'moment']
 4) All edges including a word not in the Concreteness_ratings_Brysbaert Corpus[^3] ['looong', 'moment']

### Step 2: Excluding edges based of relative frequency

Equation 1 and 2 are used to determine which edges to exclude.

Equation 1:

![image](https://user-images.githubusercontent.com/107996462/207158586-9453d4ba-f2fd-465b-ae0e-d1c58579c49a.png)

This is a [sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function) passing through the origin. 
<br />

Equation 2: 

![image](https://user-images.githubusercontent.com/107996462/207158880-bdb99339-6c57-49e0-88f7-370f7dafdede.png)

$w^{'}$ and $w$ are the updated and original edge weight. $deg_{n}$, $bc_{n}$ and $rf_{n}$ refer to the [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)), [betweeness centrality](https://en.wikipedia.org/wiki/Centrality#Betweenness_centrality), and relative frequency of $node_{n}$. The relative frequency $rf_{n}$ is the fraction of a word's frequency in all the context windows compared to the entire corpus. 
<br />

Using [min-max normalization](https://en.wikipedia.org/wiki/Feature_scaling), $w$, $deg_{n}$, $bc_{n}$ and $rf_{n}$ were set to between [1,2]. 

Edges were excluded by a threshold for their updated weight $w^{'}$.

### Motivation for equations

The left part of equation 2 captures classical methods to filter edges. The problem (see also here[^2]) is that because of the heavy-tailed distribution of absolute word frequencies in the Erowid corpus, words with high weights, degree and betweeen cenetrality tend to be frequent words. Therefore, if we filter by the classical methods, we won't capture rare words that are relevant to the phenomenology of time perception. Similar to [tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf), the relative word frequency factor weights up words used more frequently in the time perception contexts. 

For intution, below are the top 15 values for $w$, $deg_{n}$, $bc_{n}$ and $rf_{n}$:

![image](https://user-images.githubusercontent.com/107996462/207424055-2309978b-7c9e-4e3e-9fcb-dd3c285e7094.png)

The sigmoid function $S(x)$ (equation 1) is applied to all the left factors in equation 2, so that rare words with relatively high weights, degree, and betweeness centrality are upweighted.

### Hyperparameters

In equation 1 and 2, there are also hyperparameters $k$ and $α$. $k$ controls the extent to which the weights, degree, and betweeness centrality of rare words are upweighted. $α$ determines how many rare time perception context words are included. These tend to be words at the very end of the tail of the heavy-tailed distribution, often having a weight and degree of 1. In theory, one could also add hyperparameters controlling the weighting of weights, degree and betweeness centrality individually.

Below are two graphs comparing hyperparameter values:
- coming soon



## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: See pp.109 in Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^3]: Brysbaert, M., Warriner, A.B. and Kuperman, V., 2014. Concreteness ratings for 40 thousand generally known English word lemmas. Behavior Research Methods [Online], 46(3), pp.904–911. Available from: https://doi.org/10.3758/s13428-013-0403-5.
[^4]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^5]: Vincent D Blondel, Jean-Loup Guillaume, Renaud Lambiotte, Etienne Lefebvre, Fast unfolding of communities in large networks, in Journal of Statistical Mechanics: Theory and Experiment 2008 (10), P1000


