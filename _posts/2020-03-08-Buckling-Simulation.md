---
layout: post
title:  "Buckling Simulation for 3D Printing in Fresh Concrete"
date:   2020-03-09 02:55:49 +0100
categories: 
image: /assets/2003_BucklingSimulation/Cover.JPG
tags: concrete, Witteveen+Bos, Karamba, NTU
---

![image](/assets/2003_BucklingSimulation/200312_width.gif)

3d concrete printing technology has opened up a new era for the construction industry as a new approach to construct concrete structures with an advantage of eliminating the need of customising one-used formwork/mold on non-standard geometries. [Witteveen+Bos][DCG] has been developing 3d concrete printing technology globally.

One major **challenge** to 3d print fresh concrete in layer-based extrusion process is to overcome **buckling** and **collapsing** as there is no formwork or mold to support fresh concrete stay in shape. Here, we will focus on how to avoid buckling in 3d printing through [Finite Element Analysis][FEA] using [Karamba 3D][KRB].

To give an idea of what **buckling** are we talking about here in graphic, please have a look at the gif below. It was supposed to print a straight wall, but it tumbled down as the fresh concrete was _NOT_ stiff enough to support the self-weight deposited on top.

<img src="{{site.url}}/assets/2003_BucklingSimulation/buckling.gif" style="display: block; margin: auto;" />

There are 2 stages shown in the process.
1. **Onset of buckling**: subtle settlement occurred as buckling.
2. **Collapsing**: structure failing.

What we are going to do is numerically simulating **Onset of buckling** to avoid the subsequent two stages.

This became an interesting topic that traditionally fresh concrete can be checked in [slump test][ST], which allows you to measure the workability of freshly made concrete. In contrast to 3d concrete printing applications, there are no such official tests can be referenced. Therefore, we simply proposed to consider fresh concrete as **soil**, and this is where geotechnical testing methods come in, which we are not going to discuss in detail.

The 3d concrete printing research program at [TU/Eindhoven][TUE] University, Netherland, has made fruitful research results for studying the structural behavior of 3d printed concrete elements. Two relevant papers published by TU/E can be found:
1. [Early age mechanical behaviour of 3D printed concrete: Numerical modeling and experimental testing.][Ear]
2. [Mechanical performance of wall structure in 3D printing process: theory, design tools and experiments.][AkSE] 

As a benchmark, we used TU/E's setup to compare with our buckling simulation result. 
Setup as following:
1. Material: Weber Beamix 145-1
2. Printing Speed: 5000 mm/min
3. Print width: 5cm
4. Geometry: cylinder in radius 25cm
5. Layer height: 1cm

![image](/assets/2003_BucklingSimulation/stiffness-module.JPG) | ![image](/assets/2003_BucklingSimulation/compressive-yeild-strength.JPG)

**Initial strength of fresh concrete** and **strength development of fresh concrete** are two important factors to determine the structural behaviour of the 3d printing process. It has a direct impact on setting time-dependent material properties, meaning the strength and stiffness of fresh concrete are developed along the curing time.

Based on the graphs above, we know:
* **Stiffness Module**[MPa] = 0.0781 + 0.0012t
* **Compressive Strength**[kPa] = 5.984 + 0.147t

Depend on the print length of each layer and the printing speed, we can calculate what is the actual **age** of the individual layer:

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

Upon the age of each layer, we can then acquire the actual **young's module** and **compressive strength** of individual layer:

<img src="{{site.url}}/assets/2003_BucklingSimulation/strengthDevelopment.JPG" style="display: block; margin: auto;" />

Now we have all the ingredients we need, the next step is to parametrically develop a numerical model to automate the steps above.
The benefit of parametric modelling is that one doesn't have to rebuild repetitive tasks if the principle remains the same.

<img src="{{site.url}}/assets/2003_BucklingSimulation/Karamba_SimpleWall.png" style="display: block; margin: auto;" />
<img src="{{site.url}}/assets/2003_BucklingSimulation/karamba.JPG" style="display: block; margin: auto;" />

Finally, we reach buckling simulation outcomes. 
As reminders, there are a few things I want to point out for using Karamba in such simulations.
1. Shell structure was selected as a cross-section element with a constant height. Karamba doesn't support volumetric elements.
2. Spring cross-section was applied at the footing in order to define the stiffness relation over the buckling shapes.
3. Karamba's FEA calculations are based on the assumptions of small displacements in ratio of about 10% compared to cross-section's dimension.  

<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_sequence.jpg" style="display: block; margin: auto;" />

![image](/assets/2003_BucklingSimulation/ring_f02.gif) | ![image](/assets/2003_BucklingSimulation/ring_p02.gif)
![image](/assets/2003_BucklingSimulation/internal_f02.gif) | ![image](/assets/2003_BucklingSimulation/internal_p02.gif) 
![image](/assets/2003_BucklingSimulation/dual_f02.gif) | ![image](/assets/2003_BucklingSimulation/dual_p02.gif)

One may ask, how can we improve the process to avoid buckling occurred? well, there are some factors we can work on:
1. Print slower: slower it prints, higher strength and stiffness are developed.
2. Widen print width: wider it prints, more stable it will be.
3. Add internal structure.
4. Avoid long stretch wall.
5. Tweak material properties, e.g. mixing cement accelerator.

Here is a quick example of how adding internal structure can help with minimising buckling compared to no internal structure.

<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_Crossing.gif" style="display: block; margin: auto;" />
<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_noCrossing.gif" style="display: block; margin: auto;" />

To conclude, this buckling simulation can be used as a quick analytical tool, allowing you to digitally predict what would happen prior to the physical experiments. This not only minimise the chance of unnecessary trial and error but also give a better understanding of improving 3d concrete printing process. 

The benefit of using Karamba3D in [Grasshopper][GH] environment in such simulation is that the computation duration is significantly reduced as it takes only a few seconds to calculate the buckling result with 35 layers attached.

Please contact me if you found interested in this tool: shaun.wu@witteveenbos.com

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
[JV]: https://www.linkedin.com/feed/
[SW]: https://www.linkedin.com/in/shaun-wu/
[CBA]: https://creativecommons.org/licenses/by-sa/4.0/
[NXN]: https://www.linkedin.com/in/neng-xiong-ab144137/

[file]:{{ site.url }}/assets/2003_BucklingSimulation/200313_BucklingSimulation_SW.gh











