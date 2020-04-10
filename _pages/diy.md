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

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{epi}}{dt}&space;=&space;\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon&space;}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})&space;\pm&space;\frac{A_{surf}}{V_{epi}&space;\rho&space;c_p}&space;J" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{epi}}{dt}&space;=&space;\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon&space;}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})&space;\pm&space;\frac{A_{surf}}{V_{epi}&space;\rho&space;c_p}&space;J" title="\frac{dT_{epi}}{dt} = \frac{Q}{V_{epi}}T_{in} - \frac{Q}{V_{epi}}T_{epi} + {\upsilon }_t \frac{A_t}{V_{epi}}(T_{hypo} - T_{epi}) \pm \frac{A_{surf}}{V_{epi} \rho c_p} J" /></a>

In this first ordinary differential equation Q stands for a flow discharge rate, V for different Volumes, <a href="https://www.codecogs.com/eqnedit.php?latex={\upsilon}_t" target="_blank"><img src="https://latex.codecogs.com/svg.latex?{\upsilon}_t" title="{\upsilon}_t" /></a> for turbulent diffusivity, A for area (index surf for surface area, t for the thermocline area), J for atmospheric heat fluxes, <a href="https://www.codecogs.com/eqnedit.php?latex=\rho" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\rho" title="\rho" /></a> for the density of water, and <a href="https://www.codecogs.com/eqnedit.php?latex=c_p" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_p" title="c_p" /></a> is the specific heat capacity of water.

Thankfully, the equation for the hypolimnion heat balance is simpler as we are neglecting submerged inflows or sediment heat sources:

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" title="\frac{dT_{hypo}}{dt} = {\upsilon}_t \frac{A_t}{V_{hypo}} (T_{epi} - T_{hypo})" /></a>

Now we need some boundary conditions to 'drive' the model. For this we will use measured daily meteorological, which will act as heat flux sources from the top (also called Neumann boundary condition type in the literature). We will separate the atmospheric heat fluxes J into <a href="https://www.codecogs.com/eqnedit.php?latex=J&space;=&space;J_{sw}&space;&plus;&space;J_{in}&space;*&space;J_{out}&space;&plus;&space;J_{E}&space;&plus;&space;J_{H}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J&space;=&space;J_{sw}&space;&plus;&space;J_{in}&space;*&space;J_{out}&space;&plus;&space;J_{E}&space;&plus;&space;J_{H}" title="J = J_{sw} + J_{in} * J_{out} + J_{E} + J_{H}" /></a> (meaning that J is the sum of shortwave radiation, incoming longwave radiation, outgoing longwave radiation, latent heat flux (evaporation), and sensible heat flux (convection and conduction).

We'll set up each heat flux term now. The easiest one is the solar shortwave radiation, which is 'just' the measured shortwave radiation <a href="https://www.codecogs.com/eqnedit.php?latex=J_{sw}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_{sw}" title="J_{sw}" /></a>. As the shortwave flux has the unit <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{cal}{{cm}^2&space;d}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{cal}{{cm}^2&space;d}" title="\frac{cal}{{cm}^2 d}" /></a>, we will get a total heat flux (by multiplying with <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{A_{surf}}{V_{epi}\rho&space;c_p}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{A_{surf}}{V_{epi}\rho&space;c_p}" title="\frac{A_{surf}}{V_{epi}\rho c_p}" /></a>) of <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{C}{d}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{C}{d}" title="\frac{C}{d}" /></a> (this is true for all the other heat fluxes, but I admit that this unit is somehow weird but useful for our simple model). 



More coming soon...
