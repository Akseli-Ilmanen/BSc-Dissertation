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


## Link (edge) filtering problem[^3]

The graph above consists of XXXXX nodges XXXXX edges. This is a subset of all the possible XXXXX nodges XXXXX edges. To create the smaller and more meaningful graph, edges were filtered based of the following criteria: 

#### Step 1: Excluding edges
 1) All self-connections - ['perception', 'perception']
 2) All edges (word pairs) including a non-noun. - ['feel', 'heart']
 3) All edges including a seed word  - ['time', 'moment']
 4) All edges including a word not in the Concreteness_ratings_Brysbaert Corpus[^4] ['looong', 'moment']

#### Step 2: Relative frequency - centrality trade off

Equation 1:
$$S(x) = 1/(1 + e^{-kx}) - 0.5$$

Equation 2: 
 $$w^'= nrt{(rf_{1} x rf_{2})} x S(w) x (S(deg_{1} x S(deg_{2}) x (S(bc_{1} x S(bc_{2}))$$





## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^3]: Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^4]: Brysbaert, M., Warriner, A.B. and Kuperman, V., 2014. Concreteness ratings for 40 thousand generally known English word lemmas. Behavior Research Methods [Online], 46(3), pp.904–911. Available from: https://doi.org/10.3758/s13428-013-0403-5.



