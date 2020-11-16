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
The main scripting will take place in a C# scripting component. We will first set up our component and understand how the data is stored and processed. The initial input is a boolean button to reset the computational process and a timer attached at the bottom of the component to update the process in certain time interval. You can right click the timer to select your desired interval. The outputs will be the current position and velocity of the particle. Remember to keep the timer disabled if you are not using it, otherwise the C# script component will be running indefinitely in the background and consume your computer's memory heavily.
<img src="{{site.url}}/assets/2011_ParticleSystem/201110_02.JPG" style="display: block; margin: auto;" height="400" />
We can open up the C# scripting editor by double-clicking the C# scripting component. The run script area is where the script will be executed. We set a condition to reset the process if we click the reset button and the output ports will extract the current particle information from the global scope area. The global scope is where we store particle's attributes as persistent data and the states of the particle is stored in every execution so we will be able to view the intermediate changes in run time.

## Particle
<img src="{{site.url}}/assets/2011_ParticleSystem/particle.png" style="display: block; margin: auto;" height="200" />
Particle can be described as a localised object. Each particle has its own set of attributes, such as position, velocity, mass, and other information that might be useful to attach with it. These attributes, however, are all dimensionless, we can of course logically assign certain units on it, but what we actually care is its kinematic movement. In other words, how particles move in a given environment.

## Falling ball simulation
<img src="{{site.url}}/assets/2011_ParticleSystem/201110_Falling ball.gif" style="display: block; margin: auto;" height="300" />

Particle is moved toward to a direction by a given or a set of force act on it. We will start from simulating a single particle or a falling ball and observe how the particle respond to its own weight. Gravity in this case, will be the only force that act on the ball. To calculate the force act on the ball by gravity, we will borrow Newton's laws of motion *(F = m a)*.   
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
    Vector3d accelerationZ = new Vector3d(0, 0, -forceZ / mass); // calculate the acceleration of the particle, forceZ is inverted

    Vector3d velocity_current = (velocity + accelerationZ); // update velocity
    Point3d pt_position = pt + velocity; // update position 

    return Tuple.Create(pt_position, velocity_current);
  }
  // <Custom additional code>
```

That's it! We have made our very first physic engine! To make the ball falling downward in our coodinate system, the force Z is therefore inverted at the line where we calculate the acceleration.
Now let's make one step further by adding a bit complexity for a spring simulation.  

## Single Spring Simulation

<img src="{{site.url}}/assets/2011_ParticleSystem/201112_bouncing balll.gif" style="display: block; margin: auto;" height="500" />

Single spring simulation has two additional forces compared to our previous falling ball simulation:
1. Spring Force (Force = Spring Constant * Δ L(Length - Rest Length)): <br />
It is widely known as [Hooke's Law][HL] that a force is exerted if an elasitc spring is compressed or extended in relation with its rest length. Rest length means the length of spring in a state of equilibrium without any force exerted on it. The value of the spring constant represents the stiffness of the spring which can be a reasonable arbitary none zero value for our simulation.

2. Damping Force (Force = Damping Constant * Particle's Velocity): <br />
Damping force is always remained opposite to the direction of the spring force. It is the force to slow dowm the particle's motion by associating with particle's velocity. Without the damping force, our system will not reach to an equilibrium, the dynamic particle will keep bocuning forth and back indefinitely. You can try to set the damping constant to 0 to see the motion. Similar to the spring constant, damping constant is a arbitary value.

In addition, we will add one more particle(particle 1) at the origin as our anchoring point which remains constantly static without being affected by any forces. Particle 2 will be the falling ball particle with a spring connected to particle 1. Below is a diagram that illustrates how three forces interact with each other. 

<img src="{{site.url}}/assets/2011_ParticleSystem/1112_01.JPG" style="display: block; margin: auto;" height="400" />

<img src="{{site.url}}/assets/2011_ParticleSystem/201112_01.png" style="display: block; margin: auto;" height="300" />

```c#
private void RunScript(bool button, double mass, double gravity, double spring, double damping, Point3d anchorPt, Point3d StartingPt, ref object Position, ref object Velocity)
  {
    if(button)
    {
      pt = StartingPt;
      velocity = Vector3d.Zero;
    }
    else
    {
      Tuple<Point3d, Vector3d> compute = physic_engine(pt, mass, gravity, velocity, damping, anchorPt, spring);
      pt = compute.Item1;
      velocity = compute.Item2;
    }
    Position = pt;
    Velocity = velocity;
  }

  // <Custom additional code> 
  Point3d pt = new Point3d(0, 0, 0);
  Vector3d velocity = new Vector3d(0, 0, 0);

  Tuple<Point3d, Vector3d> physic_engine (Point3d pt, double mass, double gravity, Vector3d velocity, double damping, Point3d anchorPt, double spring)
  {
    double forceSpring_Z = -spring * (pt.Z - anchorPt.Z);
    double forceSpring_Y = -spring * (pt.Y - anchorPt.Y);
    double forceSpring_X = -spring * (pt.X - anchorPt.X);

    double forceDamping_Z = damping * velocity.Z;
    double forceDamping_Y = damping * velocity.Y;
    double forceDamping_X = damping * velocity.X;

    double forceGravity = mass * -gravity;

    double forceZ = forceSpring_Z - forceDamping_Z + forceGravity ;
    double forceY = forceSpring_Y - forceDamping_Y;
    double forceX = forceSpring_X - forceDamping_X;

    Vector3d acceleration = new Vector3d(forceX / mass, forceY / mass, forceZ / mass);

    Vector3d velocity_current = (velocity + acceleration);
    Point3d pt_current = pt + velocity_current;

    return Tuple.Create(pt_current, velocity_current);
  }
  // </Custom additional code> 
