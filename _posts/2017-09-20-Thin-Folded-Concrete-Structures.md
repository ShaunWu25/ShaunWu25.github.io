---
layout: post
title:  "Computational Strategies for Design and Assembly of Thin Folded Concrete Structures"
date:   2017-09-20 02:55:49 +0100
categories: 
image: /assets/1709_folded_concrete/DSC_4397.jpg
tags: concrete, ETH
---

![image](\assets\1709_folded_concrete\DSC_4397.jpg)

This post is about my master thesis studied in Master of Advanced Studies in architecture and digital fabrication, ETH-Zurich in Switzerland. 
It aims to develop an innovative methodology by means of digital fabrication to constructure thin folded concrete structures in a static mold.

[**Smart Dynamic Casting**][SDC](SDC) is a digital fabrication method that consists of a static or a dynamic formwork, which mounted on an industrial robot or a linear axis, as end-effector.

The concrete, poured into the formwork while slipping along a trajectory, is shaped within a time window for slip casting, depending on the behaviour of the material.
This innovative technology has already proved to be a valid alternative to traditional concrete casting technique for non-standard architectural component, with a formwork significantly smaller than the final elements. In addition, SDC is able to generate a set of different geometries within the design space of a single formwork.

The formwork shapes the concrete in a state between soft and hard, plastic enough to be deformed without cracking and buckling, but also sufficiently strong to hold the weight above that is placed in the formwork. Demolding process was integrated following along the robotic trajectory.

![image](\assets\1709_folded_concrete\002.png) | ![image](\assets\1709_folded_concrete\001.png) | ![image](\assets\1709_folded_concrete\003.png)

design > folded > analysis

## Research Goal
The core of our thesis consists in the development of the computational approach to investigate a possible combination of folded precast components from single elements to assembled geometries, which could be fabricated through Smart Dynamic Casting(SDC) technology. This digital tool-box would be able to manipulate simple geometric inputs to explore the design space of SDC, and finally to bridge the gap between design and fabrication, providing the relevant information for the robot. As part of the research, to fully understand the complexity of the SDC process, we attended a series of fabrication experiments, in the attempt to realise a full-scale prototype.

## Developable Surface
In order to identify which kind of geometry SDC, adapted for thin folded structures, can produce, the parameters affecting the shape have to be identified and analyzed. There is a correlation between the shape of the precast element, the geometry of the formwork, the slipping speed and trajectory.
A first generalisation concerns the slip forming technique. The slipping motion of the straight edges at the bottom of the formwork will always describe a 1-degree curvature ruled surface in space. Hereto, the types of possible geometries will be in theory within the whole set of first-degree developable ruled surfaces, cylindrical, tangential and conical developable (Glaeser,2007,61).

![image](\assets\1709_folded_concrete\16.png) | ![image](\assets\1709_folded_concrete\17.png)

## Digital Tools
This advanced digital design tool, which could incorporate the constraints from material and manufacturing, will adapt to the needs of the designer, to give the maximum freedom. Two main features will be included in different modules, one for the design, giving the possibility of multiple design inputs, from curves to surfaces and math functions, and one for the evaluation, returning a real-time visual feedback of the geometry. The design framework will reflect the fabrication settings to develop the right geometries based on the physical possibilities of the SDC current tool, but also there will be options for future tools developments, expanding the design space drastically.

<div class="video"> <figure> <iframe width="640" height="480" src="//www.youtube.com/embed/6jjZQNh1vrw" frameborder="0" allowfullscreen></iframe> </figure> </div>
This short video briefly introduces my master thesis. I developed a series of plug-ins within Rhino/Grasshopper environment which seamlessly bridging the process from the digital design stage to physical fabrication.

## Research Methodology
The produced elements and overall geometries would be determined from the formwork functionality and material constraint. Several specific plug-ins have been developed to explore the design potential of folded geometries within the SDC process. The research method was structured around two main areas: the physical tools and computational approaches. The latter would be the main focus of this thesis, from the understanding of the fabrication as a physical process to the digital design aspects. The outcome will be a digital design toolbox, which allows the user to explore feasible geometries with fast feedback about the material constraints, and finally generating the fabrication data for the ABB robot. It is worth to highlight that all the parameter in the plug-ins could be easily updated with new information from the physical experiments.

![image](\assets\1709_folded_concrete\DSC_5027.jpg)
A curved/folded concrete structure was successfully built in height of 1.5 meter without the need of complex formwork.

## Architectural applications
From the exploration of the single elements, it is now clear that the vocabulary of the feasible folded structures is manifold. Still, numerous physical experiments have to be performed to understand the limits of the achievable curvature for each piece. Additionally, the setup requires further developments along a strategy that addresses the challenges that were discovered and identified in the first experimental phase.

----
**Project Lead**: [Gramazio Kohler Research, ETH-Zurich][GKR]

**Tutors**: [PhD Researcher Anna Szabo](https://gramaziokohler.arch.ethz.ch/web/e/team/227.html)

**Students**: Shaun Dai-Syuan Wu, Federico Giacomarra

Special Thanks: [Ena Lloret-Fritschi][ELF]

[SDC]: https://gramaziokohler.arch.ethz.ch/web/e/forschung/223.html
[GKR]: https://gramaziokohler.arch.ethz.ch/web/e/forschung/index.html
[ELF]: https://gramaziokohler.arch.ethz.ch/web/e/team/106.html