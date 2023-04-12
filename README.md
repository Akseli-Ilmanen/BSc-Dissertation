# Quantitative phenomenology of altered time perception

Consider these quotes: 
> "As to the 'present', well ... perhaps the true line of our perceived travel in time through reality is just going outwards, from the centre of a circle towards     the circumference, on an infinite lollipop candy-like spiral; lines in the spiral all featuring eternities of time itself in various repetitions, and the travel in     reality prime just being a movement from one candy-line of infinite possibilities to the next, along only one given instance of each realised possibility."
 
 > "At one point I asked how long it had been and I received the answer "5 minutes", that was after some eternities."

These are quotes from a psychoactive substance report from the [Erowid Experience Vault](https://erowid.org/experiences/exp_front.shtml/).[^1] Within, the total of nearly 40,000 reports, there are thousands of phrases indicative of altered time perception. Let's go explore them!

## Code

All the code used for this (ongoing) dissertation is accessible in Jupyter Notebook format and supplemented with extensive notes. Feel free to use it. 

The code is currently incomplete, the rest of it is coming soon.
 
## Context-window co-occurence network: Method

This diagram explains the main idea of this method briefly.

![image](https://user-images.githubusercontent.com/107996462/230783450-5b42ed51-ee46-4f4f-9d7b-137af0257616.png)

<br />

## Context-window co-occurence network: Example

Below is a co-occurence network with seed words determining the colour. It was created applying the method above to all the psychoactive substance reports. 

For an interactive similar version, click [here](https://akseli-ilmanen.github.io/Online-Gephi-Test/network). The interactive version works only on a browser not on a phone. The clustering algorithm applied is similar but there nodes and edges are coloured not by seed word but by their local cluster.

#### Graph 1
![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Images/Github.svg)

![image](https://user-images.githubusercontent.com/107996462/207780649-8a6e5feb-7ece-47ef-a606-95caf77fab72.png)

These graphs are created in Gephi[^4], using the modularity clustering algorithm[^5], commonly known as the [Louvain algorithm](https://en.wikipedia.org/wiki/Louvain_method), and the 'Circle Pack' layout plugin. The colour gradient labelling was created using a Sigmoid function, passing through the origin. (Details coming soon). The size of the nodes corresponds to their [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)).


The graph above has 1277 nodes 4,912 edges. This is a subset of all the possible 13,870 nodes 1,091,046 edges that were collected using the method above (context window = 4). A larger graph using all these nodes and edges, such as the one below, may actually tell us less.

#### Graph 2

![image](https://user-images.githubusercontent.com/107996462/230783406-331be6c8-ddd8-4646-913a-1bd7d735c7cb.png)


## Node & edge filtering problem[^2] 

To create the smaller and more meaningful graph, edges have to be filtered by some approach. One could filter all edges with a weight below a global threshold. Yet, because of the heavy-tailed distribution of word frequencies (see also [Zipf's law](https://en.wikipedia.org/wiki/Zipf%27s_law)), this would disproportionately affect rare words, potentially important for the phenomeology of time perception. 

Here, I solved this problem through the following steps: 

### Step 1: 

The assumption for this step is that words co-occuring frequently nearby the seed words but less frequently in other context are the most important words related to time. This is losely inspired by [tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).


#### Equation 1

$$rf = \frac{f_{Time}}{f_{Erowid}}$$

The relative frequency $rf$ captures word frequency in the Time corpus (all context windows concatenated) relative to its frequency in the entire Erowid corpus. (Co-occurence networsk of specific classes :arrow_down: will consider $rf$ from their Class corpus).

- [x] Filter all the edges where neither node is in the top k<sup>th</sup> percentile of words in the time corpus sorted by their $rf$ scores. (A stronger version of this filters all edges where if any one of the two nodes is not in the top k<sup>th</sup> percentile).

### Step 2: 

Step 1 tends to disproportionately favour rare words, as they may stochastically appear close by time words and never outside the Time corpus. Yet, if these rare words reappear in similar word contexts, we may assume they represent semantically coherent phenomena. I assume in a co-occurence network, words representing a semantically coherent phenomena would cluster together. Therefore, the network measure [betweeness centrality](https://en.wikipedia.org/wiki/Centrality#Betweenness_centrality) might capture which words are central to this clustering. 

- [x] Filter all the edges where either of the two nodes is not in the top k<sup>th</sup> percentile sorted by their betweeness centrality scores (This step is similar to the 'strong version' of step 1).

Betweeness centrality scores were calculated using NetworkX[^3]. For formulae and code see [here](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.centrality.betweenness_centrality.html).

**Why step 1 before step 2**: As you can see in graph 2, some of the major nodes are 'drug', 'feel', 'life' or 'experience'. They are very frequent across the Erowid corpus. If one were to calculate the betweeness centrality for a network of all the words in the Erowid corpus or Time corpus, they would have the highest scores. However, if one calculates the betweeness centrality for a subset of words (with high $rf$), the words 'space', 'peak' and 'check' have the highest betweeness centrality. I think the latter is telling us more about the phenomeology of time perception. 

### Step 3: 

Equation 2 considers the weight of an edge relative to the degree and and strength of a node, thus filter all edges that do "not carry a disproportionate fraction of a node's strength"[^2].

#### Equation 2[^2]:

$$p_{ij} = (1 - \frac{w_{ij}}{s_{i}})^{k_{i} - 1}$$

$w_{ij}$ is the weight of an edge. $k_{i}$ and $s_{i}$ are the [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)) and strength of a node<sub>i</sub>. The strength is a weighted version of degree by multiplying the sum of all the weights of edges to/from that node.

- [x] Filter all the edges where the probability $p_{ij}$ of an edge<sub>ij</sub> (of node<sub>i</sub> and node<sub>j</sub>) was above a certain threshold. 

This step was particularly important to remove many hub node - island node pairs. These were pairs where a hub word such as 'heart' co-occurred with an obscure word X, and X never co-occured with any other node in the network. Since, they majority of the words were island nodes, not excluding these pairs, resulted in network structures where every hub node is surrounded by and island of island nodes. This makes it difficult to understand how intermediate or hub nodes relate to each other.

## Comparing psychoactive classes

Psychoactive substances were categorized into classes using the classification scheme from[^6].

<table>
<thead>
<tr>
<th style="font-size: 16px; width: 50%">Co-occurence networks</th>
<th style="font-size: 16px; width: 50%">BERTopic modelling</th>
</tr>
</thead>
<tbody>
<tr>
<td style="width: 50%">Psilocybin_mushroms (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Gephi_unedited_Psilocybin_mushroms.png.png" alt="Psilocybin_mushroms co-occurrence network">
</td>
<td style="width: 50%">Psilocybin_mushroms (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Picture1.png" alt="BERTopic model">
</td>
</tr>
<tr>
<td style="width: 50%">LSD (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Gephi_unedited_LSD.png" alt="LSD co-occurrence network">
</td>
<td style="width: 50%">LSD (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Picture1.png" alt="BERTopic model">
</td>
</tr>
<tr>
<td style="width: 50%">DMT (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Gephi_unedited_DMT.png" alt="DMT co-occurrence network">
</td>
<td style="width: 50%">DMT (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Picture1.png" alt="BERTopic model">
</td>
</tr>
<tr>
<td style="width: 50%">Cannabis spp. (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Gephi_unedited_Cannabis_spp.png" alt="Cannabis co-occurrence network">
</td>
<td style="width: 50%">Cannabis spp. (Substance)<br>
<img src="https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Co-occurrence_png/Picture1.png" alt="BERTopic model">
</td>
</tr>
<tr>
</table>
    
       
       

## BERTopic analysis

I am also using [BERTopic library](maartengr.github.io/BERTopic/index.html)[^7] to identify topics across Erowid reports. Below is a screenshot for one of my graphs. There is a lot going on, so I recommend using the [interactive version](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/407b29eef8ef677def4d59cd2b5e12fccc71a5d7/BERTopic%20Plotly.html) to play around with this graph.

![image](https://user-images.githubusercontent.com/107996462/229903690-f6df344a-af8d-48d9-b325-8bad01d98661.png)

If you are interested in the details of these topics, use the [Erowid Quotes finder tool](https://akseli-ilmanen.github.io/BSc-Dissertation/) to find quotes for specific topics. There you can filter topics by class or substance. More coming soon!

## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: See pp.109 in Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^3]: Hagberg, A.A., Schult, D.A. and Swart, P.J., 2008. Exploring Network Structure, Dynamics, and Function using NetworkX.
[^4]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^5]: Vincent D Blondel, Jean-Loup Guillaume, Renaud Lambiotte, Etienne Lefebvre, Fast unfolding of communities in large networks, in Journal of Statistical Mechanics: Theory and Experiment 2008 (10), P1000
[^6]: Sanz, C., Zamberlan, F., Erowid, E., Erowid, F., & Tagliazucchi, E. (2018). The Experience Elicited by Hallucinogens Presents the Highest Similarity to Dreaming within a Large Database of Psychoactive Substance Reports. Frontiers in Neuroscience, 12. https://www.frontiersin.org/articles/10.3389/fnins.2018.00007
[^7]: Grootendorst, M., 2022. BERTopic: Neural topic modeling with a class-based TF-IDF procedure [Online]. [Online]. Available from: https://doi.org/10.48550/arXiv.2203.05794 [Accessed 23 March 2023].
