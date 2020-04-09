---
layout: archive
title: "Research"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

A focus of my work is to produce a fast and reproducible workflow for the application of numerical models consisting of sensitivity analyses, automatic calibration techniques, multivariate statistical methods for post-processing and output visualization that is easy to understand. I firmly believe that aquatic ecosystem models are important tools for teaching physical as well as ecological in-lake processes, are crucial to check hypotheses obtained by field data and should be made accessible to every limnologist, the stakeholders as well as the public regardless of computational power and computer science education.

Here's an animation showing how a flexible Lagrangian layer structure of a vertical hydrodynamic model ([GLM](https://aed.see.uwa.edu.au/research/models/GLM/)) is working. The thickness of each layer is either expanding or shrinking in dependence of its density which further depends on the mixing dynamics. Especially surface cooling events in the evening and at night are causing convective fluxes that deepen the surface mixed layer.

<br/><img src='/images/glm_layerstruc.gif'>

The following paragraphs give you an overview of selected current research projects of mine and projects that I am involved in.

ABI: improving modeling tools
======
Numerical lake models are essential tools for limnologists and civil engineers. In this project we are developing tools for the GLM-AED2 modeling suite to make it more robust and accessible. The vertical 1D GLM features a flexible Eulerian grid as well as an energy balance approach for mixing. Due to its fair computational needs and low requirements for boundary conditions (bathymetry, initial temperature profile and meteorological conditions), GLM is a great choice for first-time modelers as well as experienced users.

Lake Mendota: long-term lake dynamics
======
I am using GLM-AED2 to simulate Lake Mendota in Wisconsin, USA. Here, we are using long-term monitoring data to calibrate and validate model variables to fit observed to simulated data. A novel sensitivity and calibration approach supports a reproducible modeling workflow for this project. Especially the analysis of long-term dynamics of dissolved oxygen helps us to identify drivers of future anoxia.

CNH: human-catchment-lake interactions
======
To understand lake ecosystems, one also has to understand its couplings to the catchment, economic drivers and society. In this project, a fully coupled lake model is used to answer complex economic as well as biogeochemical questions.

ISIMIP: impact of climate change on lakes worldwide
======
In this project we are using GLM and GOTM to project the impact of climate change on lake thermal characteristics worldwide. Here, we are simulating more than 60 different lakes over long periods of time (centuries). It's the scientific consensus that lakes act as sentinels to climate change and will experience drastic ecological shifts.

Lake Tegel: impact of heavy rainfall on algae blooms
======
We've calibrated a depth-averaged 2D flow model (TELEMAC2D) to the urban Lake Tegel in Berlin, Germany, to investigate the impact of short-duration heavy rainfall events on the formation of phytoplankton blooms. A novelty for this approach is the application of the water quality module 'Eutro' to a real-world case study.

Here's a map showing the locations of some of the lakes I've simulated so far:
<br/><img src='/images/lakes.jpg'>
