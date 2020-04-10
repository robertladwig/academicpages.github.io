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
## Introduction
Welcome, in this paragraph I want to briefly describe and show how you can write and run a lake model in R. When I began my PhD studies, I had a hard time wrapping my head around all the equations and coding techniques. Therefore, I want to provide a walkthrough here. Please write me a mail if you find errors or typos. Hope you enjoy it.

A great type of lake models just assumes that the lake is divided into two volume layers: the epilimnion (surface mixed layer) and the hypolimnion (bottom stagnant layer). Both layers are divided by a metalimnion (layer with a steep density gradient). This model assumption generally holds up for lakes that stratify during summer. The entrainment over the metalimnion (or thermocline) depends on a vertical diffusion coefficient, which is further simplified as a function of the diffusion at neutral stability to the Richardson number. This last assumption helps the model in dynamically changing from mixed to stratified condtions.

All equations and derivations are from Steven C. Chapra (2008) 'Surface Water-Quality Modeling' Waveland Press, Inc.

## Main equations
First, we can write the heat balance of the epilimnion as the change of epilimnion water temperaturen over time in dependence of an inflow source, an outflow sink, turbulent diffusion over the metalimnion between the hypolimnion and epilimnion, as well as heat fluxes from the atmosphere (more to this term later):

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{epi}}{dt}&space;=&space;\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon&space;}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})&space;\pm&space;\frac{A_{surf}}{V_{epi}&space;\rho&space;c_p}&space;J" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{epi}}{dt}&space;=&space;\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon&space;}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})&space;\pm&space;\frac{A_{surf}}{V_{epi}&space;\rho&space;c_p}&space;J" title="\frac{dT_{epi}}{dt} = \frac{Q}{V_{epi}}T_{in} - \frac{Q}{V_{epi}}T_{epi} + {\upsilon }_t \frac{A_t}{V_{epi}}(T_{hypo} - T_{epi}) \pm \frac{A_{surf}}{V_{epi} \rho c_p} J" /></a>

In this first ordinary differential equation Q stands for a flow discharge rate, V for different Volumes, <a href="https://www.codecogs.com/eqnedit.php?latex={\upsilon}_t" target="_blank"><img src="https://latex.codecogs.com/svg.latex?{\upsilon}_t" title="{\upsilon}_t" /></a> for turbulent diffusivity, A for area (index surf for surface area, t for the thermocline area), J for atmospheric heat fluxes, <a href="https://www.codecogs.com/eqnedit.php?latex=\rho" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\rho" title="\rho" /></a> for the density of water, and <a href="https://www.codecogs.com/eqnedit.php?latex=c_p" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_p" title="c_p" /></a> is the specific heat capacity of water.

Thankfully, the equation for the hypolimnion heat balance is simpler as we are neglecting submerged inflows or sediment heat sources:

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" title="\frac{dT_{hypo}}{dt} = {\upsilon}_t \frac{A_t}{V_{hypo}} (T_{epi} - T_{hypo})" /></a>

How can we get the vertical diffusivity? The goal is to get a dynamic respresentation of the vertical diffusivity in dependence of the water column stability (in contrast to telling the model to use high or low values on some days). We'll state the diffusion coefficient as depending on the diffusion at neutral stability to the Richardson number: 

<a href="https://www.codecogs.com/eqnedit.php?latex=E&space;=&space;\frac{E_0}{(1&plus;\alpha&space;{R_i})^{3/2}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?E&space;=&space;\frac{E_0}{(1&plus;\alpha&space;{Ri})^{3/2}}" title="E = \frac{E_0}{(1+\alpha {Ri})^{3/2}}" /></a>

We can rewrite the diffusion coefficient at neutral stability to depend on the ratio of wind shear stress to water density:
<a href="https://www.codecogs.com/eqnedit.php?latex=E_0&space;=&space;c&space;\sqrt{\frac{\tau}{\rho_{water}}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?E_0&space;=&space;c&space;\sqrt{\frac{\tau}{\rho_{water}}}" title="E_0 = c \sqrt{\frac{\tau}{\rho_{water}}}" /></a>

