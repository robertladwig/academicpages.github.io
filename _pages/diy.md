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
updated: 04-10-2020
## Introduction
Welcome! In this tutorial, I want to describe and show you how you can write and run a lake model in R. When I began my PhD studies, I had a hard time wrapping my head around all the equations and coding techniques. Therefore, I want to provide a walkthrough here. Please send me an email if you find errors or typos (especially if you find errors!). Hope you enjoy it.

A great type of lake model just assumes that the lake is divided into two volume layers: the epilimnion (surface mixed layer) and the hypolimnion (bottom stagnant layer). Both layers are divided by a metalimnion (layer with a steep density gradient). This model assumption generally holds up for lakes that stratify during the summer. The entrainment over the metalimnion (or thermocline) depends on a vertical diffusion coefficient, which is further simplified as a function of the diffusion at neutral stability to the Richardson number. This last assumption helps the model dynamically change from mixed to stratified conditions.

All equations and derivations are from Steven C. Chapra (2008) 'Surface Water-Quality Modeling' Waveland Press, Inc.

## Main equations
First, we can write the heat balance of the epilimnion as the change of epilimnion water temperature over time in dependence with an inflow source, an outflow sink, turbulent diffusion over the metalimnion between the hypolimnion and epilimnion, as well as heat fluxes from the atmosphere (more to this term later):

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{epi}}{dt}&space;=&space;\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon&space;}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})&space;\pm&space;\frac{A_{surf}}{V_{epi}&space;\rho&space;c_p}&space;J" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{epi}}{dt}&space;=&space;\frac{Q}{V_{epi}}T_{in}&space;-&space;\frac{Q}{V_{epi}}T_{epi}&space;&plus;&space;{\upsilon&space;}_t&space;\frac{A_t}{V_{epi}}(T_{hypo}&space;-&space;T_{epi})&space;\pm&space;\frac{A_{surf}}{V_{epi}&space;\rho&space;c_p}&space;J" title="\frac{dT_{epi}}{dt} = \frac{Q}{V_{epi}}T_{in} - \frac{Q}{V_{epi}}T_{epi} + {\upsilon }_t \frac{A_t}{V_{epi}}(T_{hypo} - T_{epi}) \pm \frac{A_{surf}}{V_{epi} \rho c_p} J" /></a>

In this first ordinary differential equation Q stands for a flow discharge rate, V for different Volumes, <a href="https://www.codecogs.com/eqnedit.php?latex={\upsilon}_t" target="_blank"><img src="https://latex.codecogs.com/svg.latex?{\upsilon}_t" title="{\upsilon}_t" /></a> for turbulent diffusivity, A for area (index surf for surface area, t for the thermocline area), J for atmospheric heat fluxes, <a href="https://www.codecogs.com/eqnedit.php?latex=\rho" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\rho" title="\rho" /></a> for the density of water, and <a href="https://www.codecogs.com/eqnedit.php?latex=c_p" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_p" title="c_p" /></a> is the specific heat capacity of water.

Thankfully, the equation for the hypolimnion heat balance is simpler as we are neglecting submerged inflows or sediment heat sources:

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{dT_{hypo}}{dt}&space;=&space;{\upsilon}_t&space;\frac{A_t}{V_{hypo}}&space;(T_{epi}&space;-&space;T_{hypo})" title="\frac{dT_{hypo}}{dt} = {\upsilon}_t \frac{A_t}{V_{hypo}} (T_{epi} - T_{hypo})" /></a>

How can we get the vertical diffusivity? The goal is to get a dynamic representation of the vertical diffusivity in dependence of the water column stability (in contrast to telling the model to use high or low values on some days). We'll state the diffusion coefficient as depending on the diffusion at neutral stability to the Richardson number: 

<a href="https://www.codecogs.com/eqnedit.php?latex=E&space;=&space;\frac{E_0}{(1&plus;\alpha&space;{R_i})^{3/2}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?E&space;=&space;\frac{E_0}{(1&plus;\alpha&space;{Ri})^{3/2}}" title="E = \frac{E_0}{(1+\alpha {Ri})^{3/2}}" /></a>

We can rewrite the diffusion coefficient at neutral stability to depend on the ratio of wind shear stress to water density:
<a href="https://www.codecogs.com/eqnedit.php?latex=E_0&space;=&space;c&space;\sqrt{\frac{\tau}{\rho_{water}}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?E_0&space;=&space;c&space;\sqrt{\frac{\tau}{\rho_{water}}}" title="E_0 = c \sqrt{\frac{\tau}{\rho_{water}}}" /></a>

