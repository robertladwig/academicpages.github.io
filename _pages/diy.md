---
layout: archive
title: "Tutorials"
permalink: /diy/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

# Writing a two-layer thermodynamic model in R
04-09-2020
Welcome, in this paragraph I want to briefly describe and show how you can write and run a lake model in R. When I began my PhD studies, I had a hard time wrapping my head around all the equations and coding techniques. Therefore, I want to provide a walkthrough here. Please write me a mail if you find errors or typos. Hope you enjoy it.

A great type of lake models just assumes that the lake is divided into two volume layers: the epilimnion (surface mixed layer) and the hypolimnion (bottom stagnant layer). Both layers are divided by a metalimnion (layer with a steep density gradient). This model assumption generally holds up for lakes that stratify during summer. The entrainment over the metalimnion (or thermocline) depends on a vertical diffusion coefficient, which is further simplified as a function of the diffusion at neutral stability to the Richardson number. This last assumption helps the model in dynamically changing from mixed to stratified condtions.

All equations and derivations are from Steven C. Chapra (2008) 'Surface Water-Quality Modeling' Waveland Press, Inc.

First, we can write the heat balance of the epilimnion as the change of epilimnion water temperaturen over time in dependence of an inflow source, an outflow sink, turbulent diffusion over the metalimnion between the hypolimnion and epilimnion, as well as heat fluxes from the atmosphere (more to this term later):
<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{epi}}{dt}=\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})\pm&space;\frac{J}{\rho&space;c_p&space;H}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{epi}}{dt}=\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})\pm&space;\frac{J}{\rho&space;c_p&space;H}" title="\frac{dT_{epi}}{dt}=\frac{Q}{V_{epi}}T_{in} - \frac{Q}{V_{epi}}T_{epi} + {\upsilon}_t \frac{A_t}{V_{epi}}(T_{hypo} - T_{epi})\pm \frac{J}{\rho c_p H}" /></a>

In this first ordinary differential equation Q stands for a flow discharge rate, V for different Volumes, <a href="https://www.codecogs.com/eqnedit.php?latex={\upsilon}_t" target="_blank"><img src="https://latex.codecogs.com/svg.latex?{\upsilon}_t" title="{\upsilon}_t" /></a> for turbulent diffusivity, At for the area of the thermocline, J for atmospheric heat fluxes, <a href="https://www.codecogs.com/eqnedit.php?latex=\rho" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\rho" title="\rho" /></a> for the density of water, and <a href="https://www.codecogs.com/eqnedit.php?latex=c_p" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_p" title="c_p" /></a> is the specific heat capacity of water.

Thankfully, the equation for the hypolimnion heat balance is simpler:

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" title="\frac{dT_{hypo}}{dt} = {\upsilon}_t \frac{A_t}{V_{hypo}} (T_{epi} - T_{hypo})" /></a>

More coming soon...
