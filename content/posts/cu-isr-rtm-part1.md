---
title: "Cu-ISR - Reactive Transport Modelling - Part 1"
author: "Pablo"
date: 2023-06-06T21:56:56+08:00
tags: ["copper", "in-situ recovery", "thesis", "modelling", "reactive transport", "rtm"]
draft: false
---

## Motivation

The next series of blog posts will focus on reactive transport modelling (RTM) and how it can help unlock the potential of in situ recovery for metals such as Cu. Reactive transport models, or RTMs, combine fluid dynamics, chemical equilibrium and kinetics principles, and geology to model the movement and transformation of solutes in complex geological systems. As mentioned in the previous post, ISR mainly operates in complex geological settings. Many past endeavours had failed to be economically feasible, with little information about why they failed. I think the failure comes from the complexity of ore, which is usually overlooked. 

There are two main aspects where RTM can be beneficial for ISR. The first is to help understand complex laboratory experiments, and the second is to forecast copper recovery at an operational scale while estimating the inherent uncertainty of the results. Traditionally in metallurgy (especially hydrometallurgy), laboratory experiments have been the backbone for understanding copper extraction efficiency. However, these experiments often provide limited insight into the complex dynamics of in situ recovery. 

The implications of RTMs extend far beyond the laboratory. By accurately predicting the behaviour of copper dissolution and transport in the subsurface, these models offer a powerful tool for forecasting the performance of ISR on a larger scale. From assessing the economic viability of potential mining sites to optimizing extraction methods, the ability to make data-driven decisions based on reliable simulations has the potential to revolutionize the mining industry.

In this RTM Part 1, I'll be focusing on how RTM can help extract more information from (hydro)metallurgical experiments for ISR, which was the focus of my PhD and first research paper[^1]. In the next series, we will go together on how to build an RTM for experimental and large-scale predictive purposes using open-source codes, Python and PEST. 

## What are the limitations of ISR experiments?
One of the crucial aspects of ISR is the coupled nature of solute transport and in-situ geochemical processes. The traditional set of experimental tests for ISR, however, usually remove one or the other. For example, bottlerolls experiments remove the transport part of the dynamic to focus only in the dissolution chemistry. Columns experiments, in the other hand, are a good, but not perfect, options to include coupled processes. Most of the time results from column experiments can mask underlying geochemical processes that can be important at larger time and spatial scales. 


[![brolls](/broll.gif "Bottlerolls")](https://www.911metallurgist.com/blog/bottle-roll-leach-test-leaching-procedures)


## Cu-ISR column experiments
Here I'm going to describe the experiments we did for this study. We did three column experiments with material obtained from a prospective site in South Australia (Kapunda). We used ferric sulphate and sulphuric acid to leach the samples and, interestingly, natural groundwater to make up the solutions. We programmed the experiments to have multi-stage leaching starting with water only (from the site) and progressing to acidic leaching and eventually to oxidative leaching, testing progressively higher concentrations of the oxidation. The acid concentration was kept the same during all the stages. We collected solution samples during all the experiments but characterised the material (QXRD and QEMSCAN) only before starting the tests. If you are not familiar with column experiments, they look like the following:

![column experiments](/kapunda_graphical_abs_v1.svg "Column Experiments")



## Reactive transport models of the columns
We used PHT3D, which couples MODFLOW-2005, MT3DMS and PHREEQC to model the columns. The columns were represented as a one-dimensional domain divided into small grid cells. The models simulated the leaching process over the experiment's total duration, with the initial conditions based on the measured and estimated mineral concentrations. We used the WATEQ4F database for the reaction network. After some initial tests, we realised that some preferential flow paths existed in the columns, so we decided to go for a dual-domain mass transfer approach to simulate this behaviour. 

We simulated all the mineral dissolution as kinetic, which is a good approach to this kind of experiment. If you are a geochemist and want to know which minerals we simulated, here they are:

| Mineral      | Dissolution Rate Reference           |
|--------------|--------------------------------------|
| Chalcopyrite | [Kimball (2010)](https://doi.org/10.1016/j.apgeochem.2010.03.010)       |
| Chalcocite   | [Hashemzadeh (2019)](https://doi.org/10.1016/j.hydromet.2019.04.025) |
| Chalcanthite | Depending on thermodynamic drive     |
| Malachite    | [Nicol (2018)](https://doi.org/10.1016/j.hydromet.2018.03.017)        |
| Pyrite       | [Rimstidt (1993)](https://doi.org/10.1016/0016-7037(94)90241-0)           |


## Main results

The following images and texts show the main results arising from the fit of the models to the experimental data.

- The presence of flow paths in the columns influenced Cu breakthrough behaviour. The dual-domain mass transfer (DDMT) approach, accounting for mass transfer between mobile and immobile domains, provided a better match with experimental observations than single-domain models, especially regarding tailing shapes in the curves (see green vs pink fits).

![Main Fit](/calib_main_sd_added.svg "RTM results")

- The model simulations helped understand the contribution of different Cu minerals to overall Cu recovery in each leaching stage. The fraction of Cu recovery attributed to specific minerals varied between stages. Dissolution of Cu oxides dominated in the early stages, and Cu sulphides became more significant in later stages with higher oxidant concentrations. The following image shows the simulated recovery percentages from each mineral for each stage.

![Mineral recovery](/mineral_contrib_recov.svg "RTM simulated recoveries per stage")


Finally, we also simulated gangue mineral reactions, from where we suggested the following:

- The samples had a lot of pyrite, and pyrite usually has a lot of impurities. We improved the fit of cobalt (Co) and nickel (Ni), including a fraction of those into the pyrite structure. 

- We also speculated about jarosite precipitation during the experiments and how it helped us fit K. If higher enough amounts of jarosite precipitate in the field (common in Acid Mine Drainage) would generate an operational issue. 


I hope you've enjoyed learning a bit about Cu-ISR, experiments and reactive transport modelling. If you want to get deeper into these experiments and models, please go ahead and read my paper. In the next post, I'll try to go step-by-step on how to build a model of this kind using open-source code.

[^1]: Ortega-Tong, P., Jamieson, J., Kuhar, L., Faulkner, L., Prommer, H. 2023. In situ recovery of copper: identifying mineralogical controls via model-based analysis of multistage column leach experiments. ACS ES\&T Eng. https://doi.org/10.1021/acsestengg.2c00404
