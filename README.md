# A semantic network analysis of altered time perception in 21,548 psychoactive substance reports
<img src="drawing.jpg" alt="drawing" width="200"/>
![image](https://user-images.githubusercontent.com/107996462/206793644-e2e61151-d1e9-4d54-b0cb-2fedc0d0f5c3.png)

Consider these quotes: 

> "As to the 'present', well ... perhaps the true line of our perceived travel in time through reality is just going outwards, from the centre of a circle towards the circumference, on an infinite lollipop candy-like spiral; lines in the spiral all featuring eternities of time itself in various repetitions, and the travel in reality prime just being a movement from one candy-line of infinite possibilities to the next, along only one given instance of each realised possibility."

> "At one point I asked how long it had been and I received the answer "5 minutes", that was after some eternities."
 
These are quotes from a psychoactive substance report from the [Erowid Experience Vault](https://erowid.org/experiences/exp_front.shtml/).[^1] Within, the total of nearly 40,000 reports, there are ten-to-hundred of thousands mentions of phrases indicative of altered time perception. Let's go explore them!

## Overview

All the code used for this (ongoing) dissertation is accessible in Jupyter Notebook format and supplemented with extensive notes. Feel free to use it. 

- [x] #739 AI_Diss1_erowid_data_collection.ipynb: Web scraping of 38,836 reports, with information about title, substances and url and documentent text. 
- [x] #739 AI_Diss2_diss_pre-processing1.ipynb: Exclusion of reports and categorizing into psychoactive substances and classes. 
- [x] #739 AI_Diss2_diss_pre-processing2.ipynb: Tokenization, lemmatization, removal of word types
- [x] #739 AI_Diss4_Get Time Corpus.ipynb: :arrow_down: for explanation
- [ ] Code for co-occurence network (finalising): :arrow_down: for explanation
- [ ] More stuff!
 
## Context-window co-occurence network: Method

This diagram should explain the main idea of this method. Soon, I will add more details about the implementation, and how I created the graph below.  
<br />

![image](https://user-images.githubusercontent.com/107996462/206631309-72456e73-12f9-4370-ac04-d76459e46af0.png)

<br />

## Context-window co-occurence network: Example

![alt text](https://github.com/Akseli-Ilmanen/BSc-Dissertation/blob/main/Graph1.svg?raw=true)

*This graph was created with Gephi* [^2]


## References

[^1]: Erowid E, Erowid F. "The Value of Experience". Erowid Extracts. Jun 2006;10:14-19.
[^2]: Bastian M., Heymann S., Jacomy M. (2009). Gephi: an open source software for exploring and manipulating networks. International AAAI Conference on Weblogs and Social Media.

