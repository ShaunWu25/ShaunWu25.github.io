---
layout: post
title:  "Buckling Simulation for 3D Printing in Fresh Concrete"
date:   2020-03-09 02:55:49 +0100
categories: 
image: /assets/2003_BucklingSimulation/Cover.JPG
tags: concrete, Witteveen+Bos, Karamba, NTU
---

![image](/assets/2003_BucklingSimulation/200312_width.gif)

3D concrete printing technology has opened up a new era for the construction industry as a new approach to manufacture concrete structures, with the advantage of eliminating the need of customising one-used formwork/mold on non-standard geometries. [Witteveen+Bos][DCG] has been developing and working on 3D concrete printing initiatives all around the world.

One major **challenge** to 3D print with fresh concrete in layer-based extrusion process is to overcome **buckling** and **collapsing** during printing as there is no formwork or mold to support fresh concrete stay in shape. Here, we will focus on how to avoid buckling in 3D printing through [Finite Element Analysis][FEA] using [Karamba 3D][KRB].

There are two modes of potential failure in the 3D concrete printing process; **Elastic** and **Plastic** buckling. **Elastic Buckling** is about the structure bending and failing due to instability; **Plastic Buckling** is about exceeding the material's strength limit. Out of our physical experiments, plastic buckling is less likely to occur in the case of 3D concrete printing process as slim and tall geometry. In short, elastic buckling usually dominates the printing failure.

To give an idea of what kind of **buckling** we are dealing with, please have a look at the illustration below. The goal was to print a straight wall, but it tumbled down as the fresh concrete was _NOT_ stiff enough to support the self-weight deposited on top of each layer.

<img src="{{site.url}}/assets/2003_BucklingSimulation/buckling.gif" style="display: block; margin: auto;" />

There are 2 stages shown in the buckling process.
1. **Onset of buckling**: subtle settlement occurred as buckling.
2. **Collapsing**: structure failing.

What we are going to do is numerically simulating **Onset of buckling** to avoid the subsequent collapsing stage.

This became an interesting topic because traditionally fresh concrete can be checked in [slump test][ST], which allows you to measure the workability of freshly made concrete. In contrast to 3D concrete printing applications, there are no such official tests available to predict the print performance of a mixture. However, alternatively, if we consider the fresh concrete as **soil**, we can make use of existing geotechnical testing methods, which we are not going to discuss here.

The 3D concrete printing research program at [Eindhoven University of Technologies (TU/e)][TUE], has published their pioneering research results for studying the structural behaviour of 3D printed concrete elements. Two papers relevant to the earlier described buckling simulation tools are published by TU/e:
1. [Early age mechanical behaviour of 3D printed concrete: Numerical modeling and experimental testing.][Ear]
2. [Mechanical performance of wall structure in 3D printing process: theory, design tools and experiments.][AkSE] 

To perform and verify the buckling simulation with our software, we used the following data from the above papers from TU/e.

1. Material: Weber Beamix 145-1
2. Printing Speed: 5000 mm/min
3. Print width: 5cm
4. Geometry: cylinder in radius 25cm
5. Layer height: 1cm
6. The following two graphs indicating early initial strength of fresh concrete and strength development;

![image](/assets/2003_BucklingSimulation/stiffness-module.JPG) | ![image](/assets/2003_BucklingSimulation/compressive-yeild-strength.JPG)
*source: [TU/Eindhoven][TUE]* | *source: [TU/Eindhoven][TUE]*

**Initial strength of fresh concrete** and **strength development of fresh concrete** are two important factors to determine the structural behaviour during the 3D printing process. They both have a direct impact on the setting of time-dependent material properties. This means that the strength and stiffness of fresh concrete are increasing during the printing process.

Based on the graphs above, we know the following early age properties of the fresh concrete:
* **Stiffness Module**[MPa] = 0.0781 + 0.0012t
* **Compressive Strength**[kPa] = 5.984 + 0.147t

Based on the printing path length and the printing speed of each layer, we can calculate the actual **age** of each layer as follows:

```python
finalAging= []
for min in mins:
    if len(finalAging) == 0:
        finalAging.append(min)
    else:
        for i in range(len(finalAging)):
            finalAging[i] += min
        finalAging.append(min)
aging = finalAging
```

With the age of each layer, we can acquire the actual **young's module** and **compressive strength** of each layer:

<img src="{{site.url}}/assets/2003_BucklingSimulation/strengthDevelopment.JPG" style="display: block; margin: auto;" />

Now we know the properties of each layer, the next step is to parametrically develop a numerical model to automate the steps above.
The benefit of parametric modelling is that one doesn't have to rebuild repetitive tasks for future applications, if the principle remains the same.

