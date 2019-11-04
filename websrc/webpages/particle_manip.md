
@page particle_manip Particle manipulation

@tableofcontents

@section particle Use the Particle System.
By default, SpaceHub provides you a particle type to create initial conditions. The default type provides you,

- @bflabel{mass} The mass of particle.
- @bflabel{radius} The radius of the particle.
- @bflabel{pos} The position of the particle. 3D vector with (x,y,z) components.
- @bflabel{vel} The velocity of the particle. 3D vector with (x,y,z) components.

The particle type is associated with the simulator type, so you can get the particle type by from the Simulator type.

```cpp
 ...
 //import the unit system of spaceHub
 using namespace space::unit;
 //Use the associated particle type of the default simulator of SpaceHub
 using Particle = typename DefaultSolver::Particle;
 ...
 //1.Create particle without any parameter.
 Particle particle1;
 particle1.mass = 1_Ms;
 particle1.radius = 1_Rs;
 ...
 //2. Create a particle with 8 parameters.
 //
 //                               px     py     pz    vx     vy      vz
 //                               |      |      |     |      |       |
 Particle particle2(1_Ms, 1_Rs, 1_AU, 0.4_AU, 0_AU, 2_kms, -3_kms, 0_kms);

//Use the embedded 3d Vector type of the Particle.
 using Vector = typename Particle::Vector;

//define the position and velocity vectors.
 Vector pos(3_PC, 2.5_PC,-4_Pc), vel(0.2_kms, -1.2_kms, 3_kms)

//3. Create a particle with mass, radius, position and velocity.
 Particle particle3(1_Ms, 1_Rs, pos, vel);

//4. create a particle without providing the position and velocity. Then the position and velocity will be (0,0,0) and (0,0,0);
 Particle particle4(1_Ms, 1_Rs);
 ```

See also space::particle_set::SizeParticles::Particle, which is the default particle type.

@section move_particle1 Move the particles to specific position and velocity.

Indeed, what makes you much easier to create the initial conditions are the four powerful functions provided by SpaceHub.

- space::orbit::move_particles_pos
- space::orbit::move_particles_vel
- space::orbit::move_particles
- space::orbit::move_to_COM_frame
 
Associated with the Kepler orbit we introduced in last tutorial, you can create any initial conditions in a very easy way.

First, we will introduce you `move_particles_pos` and `move_particles_vel`, which are used to move the particles to a specific position or velocity. The two
functions accept arbitrary number of arguments, the first is the target position/velocity you want to move to and the rests are the particles you are going to move.

```cpp
 ...
 using namespace space::unit;
 using namespace space::orbit;//import the functions above.
 using Particle = typename DefaultSolver::Particle;
 using Vector = typename Particle::Vector;
 ...

 //All the three particles are rest at origin.
 Particle p1(1_Ms, 1_Rs), p2(1_Ms, 3_km), p3(1.4_Ms, 10_km);

 //move particle p1 to (1_AU, 2_AU, 0) which is equivalent to  p1.pos = Vector(1_AU, 2_AU, 0).
 move_particles_pos(Vector(1_AU, 2_AU, 0), p1);
 //move particle p2 to (5_AU, 0, -3_AU).
 move_particles_pos(Vector(5_AU, 0, -3_AU), p2);

 //move the *centre of mass* of p1 and p2 to (-10_AU, 2_AU, 3_AU) and *keep their relative position*
 move_particles_pos(Vector(-10_AU, 2_AU, 3_AU), p1, p2);

 //move_the centre of mass of p1, p2 and p3 to origin equivalent.
 move_particles_pos(Vector(0), p1, p2, p3);
 ```

 You can do the same things with `move_particles_vel` to move the velocities of the particles by providing the target velocity in 3D vector. The way that SpaceHub saves your time here is providing you a way to move a bulk of particles at the same time with keeping their relative position unchanged.

 The function `move_particles` is designed to move the position and velocity at the same time. The first argument is the target position, the second argument is the target velocity and the rest are the particles you are going to move. You can do it in this way,

```cpp
 ...

 //move the *centre of mass* of p1, p2 and p3 to (-10_AU, 2_AU, 3_AU) and the centre of mass velocity to (2_km, 0.3_km, -3_km)
 // with keeping their relative position and relative velocity. Here they overlap with each other.
 move_particles_pos(Vector(-10_AU, 2_AU, 3_AU), Vector(2_km, 0.3_km, -3_km), p1, p2, p3);

 ```

 The last function `move_to_COM_frame` is used to move a bulk of particles to the centre of mass reference frame and set the centre of mass of the particles to origin.

 ```cpp
 ...
 move_to_COM_frame(p1, p2, p3);

 ```

 If you have 100 particles need to move, it would be cumbersome to pass 100 particles as the parameter of the function above, nor to define 100 particle variables. The way, we usually do is to put the particles in an array(Container). The four functions above can also move the particles in a STL container, e.g, std::array, std::vector, std::list...

 ```cpp
 #include<array>
 ...
 using Particle = typename DefaultSolver::Particle;
 using Vector = typename Particle::Vector;
 ...
 //An array with 100 particles
 std::array<Particles,100> particles;
 //initializations on the 100 particles. we will provide specific examples later.
 ...
 //Move the 100 particles
 move_particles(Vector(2_AU, 0, 0), Vector(0, 0.3_kms,0), particles);

 ```

 @section move_kepler Move to Kepler orbit
 What SpaceHub really save you from calculating the initial conditions is to combine the move functions above with Kepler orbit in last tutorial. You can pass a orbit variable the function `move_particles` to move the particles to the corresponding position and velocity of that orbit. For example, to create the sun-earth-moon system.

 ```cpp
 
 ```

@m_class{m-note m-dim m-text-center}

@parblock
  <a href="create_kepler.html"> << Create Kepler orbit </a> | <a href="particle_manip.html"> Particle manipulation >> </a> 
@endparblock