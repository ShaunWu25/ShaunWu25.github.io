---
layout: post
title:  "Buckling Simulation for 3D Printing in Fresh Concrete"
date:   2020-03-09 02:55:49 +0100
categories: 
image: /assets/2003_BucklingSimulation/Cover.JPG
tags: concrete, Witteveen+Bos, Karamba, NTU
---

![image](/assets/2003_BucklingSimulation/200312_width.gif)

3d concrete printing technology has opened up an new era for constrcution industry as a new approach to construct concrete structures with an advantage of elminiating the need of customising one-used formwork/mold on non-standard geometries.

One major **challenge** to 3d print fresh concrete in layer-based extrusion process is to overcome **buckling** and **collapsing** as there is no formwork or mold to support fresh concrete stay in place. Here, we will focus on how to avoid buckling in 3d printing by means of [Finite Element Analysis][FEA] using [Karamba][KRB].

The term of **buckling** may sound academic, so to give you an idea what are we talking about here in graphic, please have a look at the video below. It was supposed to print a straight wall, but it tumbled down as the fresh concrete was _NOT_ stiff enough to support the self-weight deposited on top.

<img src="{{site.url}}/assets/2003_BucklingSimulation/buckling.gif" style="display: block; margin: auto;" />

There are 3 stages shown in the process.
1. **Onset of buckling**: subtle settlement occured, structure failed in elastic buckling
2. **During buckling**: structure failed shifted to plastic buckling.
3. **Collapsing**

What we are going to do is numerically simulating **Onset of buckling** to avoid the subsequest two stages.

This became an intertesting topic that traditionally fresh concrete can be checked in [slump test][ST], which allows you to measure the workability of freshly made concrete. In contrast to 3d concrete printing applications, there are no such official tests can be referenced. Here, we simply proposed to consider fresh concrete as **soil**, and this is where geotechnical testing methods come in, which we are not going to discuss in details.

The 3d concrete printing reseach program in TU/Eindhoven university from Netherland, has made fruitful research results for studying the strucure behavior of 3d printed concrete elements. Two relevant papers published by TU/E can be found:
1. [Early age mechanical behaviour of 3D printed concrete: Numerical modelling and experimenta; testing.][Ear]
2. [Mechanical performance of wall structure in 3D printing process: theory, design tools and experiements.][AkSE] 

As a benchmark, we used TU/E's setup to compare with our buckling simulation result. Setup as following:
* Material: Weber Beamix 145-1
* Weight: 2020 kg/m3
* Printing speed: 5000 mm/min
* Geometry: cylinder in radius 25cm
* Layer height: 1cm

![image](/assets/2003_BucklingSimulation/stiffness-module.JPG) | ![image](/assets/2003_BucklingSimulation/compressive-yeild-strength.JPG)

**Initial strength of fresh concrete** and **strength development of fresh concrete** are two important factors to determine the structure behaviour of the 3d printing process. It has directly impact on setting a time-dependent material propertise, meaning the strength and stiffness are developed along the curing time.

Based on the graphs above, we know:
* **Stiffness Module**[MPa] = 0.0781 + 0.0012t
* **Compressive Strength**[kPa] = 5.984 + 0.147t

Depend on the print length of each layer and the printing speed, we can calculate what is the actual **age** of individual layer:

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

Upon the age of each layer, we can then acquire the acutal **young's module** and **compressive strength** of individual layer, which are what we need as inputs for runing FEA in Karamba:

<img src="{{site.url}}/assets/2003_BucklingSimulation/strengthDevelopment.JPG" style="display: block; margin: auto;" />

Now we have all the ingredients we need, next step is to parametrically develop a numerical model to automate the steps above.
The benefit of the parametric modelling is that one don't have to rebuild repetitive tasks if the principle remains the same.

<img src="{{site.url}}/assets/2003_BucklingSimulation/Karamba_SimpleWall.png" style="display: block; margin: auto;" />

Finally, we reach buckling simulation outcome. 

<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_sequence.jpg" style="display: block; margin: auto;" />

One may ask, how we can improve the process to avoid buckling occured? well, there are some factors we can work on:
1. Print slower: slower you print, higher strength and stiffness are developed.
2. Widener print width: wider you print, more stable it will be.
3. Add internal strucutre.
4. Tweak material propertise, e.g. mixing cement accelerator.

Here is quick example of how adding internal structures can help with minimising buckling compared to no internal structure.

<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_Crossing.gif" style="display: block; margin: auto;" />
<img src="{{site.url}}/assets/2003_BucklingSimulation/200312_noCrossing.gif" style="display: block; margin: auto;" />

[try][1]



[1]:{{ site.url }}/assets/strengthDevelopment.JPG

----
**In Collaboration With**: Witteveen+Bos, Karamba 3D, Nanyan Technology University

**Personnel**: Jordy Vos, Shaun Wu, Clemens, Matthew,  Nelson

[ST]:  https://en.wikipedia.org/wiki/Concrete_slump_test
[FEA]: https://www.simscale.com/docs/content/simwiki/fea/whatisfea.html#:~:text=The%20Finite%20Element%20Analysis%20(FEA,to%20develop%20better%20products%2C%20faster.
[KRB]: https://www.karamba3d.com/
[AkSE]: https://www.sciencedirect.com/science/article/pii/S0020740317330370?via%3Dihub
[Ear]: https://www.sciencedirect.com/science/article/abs/pii/S000888461730532X?via%3Dihub


[SDC]: https://gramaziokohler.arch.ethz.ch/web/e/forschung/223.html
[GKR]: https://gramaziokohler.arch.ethz.ch/web/e/forschung/index.html
[ELF]: https://gramaziokohler.arch.ethz.ch/web/e/team/106.html




