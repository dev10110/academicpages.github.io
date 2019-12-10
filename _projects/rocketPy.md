---
title: "rocketPy"
excerpt: "Developing a pythonic implementation of openRocket and CambridgeRocket <br/><img src='/images/projects/rocketPy/rocketPy.png'>"
collection: projects
current: true
---

<img src='/images/projects/rocketPy/rocketPy.png'>

While openRocket is a standard hobbyist rocket design and analysis tool, its built in Java and has limitations when it comes to easily extending the software.

There are other softwares out there (some not free) but I couldn't find a good pythonic implementation that met my specific need - to be able to test the robustness of a custom throttle controller designed by the ICLR team.

As such, I turned to CambridgeRocket, which has been implemented in MATLAB. Unfortunately, (not only due to MATLAB's limitations) it is very poorly written, passing random structs with names as generic as INTAB1 INTAB2, INTAB3 etc around.

It also didn't expose a method to implement and test a live controller.

However, the technical documentation and two papers published on the project are quite well written and make sense. As such, I have decided to implement them myself in python, and test my team's code on a simulator I built and therefore know the inner workings of.

A key aspect to rocketPy will be its ability to solve the stochastic differential equations, which scipy does not have good implementations of. I havent yet decided if I want to learn and implement this myself, use RK4 and allow for random noise within scipy's API, or to use a relatively ugly hack to get around this. I'll play around with these ideas once I'm at that stage :)

The current working GitHub repository is [here](https://github.com/dev10110/rocketPy), and I'm hoping to make it into a full open-source package at some point that others can modify.
