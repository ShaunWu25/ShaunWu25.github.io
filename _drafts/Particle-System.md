---
layout: post
---

Particle system

![image](/assets/2011_ParticleSystem/201109_01.gif) | ![image](/assets/2011_ParticleSystem/201109_02.gif)

I recently started to explore particle system. It is surprising that it only requires high school knowledge in physics to build up such tool.
Particle system can be used as a form-finding tool for designing a hanging chain model or cable nets structure that Frei Otto and Antoni Gaudi similar did in their physical experimentation.
This post will docuement what I have been developed step by step. By the end of this post, you will be seeing a simple physic engine built within [Rhino][RH]/[Grasshopper][GH] environment in C# programming language.

In fact, there are already existing physic engines available for Grasshopper users, such as [Kangaroo][KH], [Flexhopper][FH], or [PhysX.GH][PH].
The idea is not taking available tools for granted, it is important to understand its logic and use it consciously.
The aim of this post is trying to reveal the underlying principle of physic engines by explaining it in particle system.
Subsequently, I hope the reader(you!) can build your own physic engine, and develop it from scratch.

## C# Component Setup
<img src="{{site.url}}/assets/2011_ParticleSystem/201110_01.png" style="display: block; margin: auto;" height="200" />
The main scripting will take place in a C# scripting component. We will first set up our component and understand how the data is stored and processed. The initial input is a boolean button to reset the computational process and a timer attached at the bottom of the component to update the process in certain time interval. You can right click the timer to select your descired interval. The outputs will be the current position and velocity of particle. Remember to keep the timer disabled if you are not using it, otherwise the C# script component will run indefinitely in the background and consume your computer's memory heavily.
<img src="{{site.url}}/assets/2011_ParticleSystem/201110_02.JPG" style="display: block; margin: auto;" height="400" />
We can open up the C# scripting editor by double clicking the C# scripting component. The run script area is where the script will be executed. We set a condition to reset the process if we click the reset button and the output ports will extract the current particle information from the global scope area. The global scope is where we store particle's attributes as persistent data and the states of the particle is stored in every execution so we will be able to view the intermediate changes in run time.

## Particle
<img src="{{site.url}}/assets/2011_ParticleSystem/particle.png" style="display: block; margin: auto;" height="200" />
Particle can be described as a localised object. Each particle has its own set of attributes, such as position, velocity, mass, and other information that might be useful to attach with it. These attributes, however, are all dimensionless, we can of course logically assign certain units on it, but what we actually care is its kinematic movement. In other words, how particles move in a given environment.

## Falling ball simulation
<img src="{{site.url}}/assets/2011_ParticleSystem/201110_Falling ball.gif" style="display: block; margin: auto;" height="300" />

Particle is moved toward to a direction by a given or a set of force act on it. We will start from simulating a single particle or a falling ball and observe how the particle respond to its own weight. Gravity in this case, will be the only one force that act on the ball. To calculate the force act on the ball by gravity, we will borrow Newton's laws of motion *(F = m a)*.   
<img src="{{site.url}}/assets/2011_ParticleSystem/201110_02.png" style="display: block; margin: auto;" height="200" />
```c#
private void RunScript(bool button, double mass, double gravity, ref object Position, ref object Velocity)
  {

    if(button)
    {
      pt = Point3d.Origin;
      velocity = Vector3d.Zero;
    }
    else
    {
      Tuple<Point3d, Vector3d> compute = fall(pt, mass, gravity, velocity);
      pt += compute.Item1;
      velocity += compute.Item2;
    }
    Position = pt;
    Velocity = velocity;
  }

  // <Custom additional code> 
  Point3d pt = new Point3d(0, 0, 0);
  Vector3d velocity = new Vector3d(0, 0, 0);

  Tuple<Point3d, Vector3d> fall (Point3d pt, double mass, double gravity, Vector3d velocity)
  {
    double forceZ = mass * gravity;  // newton's law
    Vector3d accelerationZ = new Vector3d(0, 0, -forceZ / mass);

    Vector3d velocity_current = (velocity + accelerationZ);
    Point3d pt_position = pt + velocity;

    return Tuple.Create(pt_position, velocity_current);
  }
  // <Custom additional code>
```




[RH]: https://www.rhino3d.com/
[GH]: https://www.rhino3d.com/6/new/grasshopper
[KH]: https://www.food4rhino.com/app/kangaroo-physics
[FH]: https://www.food4rhino.com/app/flexhopper
[PH]: https://www.food4rhino.com/app/physxgh

