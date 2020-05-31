---
layout: post
title:  "Programming in Multi-Axis Gantry Robot for 3D Printing in Construction"
date:   2019-06-15 02:55:49 +0100
categories: 
image: /assets/1906_Gantry_Robot/3d-printer.jpg
tags: concrete, Witteveen+Bos, HDB, robotic
---

![image](/assets/1906_Gantry_Robot/3d-printer.jpg)

| ![image](/assets/1906_Gantry_Robot/Picture2.jpg) |
| *Concrete Printing in action* |

## Whar is Witteveen+Bos Robot?

**Witteveen+Bos Robot** is a Rhinoceros3D/Grasshopper plugin for programming multi-axis gantry robots with a focus on 3D concrete printing. It provides a seamless workflow from design to fabrication with full flexibilities and adaptabilities in a user-friendly matter. It consists of custom built-in components that facilitat the planning and execution of the robotic fabrication process embedded in the parametric design environment.

Witteveen+Bos robot has been chosen as a software solution by Singapore governmental agency, [Housing Development Board][HDB] in the [Center of Building Research][CBR] to conduct full-scale concrete printing technology for future generations of public housing in Singapore. 

![image](/assets/1906_Gantry_Robot/Picture11.jpg) | ![image](/assets/1906_Gantry_Robot/190725_reference.JPG)

Witteveen+Bos Robot is completely written in [C#][c] programming language in [.NET][net] framework that includes multiple strategies that are fully standardized for concrete printing operations. The users, are able to convert digital 3D designs into relevant robotic codes in just a few mouse clicks.

![image](/assets/1906_Gantry_Robot/Picture14.jpg) | 
*Convert 3d design to printable object* |

The generated robot codes can be loaded directly on the robot controllers, such as [G-Code][GC] for [Siemens Sinumerik][SS], [Rapid][RA] code for [ABB][ABB] robotic arm, [KUKA robot language][KR] for [KUKA][KK] robotic arm.

![image](/assets/1906_Gantry_Robot/Picture12.jpg) | ![image](/assets/1906_Gantry_Robot/ezgif.com-video-to-gif.gif) |
*Three linear-axis in gantry system* | *Three rotary-axis in gantry system* |

Witteveen+Bos Robot standardised three different printing strategies. These printing strategies utilise the unique 6-axis 3-dimensional orientations of 3D concrete printing system. Supported printing strategies are as follows:
1.	Vertical Printing.
2.	Tangential Printing. 
3.	On-Surface Printing.

## Vertical Printing

![image](/assets/1906_Gantry_Robot/Picture6.jpg) | <img src="/assets/1906_Gantry_Robot/Picture5.jpg" alt="image" width="345px">

Vertical printing strategy, the most commonly used for the concrete printing process. The printhead and nozzle are always perpendicular to the ground and printed object. The nozzle is rotated following along with the tangent of the print path.

## Tangential Printing

![image](/assets/1906_Gantry_Robot/Picture8.jpg) | <img src="/assets/1906_Gantry_Robot/Picture7.jpg" alt="image" width="650">

Tangential printing, meaning the printhead and nozzle are always following the curvature of the printed objects. Compare to the vertical printing strategy, the tangential printing strategy has a full-bond between the concrete layers for the curve objects. It is beneficial for printing those non-standard designs as the structural integrity will not be compromised by the complexity of geometry.

## On-Surface Printing

![image](/assets/1906_Gantry_Robot/Picture10.jpg) | 
![image](/assets/1906_Gantry_Robot/on-surface.gif) |

On-surface printing strategy allows us to print on a double-curved surface, the print head is following the surface normal at all time, and the nozzle is following the tangent of the print path 3-dimensionally.

<div class="video"> <figure> <iframe width="640" height="480" src="https://www.youtube.com/embed/-eJUiO6xcKE" frameborder="0" allowfullscreen></iframe> </figure> </div>
Digital workflow of the Witteveen+Bos Robot

----

**Developer**: [Shaun Wu][SW]

**Client**: [Housing Developement Board][HDB], Singapore

[HDB]: https://www.hdb.gov.sg/cs/infoweb/homepage
[CBR]: https://www.hdb.gov.sg/cs/infoweb/about-us/our-role/centre-of-building-research-page
[c]: https://docs.microsoft.com/en-us/dotnet/csharp/
[net]: https://dotnet.microsoft.com/
[GC]: https://en.wikipedia.org/wiki/G-code
[SS]: https://new.siemens.com/global/en.html
[RA]: https://en.wikipedia.org/wiki/RAPID
[ABB]: https://new.abb.com/
[KR]: https://en.wikipedia.org/wiki/KUKA_Robot_Language
[KK]: https://www.kuka.com/
[SW]: https://www.linkedin.com/in/shaun-wu/