Here, the wind shear stress depends on the density of the air, a drag coefficient and the measured wind speed:
<a href="https://www.codecogs.com/eqnedit.php?latex={\tau}&space;=&space;{\rho}_{air}&space;C_d&space;{u_{wind}}^2" target="_blank"><img src="https://latex.codecogs.com/svg.latex?{\tau}}&space;=&space;{\rho}_{air}&space;C_d&space;{u_{wind}}^2" title="{\tau} = {\rho}_{air} C_d {u_{wind}}^2" /></a>


Next is the Richardson number which generally describes the work of buoyancy against wind-induced turbulence. If it is below 1/4 the system will experience shear-induced turbulence. It can be estimated by:

<a href="https://www.codecogs.com/eqnedit.php?latex=Ri&space;=&space;\frac{-\frac{g}{\rho}\frac{d\rho}{dz}}{\sqrt{\frac{\tau}{\rho_{water}}}H^{-2}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?Ri&space;=&space;\frac{-\frac{g}{\rho}\frac{d\rho}{dz}}{\sqrt{\frac{\tau}{\rho_{water}}}H^{-2}}" title="Ri = \frac{-\frac{g}{\rho}\frac{d\rho}{dz}}{\sqrt{\frac{\tau}{\rho_{water}}}H^{-2}}" /></a>



where H is the thickness over which the wind can act on the lake (assumed here to be the thermocline depth). Please note that <a href="https://www.codecogs.com/eqnedit.php?latex=\alpha" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\alpha" title="\alpha" /></a> and c are constants.

