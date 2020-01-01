---
layout: post
title:  "The Tennessee Eastman Process: an Open Source Benchmark"
date:   2019-11-30 10:09:09 +0100
categories: [Engineering, Anomaly Detection, TEP]
tags: [Engineering, Anomaly Detection, TEP]
---

When you look through papers on anomaly detection in the field of process engineering, one name above all others keeps on cropping up: the Tennessee Eastman Process (TEP). The TEP is essentially a real industrial process that was modeled computationally in 1993 by [Downs and Vogel](https://www.sciencedirect.com/science/article/pii/009813549380018I). What makes it especially important is that the data recovered from this study is consistently used for comparing and benchmarking algorithms; in  particular anomaly detection algorithms. As such, I wanted to dedicate a blog post on what exactly this dataset is comprised of and what makes it so special.

The TEP is comprised of 8 chemical components in total: 4 reactants, 2 products, 1 by-product and 1 inert component. These components undergo a chemical process dominated by 5 main process units: a reactor that allows for the reaction of the gaseous feed components (A, C, D and E) into liquid products (G and H), a condenser to cool-down the gaseous product stream coming out of the reactor, a gas-liquid separator to split gas and liquid components from the cooled product stream, a centrifugal compressor to flow this gas stream back into the reactor and a stripper to handle the efficient separation of the 2 products from any unreacted feed components. There is also a purge to remove the inert (B) and the by-product (F) from the system . An overview of the industrial process is given below. 

![](https://www.ieee-dataport.org/sites/default/files/TE_flow.jpg)

From this entire process, we can get over 50 different variables that record properties of the system such as the flowrates, pressures, temperatures, levels, mole fractions and compressor power outputs. Over 10 of these are manipulated variables (flowrates, valve positions and the reaction agitator speed) which the operator can control to ensure that the chemical process is operating under control. At the current time of writing, one of the most prevalent sources of TEP data comes from [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/6C3JR1), which is the dataset set referenced in [Rieth et al. (2017)](https://link.springer.com/chapter/10.1007/978-3-319-60384-1_6). In this dataset, process variables are sampled every 3 minutes for 25 hours in the training dataset, and 48 hours for the testing dataset. 

Now one of the reasons that this dataset is so widely used for benchmarking anomaly detection algorithms is that it contains both a 'fault-free' and 'faulty' datafile. The former corresponds to the values of the TEP process when under normal operation while the latter contains 20 different simulated process faults. In theory, a good anomaly detection algorithm shouldn't give any false-positives for the fault-free dataset while at the same time catching as many of the 20 faults introduced in the faulty dataset as possible. 

An additional requirement for the anomaly detection algorithm is interpretability, i.e. not only being able to detect the fault, but also specifying what caused it to occur. Was it due to an incorrect flowrate set point? Or potentially a faulty valve? Thanks to this dataset, we know that the fault was caused by abnormal behaviour one of the process variables. The ability to pinpoint the offending variable is an important challenge to any anomaly detection algorithm. 

Using this dataset, I hope to explore with you how different algorithms fare in fault detection and diagnosis of an industrial process. Until then, I will leave you with links to some papers that use the TEP data to test their anomaly detection algorithms. 

* [Statistical Process Monitoring of the Tennessee Eastman Process Using Parallel Autoassociative Neural Networks and a Large Dataset](https://www.mdpi.com/2227-9717/7/7/411/htm)
* [A comparison study of basic data-driven fault diagnosis and process monitoring methods on the benchmark Tennessee Eastman process](https://www.sciencedirect.com/science/article/pii/S0959152412001503)
* [Fault Detection and Identification using Bayesian Recurrent Neural Networks](https://arxiv.org/abs/1911.04386)

