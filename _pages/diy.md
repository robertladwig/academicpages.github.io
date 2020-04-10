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
A great type of lake models just assumes that the lake is divided into two volume layers: the epilimnion (surface mixed layer) and the hypolimnion (bottom stagnant layer). Both layers are divided by a metalimnion (layer with a steep density gradient). This model assumption generally holds up for lakes that stratify during summer. The entrainment over the metalimnion (or thermocline) depends on a vertical diffusion coefficient, which is further simplified as a function of the diffusion at neutral stability to the Richardson number. This last assumption helps the model in dynamically changing from mixed to stratified condtions.

All equations and derivations are from Steven C. Chapra (2008) 'Surface Water-Quality Modeling' Waveland Press, Inc.

First, we can write the heat balance of the epilimnion as the change of epilimnion water temperaturen over time in dependence of an inflow source, an outflow sink, turbulent diffusion over the metalimnion between the hypolimnion and epilimnion, as well as heat fluxes from the atmosphere (more to this term later):
<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{epi}}{dt}=\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;\upsilon&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})\pm&space;J&space;A_{surface}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{epi}}{dt}=\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;\upsilon&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})\pm&space;J&space;A_{surface}" title="\frac{dT_{epi}}{dt}=\frac{Q}{V_{epi}}T_{in} - \frac{Q}{V_{epi}}T_{epi} + \upsilon \frac{A_t}{V_{epi}}(T_{hypo} - T_{epi})\pm J A_{surface}" /></a>

Coming soon.
