## Graph-driven comparative phenomenology of altered time perception in over 20,000 trip reports

This project is part of my BSc Dissertation. I study trip reports describing experiences with psychoactive substances from the [Erowid Experience Vault](https://erowid.org/experiences/)[^1]. Using graphs (see below!), I explore what kind of subjective expereriences are shared across or are particular to different substances.

You can read the full dissertation [here](https://drive.google.com/file/d/17jmOFxTU8bhnK_V7ocVQX6vw0m21eBJt/view?usp=share_link).

## Code

All the code used for this project is accessible in Jupyter Notebook format and supplemented with extensive notes. To replicate my findings for the co-occurrence networks or BERTopic graphs, use the following files in the folder 'Code' in this order. 

**Co-occurrence networks**
- Erowid Data Collection
- Pre-processing 1
- Pre-processing 2
- Pre-processing 3
- Get Time corpus 1
- Get Tf-ldf
- Analysis 1

**BERTopic graphs**
- Erowid Data Collection
- Pre-processing 1
- Pre-processing 2
- Get Time corpus 2
- Analysis 2

**Extra**
- Null model for analysis 1
- Other analyses

## Context-window co-occurence network: Method

This diagram explains the main idea of this method.

![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/analysis_1_method.png)

<br />

## Context-window co-occurence network: Example

Below is a co-occurence network with seed words determining the colour. It was created applying the method above to the class of Serotonergic psychedelics. 

 
<div align="center">
    <img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/f7ad1e99436bd1766029ea1cd64f12fd014c4bf7/Files/Gephi_Serotonergic_psychedelics.svg"   width="800" height="800">
</div>


For an in-depth walkthrough on how this graph was created, see this [YouTube video](https://www.youtube.com/watch?v=U1zzyvW_WjM&list=PLAI16F70Jqg2MmfgT78a60as-cae0amVK).

The brief version: This grah was created in Gephi[^4], using the modularity clustering algorithm[^5], commonly known as the [Louvain algorithm](https://en.wikipedia.org/wiki/Louvain_method), and the 'Circle Pack' layout plugin. The Circle Pack layout creates local clusters (circles), where nodes belonging to the same modularity class  are clustered in the same circle. In this sense, we can interpret from the graph that words in the same local clusters might be related. However, one should not read too much into the global structure of the clusters, as their arrangement is not very robust when changing around few parameters (as demonstrated [here](https://youtu.be/U1zzyvW_WjM?t=410)). 

The colour of nodes and edges are determined by the type of seed words they tend co-occur in (see [methods](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/README.md#context-window-co-occurence-network-method) visualization). The size of the nodes corresponds to their [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)), whilst the thickness of edges corresponds to their [weight](https://www.sciencedirect.com/science/article/abs/pii/S0167506008706149). 

This graph has 848 nodges and 1,272 edges, which is a small subset of all the possible 12,550 nodes and 676,305 edges for this class. This larger graph would be too large to interpret, therefore the majority of nodges and edges were filtered. One could filter all edges with a weight below a global threshold. Yet, because of the heavy-tailed distribution of word frequencies (see also [Zipf's law](https://en.wikipedia.org/wiki/Zipf%27s_law)), this would disproportionately affect rare words, potentially important for the phenomeology of time perception. 

Here this problem was solved by first impelementing a variation of the [tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) equation, and only keeping edges where both nodes are in the top k<sup>th</sup> percentile of words sorted by their $tfidf$ scores. Next the equation below was used to filter all edges that do "not carry a disproportionate fraction of a node's strength"[^2].

$$p_{ij} = (1 - \frac{w_{ij}}{s_{i}})^{k_{i} - 1}$$

$w_{ij}$ is the weight of an edge. $k_{i}$ and $s_{i}$ are the [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)) and strength of a node<sub>i</sub>. The strength is a weighted version of degree by multiplying the sum of all the weights of edges to/from that node. If $p_{ij}$ is above a set threshold, the edge is excluded.

<br />

## BERTopic modelling: Example
As another way to interpret the data, I used the [BERTopic library](maartengr.github.io/BERTopic/index.html)[^7] to identify topics across Erowid reports. BERTopic uses [BERT embeddings](https://en.wikipedia.org/wiki/BERT_(language_model)), [umap](https://pair-code.github.io/understanding-umap/) dimensionality reduction, and a cluster-tifidf scheme, amongst other things. The input for BERTopic were documents (Erowid context windows C=15), which were clustered into topics. On the graph below (interactive version available [here](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Serotonergic%20psychedelics.html)), each node represents a document. In colour mode 0, the colour corresponds to the document's topic, whilst in colour mode 1 it is based on whether the seed word of the document was a normal, fast or slow time perception word.

#### Interactive graphs
**Hint**: Double-click on a topic in the legend unselects all other topics. 
[![BERTopic graphs](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/Github-gif.gif)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Serotonergic%20psychedelics.html)


## Comparing psychoactive substances and classes

One goal of this analysis is to find out which subjective experiences are similar across substances. Below are the co-occurence graphs (left) and BERTopics graphs (right) for a number of substances and classes. For the images on the right, you can access them as interactive graphs by clicking on the images. (I recommend using a desktop browser for the latter).  For the classification scheme of substances and classes see the [supplementary material](https://ars.els-cdn.com/content/image/1-s2.0-S105381001830535X-mmc1.pdf) of Martial et al., 2019[^6].

If you are interested in examples for the topics on the right, you can use the [Erowid Quotes finder tool](https://akseli-ilmanen.github.io/BSc-Dissertation/) to find quotes for specific topics. There you can filter topics by class or substance.


| Co-occurence networks          | BERTopic modelling                          |
:-------------------------:|:-------------------------:
Serotonergic psychedelics (Class) | Serotonergic psychedelics (Class) 
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/aa4b89b8f0189f7ffa606cedd6b01bf936bd5fd7/Files/Co-occurrence_SVG/Gephi_edited_Serotonergic_psychedelics%20no%20title.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Serotonergic_psychedelics.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Serotonergic%20psychedelics.html) 
Psilocybin mushrooms (Substance) | Psilocybin mushrooms (Substance)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_Psilocybin_mushrooms%20.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Psilocybin_mushrooms.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Psilocybin%20mushrooms.html)
LSD (Substance) | LSD (Substance)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_LSD.svg%20%20.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/LSD.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_LSD.html)
DMT (Substance) | DMT (Substance)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_DMT%20.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/DMT.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_DMT.html)
Dissociative psychedelics (Class) | Dissociative psychedelics (Class)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_Dissociative_psychedelics.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Dissociative_psychedelics.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Dissociative%20psychedelics.html)
Depressants/sedatives (Class) | Depressants/ sedatives (Class) 
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_Depressant_sedatives.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Depressants_sedatives.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Depressant%20sedatives.html) 
Cannabis spp. (Substance) | Cannabis spp. (Substance)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_Cannabis_spp.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Cannabis_spp.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Cannabis%20spp.html) 
Stimulants (Class) | Stimulants (Class)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_Stimulants%20.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Stimulants.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Stimulants.html) 
Antidepressants/ antipsychotics (Class) | Antidepressants/ antipsychotics (Class)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_Antidepressants_antipsychotics.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/Antidepressants_antipsychotics.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Antidepressants%20antipsychotics.html) 
MDMA (Substance) | MDMA (Substance)
<img src="https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/e2e358d007cfa0d78ff47bcbe4b15c9596dad313/Files/Co-occurrence_SVG/Gephi_edited_MDMA%20.svg"   width="1300" height="700"> | [![image](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Files/BERTopic_PNG/MDMA.png)](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_MDMA.html)


Above is a selection of co-occurence networks and BERTopic graphs. Besides these, I also created graphs for the classes Deliriants, Entactogens and the substance Salvia divinorum. For these and all other co-occurrence graphs (left), you can access them all together on this [page](https://raw.githubusercontent.com/Akseli-Ilmanen/BSc-Dissertation/12969ea7b7d52b5a7c0accb58716f46c06d898e9/Files/Co-ocurrence%20networks%20high%20resolution%20page%20optimised.svg). And for three just mentioned you can access the BERTopic interactive graphs for Deliriants ([here](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/7a74e37ea3ea1dfd8b5f8b0ec998785b5052409c/Files/BERTopic%20plots/BERTopic_plot_Deliriants.html)), for Entactogens ([here](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Entactogens.html)), and for Salvia divinorum ([here](https://rawcdn.githack.com/Akseli-Ilmanen/BSc-Dissertation/946c2c9da9e0d03a12fca9525976cb009cb58345/BERTopic%20plots/BERTopic_plot_Salvia%20divinorum.html)).



## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: See pp.109 in Menczer, F., Fortunato, S. and Davis, C.A., 2020. A First Course in Network Science. Higher Education from Cambridge University Press [Online]. Cambridge University Press. Available from: https://doi.org/10.1017/9781108653947 [Accessed 12 December 2022].
[^3]: Hagberg, A.A., Schult, D.A. and Swart, P.J., 2008. Exploring Network Structure, Dynamics, and Function using NetworkX.
[^4]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.
[^5]: Vincent D Blondel, Jean-Loup Guillaume, Renaud Lambiotte, Etienne Lefebvre, Fast unfolding of communities in large networks, in Journal of Statistical Mechanics: Theory and Experiment 2008 (10), P1000
[^6]: Sanz, C., Zamberlan, F., Erowid, E., Erowid, F., & Tagliazucchi, E. (2018). The Experience Elicited by Hallucinogens Presents the Highest Similarity to Dreaming within a Large Database of Psychoactive Substance Reports. Frontiers in Neuroscience, 12. https://www.frontiersin.org/articles/10.3389/fnins.2018.00007
[^7]: Grootendorst, M., 2022. BERTopic: Neural topic modeling with a class-based TF-IDF procedure [Online]. [Online]. Available from: https://doi.org/10.48550/arXiv.2203.05794 [Accessed 23 March 2023].