<img src="{{site.url}}/assets/2003_BucklingSimulation/Karamba_SimpleWall.png" style="display: block; margin: auto;" />
<img src="{{site.url}}/assets/2003_BucklingSimulation/karamba.JPG" style="display: block; margin: auto;" />

<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_sequence.jpg" style="display: block; margin: auto;" />

![image](/assets/2003_BucklingSimulation/ring_f02.gif) | ![image](/assets/2003_BucklingSimulation/ring_p02.gif)
![image](/assets/2003_BucklingSimulation/internal_f02.gif) | ![image](/assets/2003_BucklingSimulation/internal_p02.gif) 
![image](/assets/2003_BucklingSimulation/dual_f02.gif) | ![image](/assets/2003_BucklingSimulation/dual_p02.gif)

Finally, we reach buckling simulation outcomes.

When looking at the above simulation results, the **maximum displacement** is our failure indicator since we focus on elastic buckling. For our simulations we assumed a maximum displacement limit is **1cm** displacement, but this limit will be updated and verified in a later stage with physical experiments.

There are a few things to point out for using Karamba in such simulations.
1. A zero-thickness shell structure was selected as a cross-section element with a constant height applied. Karamba doesn't support volumetric elements, as such, the layer centre line needs to be used to generate the shell structure instead of the 3D layer geometry.
2. Spring cross-section was applied at the footing in order to define the stiffness relation over the buckling shapes.
3. Karamba's FEA calculations are based on the assumptions of small displacements in a ratio of about 10% compared to cross-section's dimension (in this case: the cross-section of the print layer), as such, complete failure of the structure will not be visualised in the simulation

One may ask, how can we improve the maximum printable height before elastic buckling occurs? Well, there are some factors we can work on:
1. Print slower: slower it prints, higher strength and stiffness are developed during printing.
2. Widen print layer width: wider it prints, more stable it will be.
3. Add internal structure.
4. Avoid long straight walls.
5. Tweak material properties, e.g. mixing cement accelerator.

Here is a quick example of how adding internal structure can help with minimising buckling compared to no internal structure.

<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_Crossing.gif" style="display: block; margin: auto;" />
<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_noCrossing.gif" style="display: block; margin: auto;" />

To conclude, this buckling simulation can be used as a quick analytical tool, allowing you to digitally predict what would happen prior to the physical experiments. This not only minimise the chance of unnecessary trial and error but also give a better understanding of improving 3D concrete printing process. 

The benefit of using Karamba3D in [Grasshopper][GH] environment in such simulation is that the computation duration is significantly reduced as it takes a matter of seconds to calculate the results.

Please contact me if you are interested in this tool: shaun.wu@witteveenbos.com

**[Download Example File][file]** 

Software requirements: [Rhino6][RH], [Karamba3D][KRB], [Human][HM]

----
**In Collaboration With**: [Witteveen+Bos][DCG], [Karamba3D][KRB], [Nanyang Technological University][NTU]

**Personnel**: [Jordy Vos][JV], [Shaun Wu][SW], Clemens Preisinger, Matthew Tam, [Nelson Xiong Neng][NXN]

**License**: [CC-BY-SA 4.0][CBA]

[ST]:  https://en.wikipedia.org/wiki/Concrete_slump_test
[FEA]: https://www.simscale.com/docs/content/simwiki/fea/whatisfea.html#:~:text=The%20Finite%20Element%20Analysis%20(FEA,to%20develop%20better%20products%2C%20faster.
[KRB]: https://www.karamba3d.com/
[AkSE]: https://www.sciencedirect.com/science/article/pii/S0020740317330370?via%3Dihub
[Ear]: https://www.sciencedirect.com/science/article/abs/pii/S000888461730532X?via%3Dihub
[TUE]: https://www.youtube.com/channel/UCBdyr8bya4GMyAjB2CJcf7Q/feed
[GH]: https://www.rhino3d.com/6/new/grasshopper
[RH]: https://www.rhino3d.com/
[HM]: https://www.food4rhino.com/app/human
[DCG]: https://digitalconstruction.witteveenbos.com/
[NTU]: https://www.ntu.edu.sg/Pages/home.aspx
[JV]: https://www.linkedin.com/in/jordy-vos/
[SW]: https://www.linkedin.com/in/shaun-wu/
[CBA]: https://creativecommons.org/licenses/by-sa/4.0/
[NXN]: https://www.linkedin.com/in/neng-xiong-ab144137/

[file]:{{ site.url }}/assets/2003_BucklingSimulation/200313_BucklingSimulation_SW.gh











