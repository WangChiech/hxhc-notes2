---
{"date":"2022-09-30","time":"10:54","tags":[],"cssclass":null,"dg-publish":true,"dg-class":"algorithm","dg-show-local-graph":true,"permalink":"/2-areas/reading/data-driven-fdd-for-complex-systems/multiblock-modeling/","dgHomeLink":false,"dgPassFrontmatter":true,"dgShowLocalGraph":true,"dgShowBacklinks":true}
---


[[2. Areas/Reading/Data driven FDD for complex systems/Sparse modeling#5. Fault detection and diagnosis of dynamic processes using weighted dynamic decentralized PCA approach|Fault detection and diagnosis of dynamic processes using weighted dynamic decentralized PCA approach]]

# 9. Decentralized and Dynamic Fault Detection Using PCA and Bayesian Inference
#mutliblok #PCA

> As industrial plants tend to increase their level of automation  the number of sensors and, therefore, the amount of collected  data is huge, this implies that the requested computational  power to process this amount of data has grown signiﬁcantly. For this reason, the fault detection have became highly time  and resources consuming. In small plants fault detection can  be developed using a central processor which collects all the  information from the sensors, but in complex plants, with lot  of sensors, this central processor must be able to deal with this  vast amount of data, which is not always possible. **One way  to afford this problem is dividing the plant in blocks**, each one  containing a speciﬁc number of measured variables in it and  assigning one small processor in every block, this is called  decentralized approach. Here, each **local processor** does the  fault detection task with its own sensors and send the result  to a **central processor** which only has to fuse the local results  to obtain a **global detection** result.

> The most important step in this  technique is to establish the criteria used to divide the plant.  Some authors opted for one-variable blocks [13], while other  authors constructed blocks that group more than one variable  ( [8], [10], [12], [14]).

> Once the local fault detection models obtain a result, it is  necessary to **fuse all these results in order to get an unique  decision for the whole plant.** There are different ways to do  that as [8] that proposed Maximum Entropy, Multicriteria decision making (MCDM) methods as Ordered Weighted  Average (OWA) operators [17] are another option, also ( [12],  [14]) used Bayesian Inference Criterion (BIC) for their Fault  Detection and Diagnosis (FDD) method.

## Methods
The ﬁrst step is to decompose the plant into different blocks  and then to perform a DPCA method to detect faults locally in  each block, and, ﬁnally, a central processor fuse all the local  detection results using the BIC index to take a global decision.
### Plant decomposition
Sparse PLS, ANN

# 8. Knowledge-data-integrated sparse modeling for batch process  monitoring
#sparse-modeling #variable-group #multiblock 

## Introduction
According to process flow diagram (PFD) and piping and instrumentation diagram (P&ID) of the industrial process, the actual variable correlations are classiﬁed into four categories: control, reaction, type and location correlations.

## Methods
1. Using variable correlations based on process knowledge or similarities
     - control correlation
     - reaction and location correlation 
     - type correlation
     - similarity driven [[2. Areas/Reading/Data driven FDD for complex systems/Data driven fault detection and diagnosis for complex system| variable grouping technique]]
2. Variable correlation tendency analysis to analyze the variables in the same group are positively or negatively correlated
3. Knowledge based sparse projection matrix
    Construct a projection matrix based on the grouping and tendency result.
4. [[2. Areas/Reading/Data driven FDD for complex systems/Sparse modeling#4 Fault Detection and Diagnosis Based on Sparse PCA and Two-Level Contribution Plots|Two level contribution plot]]
5. 
## Cases
An industrial scale fed-batch fermentation process carried out in a 100000L biocreator. [link](https://www.sciencedirect.com/science/article/pii/S0168165614009377)