Here, the wind shear stress depends on the density of the air, a drag coefficient and the measured wind speed:
<a href="https://www.codecogs.com/eqnedit.php?latex={\tau}&space;=&space;{\rho}_{air}&space;C_d&space;{u_{wind}}^2" target="_blank"><img src="https://latex.codecogs.com/svg.latex?{\tau}}&space;=&space;{\rho}_{air}&space;C_d&space;{u_{wind}}^2" title="{\tau} = {\rho}_{air} C_d {u_{wind}}^2" /></a>


Next is the Richardson number, which generally describes the work of buoyancy against wind-induced turbulence. If it is below 1/4, the system will experience shear-induced turbulence. It can be estimated by:

<a href="https://www.codecogs.com/eqnedit.php?latex=Ri&space;=&space;\frac{-\frac{g}{\rho}\frac{d\rho}{dz}}{\sqrt{\frac{\tau}{\rho_{water}}}H^{-2}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?Ri&space;=&space;\frac{-\frac{g}{\rho}\frac{d\rho}{dz}}{\sqrt{\frac{\tau}{\rho_{water}}}H^{-2}}" title="Ri = \frac{-\frac{g}{\rho}\frac{d\rho}{dz}}{\sqrt{\frac{\tau}{\rho_{water}}}H^{-2}}" /></a>



where H is the thickness over which the wind can act on the lake (assumed here to be the thermocline depth). Please note that <a href="https://www.codecogs.com/eqnedit.php?latex=\alpha" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\alpha" title="\alpha" /></a> and c are constants.