```

## Two Springs Simulation

<img src="{{site.url}}/assets/2011_ParticleSystem/201116_two springs.gif" style="display: block; margin: auto;" height="500" />

Next step we will add one additional dynamic particle(particle 3) with a spring connected to the particle 2. We will first examine the force diagram to see what forces act on each dynamic particles and formulate the force structure accordingly.

<img src="{{site.url}}/assets/2011_ParticleSystem/1116_01.JPG" style="display: block; margin: auto;" height="400" />

We first look at the force diagram for the particle 2 that it has one gravity force from its own weight and a spring force plus a damping force that are associated with the static particle 1. In addition, a newly created particle 3 is connected with a spring below the particle 2 which exert one additional spring force and a damping force.<br />

It is clear now that the number of forces act on the particle is linked to the particle's connectivity. Among the three particles in this simulation, the particle 2 has 2 connectivities with the static particle 1 and a dynamic particle 3 below it. The others two particles remains simple with 1 connectivity.

<img src="{{site.url}}/assets/2011_ParticleSystem/1116_02.JPG" style="display: block; margin: auto;" height="400" />

Just like our previous single spring simulation, the particle 3 is only associated with the particle 2, so the force diagram is the same to the previous force diagram for the particle 2.

<img src="{{site.url}}/assets/2011_ParticleSystem/201116_01.png" style="display: block; margin: auto;" height="300" />

```c#
private void RunScript(bool button, double mass_01, double mass_02, double gravity, double k, double damping, Point3d anchorPt, Point3d StartingPt_01, Point3d StartingPt_02, ref object Position01, ref object Position02, ref object Velocity01, ref object Velocity02)
  {
    if(button)
    {
      pt_1 = StartingPt_01;
      pt_2 = StartingPt_02;

      velocity_01 = Vector3d.Zero;
      velocity_02 = Vector3d.Zero;
    }
    else
    {
      Tuple<Point3d, Point3d, Vector3d, Vector3d> compute = physic_engine(pt_1, pt_2, mass_01, mass_02, gravity, velocity_01, velocity_02, damping, anchorPt, k);

      pt_1 = compute.Item1;
      pt_2 = compute.Item2;

      velocity_01 = compute.Item3;
      velocity_02 = compute.Item4;

    }
    Position01 = pt_1;
    Position02 = pt_2;
    Velocity01 = velocity_01;
    Velocity02 = velocity_02;
  }

  // <Custom additional code> 
  Point3d pt_1 = new Point3d(0, 0, 0);
  Point3d pt_2 = new Point3d(0, 0, 0);

  Vector3d velocity_01 = new Vector3d(0, 0, 0);
  Vector3d velocity_02 = new Vector3d(0, 0, 0);

  Tuple<Point3d, Point3d, Vector3d, Vector3d> physic_engine (Point3d pt_1, Point3d pt_2, double mass_01, double mass_02, double gravity, Vector3d velocity_01, Vector3d velocity_02, double damping, Point3d anchorPt, double k)
  {
    // spring force for the particle 2
    double forceSpring_Z_01 = -k * (pt_1.Z - anchorPt.Z);
    double forceSpring_Y_01 = -k * (pt_1.Y - anchorPt.Y);
    double forceSpring_X_01 = -k * (pt_1.X - anchorPt.X);
    // spring force for the particle 3
    double forceSpring_Z_02 = -k * (pt_2.Z - pt_1.Z);
    double forceSpring_Y_02 = -k * (pt_2.Y - pt_1.Y);
    double forceSpring_X_02 = -k * (pt_2.X - pt_1.X);

    // damping force for the particle 2
    double forceDamping_Z_01 = damping * velocity_01.Z;
    double forceDamping_Y_01 = damping * velocity_01.Y;
    double forceDamping_X_01 = damping * velocity_01.X;
    // damping force for the particle 3
    double forceDamping_Z_02 = damping * velocity_02.Z;
    double forceDamping_Y_02 = damping * velocity_02.Y;
    double forceDamping_X_02 = damping * velocity_02.X;

    // gravity force for the particle 2
    double forceGravity_01 = mass_01 * gravity;
    // gravity force for the particle 3
    double forceGravity_02 = mass_02 * gravity;

    // calculatie the individual force for particle 2
    double forceZ_01 = forceSpring_Z_01 - forceDamping_Z_01 - forceSpring_Z_02 + forceDamping_Z_02 - forceGravity_01;
    double forceY_01 = forceSpring_Y_01 - forceDamping_Y_01 - forceSpring_Y_02 + forceDamping_Y_02;
    double forceX_01 = forceSpring_X_01 - forceDamping_X_01 - forceSpring_X_02 + forceDamping_X_02;
    // calculatie the individual force for particle 3
    double forceZ_02 = forceSpring_Z_02 - forceDamping_Z_02 - forceGravity_01;
    double forceY_02 = forceSpring_Y_02 - forceDamping_Y_02;
    double forceX_02 = forceSpring_X_02 - forceDamping_X_02;

    // calculate the acceleration of the particle 2
    Vector3d acceleration_Z_01 = new Vector3d(forceX_01 / mass_01, forceY_01 / mass_01, forceZ_01 / mass_01);
    // calculate the acceleration of the particle 3
    Vector3d acceleration_Z_02 = new Vector3d(forceX_02 / mass_02, forceY_02 / mass_02, forceZ_02 / mass_02);

    Vector3d velocity_01_current = (velocity_01 + acceleration_Z_01);
    Vector3d velocity_02_current = (velocity_02 + acceleration_Z_02);

    Point3d pt_01_current = pt_1 + velocity_01_current;
    Point3d pt_02_current = pt_2 + velocity_02_current;

    return Tuple.Create(pt_01_current, pt_02_current, velocity_01_current, velocity_02_current);
  }
