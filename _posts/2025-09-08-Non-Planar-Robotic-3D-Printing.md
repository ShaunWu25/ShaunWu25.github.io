---
layout: post
title:  "Non Planar Robotic 3D Printing"
date:   2025-09-08 00:00:00 +0100
categories: 
image: /assets/2509_NonPlanarRobotic3DPrinting/Frame_00150.bmp
tags: 
---

![image](/assets/2509_NonPlanarRobotic3DPrinting/231027_non planar+tilt.gif) | ![image](/assets/2509_NonPlanarRobotic3DPrinting/pot.jpg)

## Advancing Structural Integrity: 7-Axis Non-Planar Slicing for 3D Concrete Printing
In the world of 3D Concrete Printing (3DCP), we are often bound by the "2.5D" vertical straight up, nature of additive manufacturing. Traditional slicing algorithms decompose complex geometries into horizontal layers, which while effective for simple vertical forms but it presents significant structural and aesthetic limitations when dealing with complex curvature.

This post is about to share the development on a new Grasshopper plugin designed to overcome these constraints. By leveraging a 7-axis robotic setup (a KUKA robotis arm mounted on a linear track), this development focuses on Non-Planar Slicing specifically designed for the demands of structural 3DCP concrete applications.

## The Logic: Aligning with Geometry
The core of this slicer lies in its ability to read the geometry properties of the input design surface. Rather than aligning at global horizontal plane, the software analyzes the vectors from input surface’s isocurve directions and slicing tangents.

By computing these vectors, the software dynamically tilts the robotic end-effector. This tilting ensures that the print head remains perpendicular to the toolpath tangent at all times. The result is a print path that "flows" with the geometry.

Why Non-Planar? Structural Integrity First
The shift from planar to non-planar isn’t just about aesthetic smoothness; it is a necessity for high-performance concrete structures.

- Full Layer Bonding: In conventional planar printing, curved shapes are achieved through successive overhangs (the "staircase effect"). This creates weak points and reduces the contact area between layers.

- Optimal Compaction: By tilting the print head, we achieve full layer-to-layer contact throughout the entire curve. This maximizes bonding surface area, crucial for the structural integrity of load-bearing concrete elements.

- Geometric Freedom: It allows for the realization of more aggressive curvatures that would typically require extensive support structures or risk collapse during the wet-state phase of the concrete.

## The Technical Challenge: Navigating Singularities
Developing a toolpath for a 7-axis system (6-axis arm + 1-axis linear track) introduces significant complexity, particularly when the printing path is one continuous, non-planar curve.

The primary challenge is Robotic Singularity. In a continuous extrusion process, the robot cannot "reset" its configuration mid-print. As the end-effector tilts 3-dimensionally to follow isocurves, the software must constantly look ahead to ensure the joint configurations remain within reachable limits without reaching a singularity or axis limit.

The current development focuses on optimizing the inverse kinematics (IK) solver within Grasshopper to prioritize smooth joint transitions. This is to ensure that the transition from the linear track to the robotic wrist is uninterrupted.

## Real-World Application
I am proud that this non-planar slicing tool has been put to the test through a collaboration with Siam Cement Group [SCG].

SCG has utilized this plugin to push the boundaries of what is possible in the built environment, moving beyond simple walls to produce highly sophisticated 3D concrete printed objects such as bridges, urban furnture.

![image](/assets/2509_NonPlanarRobotic3DPrinting/SCG bridge.jpg) |  ![image](/assets/2509_NonPlanarRobotic3DPrinting/WhatsApp.jpeg)


[SCG]: https://scginternational.com/construction-tech/3d-printing/


