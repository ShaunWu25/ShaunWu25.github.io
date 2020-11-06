---
layout: post
---

Particle system

I recently started to explore particle system. It is surprising that it only requires high school knowledge in physics to build up such tool.
This post will docuement what I have been developed step by step. By the end of this post, you will be seeing a simple physic engine built within [Rhino][RH]/[Grasshopper][GH] environment in C# programming language.

In fact, there are already existing physic engines available for Grasshopper users, such as [Kangaroo][KH], [Flexhopper][FH], or [PhysX.GH][PH].
The idea is not taking available tools for granted, it is important to understand its logic and use it consciously.
The aim of this post is trying to reveal the underlying principle of physic engines by explaining it in particle system.
Subsequently, I hope the reader(you!) can build your own physic engine, and develop it from scratch.

## Particle
<img src="{{site.url}}/assets/2011_ParticleSystem/particle.png" style="display: block; margin: auto;" height="200" />
Particle can be described as a localised object. Each particle has its own set of attributes, such as position, velocity, mass, and other information that might be useful to attach with it. These attributes, however, are all dimensionless, we can of course logically assign certain units on it, but what we actually care is its kinematic movement. In other words, how particles move in a given environment.

## Physics
Particle is moved toward to a direction by a given or a set of force act on it. We will start from simulating a falling ball and observe how the ball respond to its own weight. Gravity in this case, will be the only one force that act on the ball. To calculate the gravity force, we will borrow Newton's laws of motion *(F = m a)*. Once we obtain the  


[RH]:
[GH]:
[KH]:
[FH]:
[PH]

