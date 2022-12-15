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

Below is a co-occurence network with seed words determining the colour. For an interactive similar version, click [here](https://akseli-ilmanen.github.io/Online-Gephi-Test/network). The interactive version works only on a browser not on a phone. The clustering algorithm applied is similar but there nodes and edges are coloured not by seed word but by their local cluster.

![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Images/All_Classes_th1%3D500_th2%3D1000_weights%3Dyes_.svg)

![image](https://user-images.githubusercontent.com/107996462/207780649-8a6e5feb-7ece-47ef-a606-95caf77fab72.png)

These graphs are created in Gephi[^4], using the modularity clustering algorithm[^5] and the 'Circle Pack' layout plugin. The colour gradient labelling was created using a Sigmoid function, passing through the origin. (More details coming soon).

## Edge filtering problem[^2] 

The graph above consists of 799 nodges 9,288 edges. This is a subset of all the possible 13,870 nodges 1,091,046 edges that were collected using the method above (context window = 4). To create the smaller and more meaningful graph, edges were filtered following step 1 and 2. 

### Step 1: Excluding edges based of word types
 1) All self-connections - ['perception', 'perception']
 3) All edges including a seed word  - ['time', 'moment']
 4) All edges including a word not in the Concreteness_ratings_Brysbaert Corpus[^3] ['looong', 'moment']

### Step 2: Excluding edges based of relative frequency and [betweeness centrality](https://en.wikipedia.org/wiki/Centrality#Betweenness_centrality)

More details coming soon!




## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: See pp.109 in Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^3]: Brysbaert, M., Warriner, A.B. and Kuperman, V., 2014. Concreteness ratings for 40 thousand generally known English word lemmas. Behavior Research Methods [Online], 46(3), pp.904–911. Available from: https://doi.org/10.3758/s13428-013-0403-5.
[^4]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^5]: Vincent D Blondel, Jean-Loup Guillaume, Renaud Lambiotte, Etienne Lefebvre, Fast unfolding of communities in large networks, in Journal of Statistical Mechanics: Theory and Experiment 2008 (10), P1000