```

The code is getting longer because we basically need to double up the calculation for the extra particle involved. As you can see it is not the best practice because if we have 10 more particles involved in the system, then the code will be easily 10 times longer. To solve this issue, we will make use of [Object-Oriented Programming][OOP] to better maintain our code structure.

## Treating Particle as Object
Earlier we mentioned every particle has it's own set of attributes, which we can make use of it by establishing a particle class that we associate these attributes with every particle we generate. Nn the later stage, we can easily extract these information whenever we update its position, velocity, etc. At this point, what we care for each particle are:
1. Position
2. Velocity
3. Status: Is it static or dynamic.
4. Mass

```c#
public class Particle
    {
        public Point3d Position;
        public Vector3d Velocity;
        public int Status;
        public double Mass;

        public Particle(Point3d initialPosition, Vector3d initialVelocity, double mass, int particleStatus)
        {
            Position = initialPosition;
            Velocity = initialVelocity;
            Status = particleStatus;
            Mass = mass;
        }
    }
```

## Searching Connectivity
In our two springs simulation, we can clearly see the connectivity of each particle because there are simply three particles involved in the system. To tackle more complicated connectivity like a cable nets structure, we will make a utility function to search the connectivity of each particle. The function will iterate every spring we feed in, look for the particles from the start and end point of the spring, store the particle in the list if they are not duplicate to the stored list, then build the connectivity by the particle's index.

```c#
Tuple<List<Point3d>, DataTree<int>> SearchConnectivity(List<Curve> curves)
    {
        List<Point3d> myPts = new List<Point3d>();
        DataTree<int> connectivity = new DataTree<int>();
        for (int i = 0; i < curves.Count; i++)
        {
            Point3d startPt = curves[i].PointAtStart;
            Point3d endPt = curves[i].PointAtEnd;

            if (i == 0)
            {
                myPts.Add(endPt);
                myPts.Add(startPt);

                connectivity.Add(myPts.IndexOf(endPt), new GH_Path(myPts.IndexOf(startPt)));
                connectivity.Add(myPts.IndexOf(startPt), new GH_Path(myPts.IndexOf(endPt)));
            }
            else
            {
                if (myPts.Contains(startPt) && myPts.Contains(endPt))
                {
                    connectivity.Add(myPts.IndexOf(endPt), new GH_Path(myPts.IndexOf(startPt)));
                    connectivity.Add(myPts.IndexOf(startPt), new GH_Path(myPts.IndexOf(endPt)));
                }
                else if (myPts.Contains(startPt))
                {
                    myPts.Add(endPt);
                    connectivity.Add(myPts.IndexOf(endPt), new GH_Path(myPts.IndexOf(startPt)));
                    connectivity.Add(myPts.IndexOf(startPt), new GH_Path(myPts.IndexOf(endPt)));
                }
                else if (myPts.Contains(endPt))
                {
                    myPts.Add(startPt);
                    connectivity.Add(myPts.IndexOf(endPt), new GH_Path(myPts.IndexOf(startPt)));
                    connectivity.Add(myPts.IndexOf(startPt), new GH_Path(myPts.IndexOf(endPt)));
                }
            }
        }
        return Tuple.Create(myPts, connectivity);
    }
```



[RH]: https://www.rhino3d.com/
[GH]: https://www.rhino3d.com/6/new/grasshopper
[KH]: https://www.food4rhino.com/app/kangaroo-physics
[FH]: https://www.food4rhino.com/app/flexhopper
[PH]: https://www.food4rhino.com/app/physxgh
[HL]: https://en.wikipedia.org/wiki/Hooke%27s_law
[OOP]: https://en.wikipedia.org/wiki/Object-oriented_programming#:~:text=Object%2Doriented%20programming%20(OOP),(often%20known%20as%20methods).
