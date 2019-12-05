---
title:  "Simulating the decay of an orbit"
date:   2018-12-05
permalink: /posts/2018/12/Simulating-the-decay-of-an-orbit/
tags:
  - space
---

<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML' async></script>

For my space propulsion class 16.522 at MIT, our project requires us to build a Hybrid Hall Thruster. This is a thruster that is able to operate both using an on-board propellant, or using the (thin) atmosphere that the satellite is flying throught as a fuel.

As part of the mission analysis team, I took on the task of simulating the orbital decay of any satellite, assuming the thruster had either used up all its fuel or was unable to produce any further trust.

Generally we were interested in cases where


| Quantity  | Values |
| ------------- | ------------- |
| Payload mass (m)  | 4  kg  |
| Frontal Area (A)  | 0.02  m<sup>2</sup>  |
| Drag coefficient (Cd) | 2.2 |
| Initial Altitude (km) | 200 - 500 km|


We want to find the time it takes for the orbit to decay to some arbitrary altitude, say 100 km.

# The Formulae

To simulate the orbiter, we can write a force balance around the orbiter in polar coordinates. If we do this, we arrive at two equations:

 {% raw %}
  $$\frac{d^2 r}{dt^2} - r \left(\frac{d\theta}{dt}\right)^2 + \frac{\mu}{r} = a_r$$
 {% endraw %}
{% raw %}
  $$\frac{d^2 \theta}{dt^2} +  \frac{2}{r} \frac{dr}{dt} \frac{d\theta}{dt} = \frac{a_\theta}{r}$$
 {% endraw %}

 where $$r$$ is the radius of the orbit, $$\theta$$ is the angle from some reference location, $$t$$ is time, $$\mu = GM$$ is the gravity parameter, or the multiplication of the universal gravitational constant and the mass of the main body we are orbiting. The (non-gravitational or centripetal) accelerations experienced by the satellite are specified in the radial direction ($$a_r$$) and the tangential direction ($$a_\theta$$).

 We know that in this case the radial acceleration is 0, and the tangential acceleration is just the drag acceleration. This can be written as
{% raw %}
  $$a_\theta = \frac{F_d}{m} = \frac{\frac{1}{2} \rho V^2 C_d A}{m}$$
 {% endraw %}

where $$\rho$$ is the air density at the altitude of the satellite, $$V = r \frac{d\theta}{dt}$$ (ignoring the rate of change of radius here as that happens on a much slower time scale than the rate of change of angle), $$C_d$$ is the drag coefficient, and $$A$$ is the frontal area of the satellite.



# Solving these equations using Mathematica

Mathematica is awesome. You literally plug in your equation and hit Shift+Enter to solve.

Here is my full code if you want to run it:

{%highlight wl %}

(*Define the radial and tangential equations*)
eqnr = D[r[t], {t, 2}] -
   r[t]* (D[\[Theta][t], t])^2  + \[Mu]/(r[t]^2) == 0;

eqnt = D[\[Theta][t], {t, 2}] + (2/r[t])*D[r[t], t]*
    D[\[Theta][t], t] == -(1/2) rho[
    r[t]] (r[t] D[\[Theta][t], t])^2 cd A/(m*r[t]);


(*Parameters*)
m = 4;
cd = 2;
A = 0.02;
h0 = 450*1000;

(*Orbit Parameters*)

\[Mu] = 6.67*^-11* (5.97*^24);

rho[r_?NumericQ]:=QuantityMagnitude@StandardAtmosphereData[Quantity[\
r-rearth,"Meters"],"Density"] (*I used the built in Mathematica model for the atmosphere. I checked that this is almost exactly the same as the NRLMSISE-00 data that is usually used at high altitudes*)

rearth = 6371*1000;

(*Define inital conditions*)
r0 = rearth + h0;
omega0 = Sqrt[\[Mu]]/(r0^1.5);


(*Termination criteria*)
hstop = 100*1000;

stopcondition =
  WhenEvent[
   r[t] <=  (rearth + hstop), {tsolve = t, "StopIntegration"}];

tsolve = 600000000;


(*PERFORM THE NUMERICAL SOLVE*)
solution =
  NDSolve[{eqnr, eqnt, stopcondition, r[0] == h0 + rearth,
    r'[0] == 0, \[Theta][0] == 0, \[Theta]'[0] ==
     omega0}, {r, \[Theta]}, {t, 0, tsolve}];


(*Print/Plot results*)
Print[{"T to hstop: ", tsolve, "secs"}]

Plot[(r[t] /. solution) - rearth, {t, 0, tsolve}]

Show[Graphics[Circle[{0, 0}, rearth]],
 ParametricPlot[{r[t] *Cos[\[Theta][t]], r[t] *Sin[\[Theta][t]]} /.
   solution, {t, 0, tsolve}, PlotRange -> { {-r0, r0*1.1}, {-r0, r0} }]]

{% endhighlight %}

AND THATS IT! (Can you hear me singing praises for Mathematica?)
