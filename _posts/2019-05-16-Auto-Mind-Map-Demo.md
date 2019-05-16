---
layout: post
title:  Automatic Realtime Mind Map Builder From Mic.
excerpt_separator: <!--more-->
---

### MM-demo
MM-demo is a project aimed at building a mind map of conversation in real time. The operation of MM-demo can be divided into several stages:

<!--more-->


1. Recognition of words, phrases and sentences for a certain period of time.
2. Finding named entities (NE) and noun phrases (NP).
3. Calculation NE and NP weights, their position weights and average weights. Bidirectional Encoder Representations from Transformers (BERT) can be used to find weights.
4. Selection of the most significant entities among NE and NP.
5. Calculation of the relationship coefficients (relationship matrix – Fig. 1) between all significant entities on the basis of our own d2v model.
6. Selection of the root node, several second level nodes (for example, five), as well as third level nodes.
7. Plotting just created graph as a mind map.



Schematically, the operation of MM-demo is shown in Fig. 2.

<img src="http://blog.ai-labs.org/media/images/relationship-matrix.png" width="100%">

Fig. 1. An example of relationship matrix

<img src="http://blog.ai-labs.org/media/images/operation-of-MM-demo.png" width="100%">

Fig. 2. The operation of MM-demo