## Boundary and initial conditions
Now we need some boundary conditions to 'drive' the model. For this we will use measured daily meteorological, which will act as heat flux sources from the top (also called Neumann boundary condition type in the literature). We will separate the atmospheric heat fluxes J into <a href="https://www.codecogs.com/eqnedit.php?latex=J&space;=&space;J_{sw}&space;&plus;&space;J_{in}&space;*&space;J_{out}&space;&plus;&space;J_{E}&space;&plus;&space;J_{H}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J&space;=&space;J_{sw}&space;&plus;&space;J_{in}&space;*&space;J_{out}&space;&plus;&space;J_{E}&space;&plus;&space;J_{H}" title="J = J_{sw} + J_{in} * J_{out} + J_{E} + J_{H}" /></a> (meaning that J is the sum of shortwave radiation, incoming longwave radiation, outgoing longwave radiation, latent heat flux (evaporation), and sensible heat flux (convection and conduction).

We'll set up each heat flux term now. The easiest one is the solar shortwave radiation, which is 'just' the measured shortwave radiation <a href="https://www.codecogs.com/eqnedit.php?latex=J_{sw}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_{sw}" title="J_{sw}" /></a>. As the shortwave flux has the unit <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{cal}{cm^{2}&space;d}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{cal}{cm^{2}&space;d}" title="\frac{cal}{cm^{2} d}" /></a>, we will get a total heat flux (by multiplying with <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{A_{surf}}{V_{epi}\rho&space;c_p}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{A_{surf}}{V_{epi}\rho&space;c_p}" title="\frac{A_{surf}}{V_{epi}\rho c_p}" /></a>) of <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{C}{d}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{C}{d}" title="\frac{C}{d}" /></a> (this is true for all the other heat fluxes, but I admit that this unit is somehow weird but useful for our simple model). The incoming longwave radiation can be described by <a href="https://www.codecogs.com/eqnedit.php?latex=J_{in}&space;=&space;\sigma&space;(T_{air}&space;&plus;&space;273)^4&space;(K&plus;0.031&space;\sqrt{e_{air}})(1-R_L)" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_{in}&space;=&space;\sigma&space;(T_{air}&space;&plus;&space;273)^4&space;(K&plus;0.031&space;\sqrt{e_{air}})(1-R_L)" title="J_{in} = \sigma (T_{air} + 273)^4 (K+0.031 \sqrt{e_{air}})(1-R_L)" /></a> (essentially depending on Stefan-Boltzmann constant <a href="https://www.codecogs.com/eqnedit.php?latex=\sigma" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\sigma" title="\sigma" /></a>, air temperature, the air vapor pressure and the reflection. The outgoing longwave radiation from the lake is <a href="https://www.codecogs.com/eqnedit.php?latex=\epsilon&space;\sigma&space;(T_{epi}&space;&plus;&space;273)^4" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\epsilon&space;\sigma&space;(T_{epi}&space;&plus;&space;273)^4" title="\epsilon \sigma (T_{epi} + 273)^4" /></a> with <a href="https://www.codecogs.com/eqnedit.php?latex=\epsilon" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\epsilon" title="\epsilon" /></a> being the emissivity of water. The latent heat flux can be calculated with <a href="https://www.codecogs.com/eqnedit.php?latex=f(Uw)(e_{sat}&space;-&space;e_{air})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?f(Uw)(e_{sat}&space;-&space;e_{air})" title="f(Uw)(e_{sat} - e_{air})" /></a> where <a href="https://www.codecogs.com/eqnedit.php?latex=f(Uw)=&space;19.0&space;&plus;&space;0.95&space;{u_{wind}}^2" target="_blank"><img src="https://latex.codecogs.com/svg.latex?f(Uw)=&space;19.0&space;&plus;&space;0.95&space;{u_{wind}}^2" title="f(Uw)= 19.0 + 0.95 {u_{wind}}^2" /></a> (function of wind speed) multiplied by the difference between surface water vapor pressure and air vapor pressure. Finally, the sensible heat flux is <a href="https://www.codecogs.com/eqnedit.php?latex=c_1&space;f(Uw)&space;(T_{epi}-T_{air})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_1&space;f(Uw)&space;(T_{epi}-T_{air})" title="c_1 f(Uw) (T_{epi}-T_{air})" /></a>, here <a href="https://www.codecogs.com/eqnedit.php?latex=c_1" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_1" title="c_1" /></a> is the Bowen coefficient. 

We will assume that the inflow equals the outflow and is contant. Also the inflow water temperature will be constant.
For initial conditions and the sake of simplicity, let's put the initial water temperature of both layers to 3 °C.

## How to solve the model
To approximate the solutions of our ordinary differential equations over time, we will use an iterative method called the fourth-order Runge-Kutta method, which has the form:
<a href="https://www.codecogs.com/eqnedit.php?latex=c_{i&plus;1}=&space;c_i&space;&plus;&space;[\frac{1}{6}(k_1&plus;2k_2&plus;k_3&plus;k_4)]h" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_{i&plus;1}=&space;c_i&space;&plus;&space;[\frac{1}{6}(k_1&plus;2k_2&plus;k_3&plus;k_4)]h" title="c_{i+1}= c_i + [\frac{1}{6}(k_1+2k_2+k_3+k_4)]h" /></a>
for which we have to solve the coefficients k in dependence of a step-size h>0. Luckily, these numerical methods are included in lots of R packages, for instance [deSolve](https://cran.r-project.org/web/packages/deSolve/index.html).

# Applying the model
We will try the formulated model on [Lough Feeagh](https://gleon.org/lakes/lough-feeagh) in Ireland, which has a maximum depth of 46.8 m. Here you can see the lake (credits to Tadhg Moore):

<br/><img src='/images/MRA_0610.JPG'>

To run the model, first we need [meteorological data](https://www.met.ie/climate/available-data/historical-data#), which consists of a time step, the shortwave radiation, the air temperature, the dew point temperature, the wind speed, and the calculated f(Uw) as a function of the wind speed. You can download the data here .

We also need additional data about the lake which all should be in cm, for instance the volume of the epilimnion (approx. 2.8 10^13 cm3), the volume of the hypolimnion (approx. 3.4 10^13 cm3), the surface area (3.9 10^10 cm2), the thermocline area (2.6 10^10 cm2), and the thickness of the metalimnion (let us assume it to be about 300 cm). We will neglect any inflows and outflows for this example (this simplifies our model even more, awesome!). Next we have to estimate the depth of the thermocline. For this we will use a regression from Hanna M. (1990): Evaluation of Models predicting Mixing Depth. Can. J. Fish. Aquat. Sci. 47: 940-947:
<a href="https://www.codecogs.com/eqnedit.php?latex=z_{therm}&space;=&space;10^{0.336&space;log10(max(L,W))-0.245}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?z_{therm}&space;=&space;10^{0.336&space;log10(max(L,W))-0.245}" title="z_{therm} = 10^{0.336 log10(max(L,W))-0.245}" /></a>
This means that the thermocline depth depends on the maximum distance over our lake (either length or width).



More coming soon...