## Boundary and initial conditions
Now we need some boundary conditions to 'drive' the model. For this we will use measured daily meteorological data, which will act as heat flux sources from the top (also called Neumann boundary condition type in the literature). We will separate the atmospheric heat fluxes J into <a href="https://www.codecogs.com/eqnedit.php?latex=J&space;=&space;J_{sw}&space;&plus;&space;J_{in}&space;*&space;J_{out}&space;&plus;&space;J_{E}&space;&plus;&space;J_{H}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J&space;=&space;J_{sw}&space;&plus;&space;J_{in}&space;*&space;J_{out}&space;&plus;&space;J_{E}&space;&plus;&space;J_{H}" title="J = J_{sw} + J_{in} * J_{out} + J_{E} + J_{H}" /></a> (meaning that J is the sum of shortwave radiation, incoming longwave radiation, outgoing longwave radiation, latent heat flux (evaporation), and sensible heat flux (convection and conduction).

We'll set up each heat flux term now. The easiest one is the solar shortwave radiation, which is 'just' the measured shortwave radiation <a href="https://www.codecogs.com/eqnedit.php?latex=J_{sw}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_{sw}" title="J_{sw}" /></a>. As the shortwave flux has the unit <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{cal}{cm^{2}&space;d}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{cal}{cm^{2}&space;d}" title="\frac{cal}{cm^{2} d}" /></a>, we will get a total heat flux (by multiplying with <a href="https://www.codecogs.com/eqnedit.php?latex=\frac{A_{surf}}{V_{epi}\rho&space;c_p}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\frac{A_{surf}}{V_{epi}\rho&space;c_p}" title="\frac{A_{surf}}{V_{epi}\rho c_p}" /></a>) of °C/day (this is true for all the other heat fluxes, but I admit that this unit is somehow weird but useful for our simple model). The incoming longwave radiation can be described by <a href="https://www.codecogs.com/eqnedit.php?latex=J_{in}&space;=&space;\sigma&space;(T_{air}&space;&plus;&space;273)^4&space;(K&plus;0.031&space;\sqrt{e_{air}})(1-R_L)" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_{in}&space;=&space;\sigma&space;(T_{air}&space;&plus;&space;273)^4&space;(K&plus;0.031&space;\sqrt{e_{air}})(1-R_L)" title="J_{in} = \sigma (T_{air} + 273)^4 (K+0.031 \sqrt{e_{air}})(1-R_L)" /></a> (essentially depending on Stefan-Boltzmann constant <a href="https://www.codecogs.com/eqnedit.php?latex=\sigma" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\sigma" title="\sigma" /></a>, air temperature, the air vapor pressure and the reflection). The outgoing longwave radiation from the lake is <a href="https://www.codecogs.com/eqnedit.php?latex=J_{out}=\epsilon&space;\sigma&space;(T_{epi}&space;&plus;273)^4" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_{out}=\epsilon&space;\sigma&space;(T_{epi}&space;&plus;273)^4" title="J_{out}=\epsilon \sigma (T_{epi} +273)^4" /></a> with <a href="https://www.codecogs.com/eqnedit.php?latex=\epsilon" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\epsilon" title="\epsilon" /></a> being the emissivity of water. The latent heat flux can be calculated with <a href="https://www.codecogs.com/eqnedit.php?latex=J_E&space;=&space;f(Uw)(e_{sat}-e_{air})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_E&space;=&space;f(Uw)(e_{sat}-e_{air})" title="J_E = f(Uw)(e_{sat}-e_{air})" /></a> where <a href="https://www.codecogs.com/eqnedit.php?latex=f(Uw)=&space;19.0&space;&plus;&space;0.95&space;{u_{wind}}^2" target="_blank"><img src="https://latex.codecogs.com/svg.latex?f(Uw)=&space;19.0&space;&plus;&space;0.95&space;{u_{wind}}^2" title="f(Uw)= 19.0 + 0.95 {u_{wind}}^2" /></a> (function of wind speed) multiplied by the difference between surface water vapor pressure and air vapor pressure. Finally, the sensible heat flux is <a href="https://www.codecogs.com/eqnedit.php?latex=J_H&space;=&space;c_1&space;f(Uw)(T_{epi}&space;-&space;T_{air})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?J_H&space;=&space;c_1&space;f(Uw)(T_{epi}&space;-&space;T_{air})" title="J_H = c_1 f(Uw)(T_{epi} - T_{air})" /></a>, here <a href="https://www.codecogs.com/eqnedit.php?latex=c_1" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_1" title="c_1" /></a> is the Bowen coefficient.

We will assume that the inflow equals the outflow and is constant. Also, the inflow water temperature will be constant.
For initial conditions and the sake of simplicity, let's put the initial water temperature of both layers to 3 °C.

Our model is now looking like this:

<br/><img src='/images/2dmodel-schematic-01.png'>

## How to solve the model
To approximate the solutions of our ordinary differential equations over time, we will use an iterative method called the fourth-order Runge-Kutta method, which has the form:
<a href="https://www.codecogs.com/eqnedit.php?latex=c_{i&plus;1}=&space;c_i&space;&plus;&space;[\frac{1}{6}(k_1&plus;2k_2&plus;k_3&plus;k_4)]h" target="_blank"><img src="https://latex.codecogs.com/svg.latex?c_{i&plus;1}=&space;c_i&space;&plus;&space;[\frac{1}{6}(k_1&plus;2k_2&plus;k_3&plus;k_4)]h" title="c_{i+1}= c_i + [\frac{1}{6}(k_1+2k_2+k_3+k_4)]h" /></a>
for which we have to solve the coefficients k in dependence of a step-size h>0. Luckily, these numerical methods are included in lots of R packages, for instance [deSolve](https://cran.r-project.org/web/packages/deSolve/index.html).

# Applying the model
We will try the formulated model on [Lough Feeagh](https://gleon.org/lakes/lough-feeagh) in Ireland, which has a maximum depth of 46.8 m. Looks beautiful, right (credits to Mikkel René Andersen):

<br/><img src='/images/MRA_0610.JPG'>

To run the model, first we need [meteorological data](https://www.met.ie/climate/available-data/historical-data#), which consists of a time step, the shortwave radiation, the air temperature, the dew point temperature, and the wind speed. You can download the data [here](https://github.com/robertladwig/robertladwig.github.io/blob/master/files/meteo.txt).

We also need additional data about the lake which all should be in cm, for instance the volume of the epilimnion (approx. 2.8 10^13 cm3), the volume of the hypolimnion (approx. 3.4 10^13 cm3), the surface area (3.9 10^10 cm2), the thermocline area (2.6 10^10 cm2), and the thickness of the metalimnion (let us assume it to be about 300 cm). We will neglect any inflows and outflows for this example (this simplifies our model even more, awesome!). Next we have to estimate the depth of the thermocline. For this we will use a regression from Hanna M. (1990): Evaluation of Models predicting Mixing Depth. Can. J. Fish. Aquat. Sci. 47: 940-947:
<a href="https://www.codecogs.com/eqnedit.php?latex=z_{therm}&space;=&space;10^{0.336&space;log10(max(L,W))-0.245}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?z_{therm}&space;=&space;10^{0.336&space;log10(max(L,W))-0.245}" title="z_{therm} = 10^{0.336 log10(max(L,W))-0.245}" /></a>

This means that the thermocline depth depends on the maximum distance over our lake (either length or width). We need this depth to estimate our volumes (epilimnion and hypolimnion) and the respective areas (thermocline area) from bathymetry data if we don't know it a priori.

Now it's the perfect time to load our libraries:
```
library(gridExtra)
library(ggplot2)
library(pracma)
library(tidyverse)
library(RColorBrewer)
library(zoo)
library(deSolve)
```
Let us first state the important parameters, load the data and give the model some initial conditions:
```
# load in the boundary data
bound <-read_delim(paste0('/home/robert/Projects/DSI/thermod/feeagh/meteo.txt'), delim = '\t')

# approximate the thermocline depth
therm_depth <- 10 ^ (0.336 * log10(3678) - 0.245) 
simple_therm_depth <- round(therm_depth)

Ve <- 2.886548e+13 # epilimnion volume (cm3)
Vh <- 3.421416e+13 # hypolimnion volume (cm3)
Ht <- 3 * 100 # thermocline thickness (cm)
At <- 26850835487 # thermocline area (cm2)
As <- 3.931e+10 # surface area (cm2)
Tin <- 0 # inflow water temperature
Q <- 0 # inflow discharge 
Rl <- 0.03 # reflection coefficient (generally small, 0.03)
Acoeff <- 0.6 # coefficient between 0.5 - 0.7
sigma <- 11.7 * 10^(-8) # cal / (cm2 d K4) or: 4.9 * 10^(-3) # Stefan-Boltzmann constant in (J (m2 d K4)-1)
eps <- 0.97 # emissivity of water
rho <- 0.9982 # density (g per cm3)
cp <- 0.99 # specific heat (cal per gram per deg C)
c1 <- 0.47 # Bowen's coefficient
a <- 7 # constant
c <- 9e4 # empirical constant
g <- 9.81  # gravity (m/s2)
thermDep <- simple_therm_depth

parameters <- c(Ve, Vh, At, Ht, As, Tin, Q, Rl, Acoeff, sigma, eps, rho, cp, c1, a, c, g, thermDep)

colnames(bound) <- c('Day','Jsw','Tair','Dew','vW')

# function to calculate wind shear stress (and transforming wind speed from km/h to m/s)
bound$Uw <- 19.0 + 0.95 * (bound$vW * 1000/3600)^2 
bound$vW <- bound$vW * 1000/3600

boundary <- bound

# simulation maximum length
times <- seq(from = 1, to = max(boundary$Day), by = 1)
# initial water temperatures
yini <- c(3,3) 
```
Great, now we write down the model itself as a function (which you can also find [here](https://github.com/robertladwig/thermod):
```
run_model <- function(bc, params, ini, times){
  
  Ve <- params[1]
  Vh <- params[2]
  At <- params[3]
  Ht <- params[4]
  As <- params[5]
  Tin <- params[6]
  Q <- params[7]
  Rl <- params[8]
  Acoeff <- params[9]
  sigma <- params[10]
  eps <- params[11]
  rho <- params[12]
  cp <- params[13]
  c1 <- params[14]
  a <- params[15]
  c <- params[16]
  g <- params[17]
  thermDep <- params[18]
  
  TwoLayer <- function(t, y, parms){
    eair <- (4.596 * exp((17.27 * Dew(t)) / (237.3 + Dew(t)))) # air vapor pressure
    esat <- 4.596 * exp((17.27 * Tair(t)) / (237.3 + Tair(t))) # saturation vapor pressure
    RH <- eair/esat *100 # relative humidity
    es <- 4.596 * exp((17.27 * y[1])/ (273.3+y[1]))
    # diffusion coefficient
    Cd <- 0.00052 * (vW(t))^(0.44)
    shear <- 1.164/1000 * Cd * (vW(t))^2
    rho_e <- calc_dens(y[1])/1000
    rho_h <- calc_dens(y[2])/1000
    w0 <- sqrt(shear/rho_e) 
    E0  <- c * w0
    Ri <- ((g/rho)*(abs(rho_e-rho_h)/10))/(w0/(thermDep)^2)
    if (rho_e > rho_h){
      dV = 100
    } else {
      dV <- (E0 / (1 + a * Ri)^(3/2))/(Ht/100) * (86400/10000)
    }
    
    # epilimnion water temperature change per time unit
    dTe <-  Q / Ve * Tin -              # inflow heat
      Q / Ve * y[1] +                   # outflow heat
      ((dV * At) / Ve) * (y[2] - y[1]) + # mixing between epilimnion and hypolimnion
      + As/(Ve * rho * cp) * (
        Jsw(t)  + # shortwave radiation
          (sigma * (Tair(t) + 273)^4 * (Acoeff + 0.031 * sqrt(eair)) * (1 - Rl)) - # longwave radiation into the lake
          (eps * sigma * (y[1] + 273)^4)  - # backscattering longwave radiation from the lake
          (c1 * Uw(t) * (y[1] - Tair(t))) - # convection
          (Uw(t) * ((es) - (eair))) )# evaporation
    
    # hypolimnion water temperature change per time unit
    dTh <-  ((dV * At) / Vh) * (y[1] - y[2]) 

    # diagnostic variables for plotting
    mix_e <- ((dV * At) / Ve) * (y[2] - y[1])
    mix_h <-  ((dV * At) / Vh) * (y[1] - y[2]) 
    qin <- Q / Ve * Tin 
    qout <- - Q / Ve * y[1] 
    sw <- Jsw(t) 
    lw <- (sigma * (Tair(t) + 273)^4 * (Acoeff + 0.031 * sqrt(eair)) * (1 - Rl))
    water_lw <- - (eps * sigma * (y[1]+ 273)^4)
    conv <- - (c1 * Uw(t) * (y[1] - Tair(t))) 
    evap <- - (Uw(t) * ((esat) - (eair)))
    Rh <- RH
    E <- (E0 / (1 + a * Ri)^(3/2))
    
    write.table(matrix(c(qin, qout, mix_e, mix_h, sw, lw, water_lw, conv, evap, Rh,E, Ri, t), nrow=1), 'output.txt', append = TRUE,
                quote = FALSE, row.names = FALSE, col.names = FALSE)

    return(list(c(dTe, dTh)))
  }
  
  # approximating all boundary conditions  (linear interpolation)
  Jsw <- approxfun(x = bc$Day, y = bc$Jsw, method = "linear", rule = 2)
  Tair <- approxfun(x = bc$Day, y = bc$Tair, method = "linear", rule = 2)
  Dew <- approxfun(x = bc$Day, y = bc$Dew, method = "linear", rule = 2)
  Uw <- approxfun(x = bc$Day, y = bc$Uw, method = "linear", rule = 2)
  vW <- approxfun(x = bc$Day, y = bc$vW, method = "linear", rule = 2)
  
  # runge-kutta 4th order
  out <- ode(times = times, y = ini, func = TwoLayer, parms = params, method = 'rk4')
  
  return(out)
}
```
As you can see in the code, the function reads in your boundary conditions, the parameters, the initial conditions and the run time. Then for simplicity it assigns these values to variables. The model itself is written as a separate function INSIDE the function which takes the run time, the initial values and the parameters as input. This sub-function then calculates important variables for every time step, e.g. air vapor pressure, air saturation, the diffusion coefficient, shear stress, water densities and the vertical diffusivity (we assume the lake is completely mixed during fall to spring, therefore the value becomes 100). Then you can see the two differential equations. At the end I added some lines to write the fluxes and variables into a separate text file, which we can use to investigate the model performance. Before the model is run (using Runge-Kutta), we need to interpolate the meteorological data (because the time steps can be below a day).

And another small function to calculate water density from temperature:
```
calc_dens <- function(wtemp){
  dens = 999.842594 + (6.793952 * 10^-2 * wtemp) - (9.095290 * 10^-3 *wtemp^2) + (1.001685 * 10^-4 * wtemp^3) - (1.120083 * 10^-6* wtemp^4) + (6.536336 * 10^-9 * wtemp^5)
  return(dens)
}
```
Now it's time ... exciting, right?
```
out <- run_model(bc = boundary, params = parameters, ini = yini, times = times)

```
If you don't get any warning, that's good! Now we can look at the model output which is stored in the variable "out":
```
result <- data.frame('Time' = out[,1],
                     'WT_epi' = out[,2], 'WT_hyp' = out[,3])
g1 <- ggplot(result) +
  geom_line(aes(x=Time, y=WT_epi, col='Surface Mixed Layer')) +
  geom_line(aes(x=(Time), y=WT_hyp, col='Bottom Layer')) +
  labs(x = 'Simulated Time', y = 'WT in deg C')  +
  theme_bw()+
  guides(col=guide_legend(title="Layer")) +
  theme(legend.position="bottom")

output <- read.table('output.txt')

output <- data.frame('qin'=output[,1],'qout'=output[,2],'mix_e'=output[,3],'mix_h'=output[,4],
                     'sw'=output[,5],'lw'=output[,6],'water_lw'=output[,7],'conv'=output[,8],
                     'evap'=output[,9], 'Rh' = output[,10],
                     'entrain' = output[,11], 'Ri' = output[,12],'time' = output[,13])

output$balance <- apply(output[,c(1:9)],1, sum)

g2 <- ggplot(output) +
  geom_line(aes(x = time,y = qin, col = 'Inflow')) +
  geom_line(aes(x = time,y = qout, col = 'Outflow')) +
  geom_line(aes(x = time,y = mix_e, col = 'Mixing into Epilimnion')) +
  geom_line(aes(x = time,y = mix_h, col = 'Mixing into Hypolimnion')) +
  geom_line(aes(x = time,y = sw, col = 'Shortwave')) +
  geom_line(aes(x = time,y = lw, col = 'Longwave')) +
  geom_line(aes(x = time,y = water_lw, col = 'Reflection')) +
  geom_line(aes(x = time,y = conv, col = 'Conduction')) +
  geom_line(aes(x = time,y = evap, col = 'Evaporation')) +
  geom_line(aes(x = time,y = balance, col = 'Sum'), linetype = "dashed") +
  scale_colour_brewer("Energy terms", palette="Set3") +
  labs(x = 'Simulated Time', y = 'Fluxes in cal/(cm2 d)')  +
  theme_bw()+
  theme(legend.position="bottom")

g3 <- ggplot(output) +
  geom_line(aes(x = time,y = Ri, col = 'Richardson')) +
  geom_line(aes(x = time,y = entrain, col = 'Entrainment')) +
  scale_colour_brewer("Stability terms", palette="Set1") +
  labs(x = 'Simulated Time', y = 'Entrainment in cm2/s and Ri in [-]')  +
  theme_bw()+
  theme(legend.position="bottom")

g4 <- ggplot(boundary) +
  geom_line(aes(x = Day,y = scale(Jsw), col = 'Shortwave')) +
  geom_line(aes(x = Day,y = scale(Tair), col = 'Airtemp')) +
  geom_line(aes(x = Day,y = scale(Dew), col = 'Dew temperature')) +
  geom_line(aes(x = Day,y = scale(vW), col = 'Wind velocity')) +
  geom_line(aes(x = Day,y = scale(Uw), col = 'Wind shear stress')) +
  scale_colour_brewer("Boundary Conditions", palette="Set3") +
  labs(x = 'Simulated Time', y = 'Scaled meteo. boundaries')  +
  theme_bw()+
  theme(legend.position="bottom")

g5 <- grid.arrange(g1, g2, g3, g4, ncol =1);g5
```

<br/><img src='/images/2L_visual_feeagh.png'>

And the results look surprisingly reasonable. The lake warms up during summer, cools down in winter, has high Richardson numbers during summer, and the heat fluxes have nice seasonal dynamics. Not too bad.

Let's compare how well our model compares to reality. In the next plot red symbolizes the surface layer and blue the bottom layer. Also, solid lines are model results and dashed lines are observed data (measured at 0.9 and 42 m, respectively).

<br/><img src='/images/2L_compare.png'>

Okaaay, it's doing some parts well, some parts bad. But keep in mind that the model is not calibrated yet (we fixed several parameters), doesn't include any inflow dynamics and vertical mixing is vastly simplified.

## Final thoughts
I hope this tutorial was helpful in explaining how a two-layer lake model can be coded in R. Of course, the code could be more efficient, faster and better looking, but I wanted to give you a short walkthrough on how to do it once you have all the important equations. Mathematical modeling can sound scary at the beginning, but once your code is working all pain will be forgotten.
