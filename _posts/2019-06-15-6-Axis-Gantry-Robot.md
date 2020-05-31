---
layout: post
title:  "Programming in multi-axis Gantry Robot for 3D Printing in Fresh Concrete"
date:   2020-03-09 02:55:49 +0100
categories: 
image: /assets/1906_Gantry_Robot/3d-printer.jpg
tags: concrete, Witteveen+Bos, HDB, robotic
---

![image](/assets/1906_Gantry_Robot/3d-printer.jpg)

| ![image](/assets/1906_Gantry_Robot/Picture2.jpg) |
| *Concrete Printing in action* |

## Whar is Witteveen+Bos Robot?

**Witteveen+Bos Robot** is a Rhinoceros3D/Grasshopper plugin for programming multi-axis gantry robots with a focus on 3D concrete printing. Witteveen+Bos Robot provides a seamless workflow from design to fabrication with full flexibilities and adaptabilities in an user-friendly matter. It consists out of custom built-in componenes that facilitates the planning and execution of robotic fabrication process embedded in the parametric design environment.

![image](/assets/1906_Gantry_Robot/Picture11.jpg) | ![image](/assets/1906_Gantry_Robot/190725_reference.JPG)

Witteveen+Bos Robot is completely written in [C#][c] programming language in [.NET][net] framework that includes multiple printing strategies which are fully standardized for concrete printing operations. The users, are able to convert digital 3D designs into relevant robotic codes in just a few mouse clicks.

![image](/assets/1906_Gantry_Robot/Picture14.jpg) | 
*Convert 3d design to printable object* |

The generated robot codes can be loaded directly on the robot controllers, such as [G-Code][GC] for [Siemens Sinumerik][SS], [Rapid][RA] for [ABB][ABB] robotic arm, [KUKA robot language][KR] for [KUKA][KK] robotic arm.

![image](/assets/1906_Gantry_Robot/Picture12.jpg) | ![image](/assets/1906_Gantry_Robot/ezgif.com-video-to-gif.gif) |
*Three linear-axis in gantry system* | *Three rotary-axis in gantry system* |

Witteveen+Bos Robot standardised three different printing strategies. The different printing strategies utilises the unique 6-axis movements of 3D-concrete printing system. The supported printing strategies are as follows:
1.	Vertical Printing.
2.	Tangential Printing. 
3.	On-Surface Printing.

## Vertical Printing

![image](/assets/1906_Gantry_Robot/Picture6.jpg) | ![image](/assets/1906_Gantry_Robot/Picture5.jpg)

Vertical printing strategy, the printhead and nozzle is always perpendicular to the ground and printed object. The nozzle is rotated following along with the tangent of the print path.

## Tangential Printing

![image](/assets/1906_Gantry_Robot/Picture8.jpg) | ![image](/assets/1906_Gantry_Robot/Picture7.jpg)

Tangential printing, meaning the printhead and nozzle is always following the curvature of printed object. Compare to the vertiacl printing strategy, the tangentail printing stategy has full-bond between the concrete layers for the curve objects. The structure integraty will not be compromised.

## On-Surface Printing

![image](/assets/1906_Gantry_Robot/Picture10.jpg) | ![image](/assets/1906_Gantry_Robot/on-surface.gif)

On-surface printing allows us to print on a double-curved surface, the print head is following the surface at all time, and the nozzle is following the tangent of the print path.

<div class="video"> <figure> <iframe width="640" height="480" src="https://www.youtube.com/embed/-eJUiO6xcKE" frameborder="0" allowfullscreen></iframe> </figure> </div>

**Developer**: [Shaun Wu][SW]

[c]: https://docs.microsoft.com/en-us/dotnet/csharp/
[net]: https://dotnet.microsoft.com/
[GC]: https://en.wikipedia.org/wiki/G-code
[SS]: https://new.siemens.com/global/en.html
[RA]: https://en.wikipedia.org/wiki/RAPID
[ABB]: https://new.abb.com/
[KR]: https://en.wikipedia.org/wiki/KUKA_Robot_Language
[KK]: https://www.kuka.com/
[SW]: https://www.linkedin.com/in/shaun-wu/
