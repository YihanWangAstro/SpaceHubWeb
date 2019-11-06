
@page particle_manip Particle manipulation

@tableofcontents

@section particle Use the Particle System
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
 //1.Create particle without any parameter, mass = 0, radius = 0.
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

@section move_particle1 Move the particles

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

@m_class{m-block m-danger}

@par What you can't
@parblock
 ```cpp
 #include<array>
 ...
 using Particle = typename DefaultSolver::Particle;
 using Vector = typename Particle::Vector;
 ...
 Particles p1(1_Ms,1_Rs);
 //An array with 100 particles
 std::array<Particles,100> particles;
 //initializations on the 100 particles. we will provide specific examples later.
 ...
 //                                             single particle  particles container
 //                                                       |       |
 move_particles(Vector(2_AU, 0, 0), Vector(0, 0.3_kms,0), p1, particles);
 ```
@endparblock
 


@section move_kepler Move particles to Kepler orbit
The way that SpaceHub really save you from calculating complex initial conditions is to combine the move functions above with Kepler orbit in last tutorial. You can pass a orbit variable as the first parameter of `move_particles` to move the particles to the corresponding position and velocity of that orbit. For example, to create the sun-earth-moon system.

 ```cpp
 using namespace space::unit;
 using namespace space::orbit;
 using Particle = typename DefaultSolver::Particle;
 ...
 // create three particles. particles are rest at origin.
 Particle sun(1_Ms), earth(1_Me), moon(1_Mmoon);

 auto moon_orbit = EllipOrbit(earth.mass, moon.mass, 384748_km , 0.0549, 1.543_deg, 0.0, 0.0, isotherm);

 //Move the moon to the corresponding position and velocity of the moon orbit. Now the earth rest at origin and moon form a
 //[earth, moon] system.
 move_particles(moon_orbit, moon);

 auto earth_orbit = EllipOrbit(sun.mass, earth.mass + moon.mass, 1_AU, 0.016, 7.155_deg, 174.9_deg, 288.1_deg, isotherm);

 // move the centre of mass of the moon and earth to the earth orbit. Now the sun rest at origin and the earth-moon system form a
 //[sun, [earth, moon]] system.
 move_particles(earth_orbit, earth, moon);

 // Move them to the centre of mass reference frame.
 move_to_COM_frame(sun, earth, moon);
 ```

|                     2D Cartons of generating [sun, [earth, moon]] system                     |                                                                                   |
| :------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
| @image html tutorial/SEM1.png 1. Particle sun(1_Ms), earth(1_Me), moon(1_Mmoon); width=400px |  @image html tutorial/SEM2.png 2. move_particles(moon_orbit, moon); width=400px   |
|    @image html tutorial/SEM3.png 3. move_particles(earth_orbit, earth, moon); width=400px    | @image html tutorial/SEM4.png 4. move_to_COM_frame(sun, earth, moon); width=400px |

 You should find that with `move_particles` and `move_to_COM_frame`, we can generate almost all configurations with few works. You don't have to calculate the initial positions and velocities of all particles manually in complex geometry.

@note
The circle is used to illustrate the orbit but does not show the trajectory.

@section examples Examples

We show you few additional examples of generating the initial condition with SpaceHub.

@m_class{m-block m-info}

@par Hierarchical Triple
@parblock
  ```cpp
 using namespace space::unit;
 using namespace space::orbit;
 using Particle = typename DefaultSolver::Particle;
 ...
 Particle m0(30_Ms), m1(20_Ms), m2(30_Ms);

 auto inner_orbit = EllipOrbit(m0.mass, m1.mass, 5_AU , 0.01, 0_deg, 0.0, 0.0, isotherm);

 move_particles(inner_orbit, m0);

 move_to_COM_frame(m0, m1);

 auto outer_orbit = EllipOrbit(m2.mass, m0.mass + m1.mass, 100_AU, 0.01, 67.155_deg, 0.0, 0.0, isotherm);

 move_particles(outer_orbit, m2);

 move_to_COM_frame(m0, m1, m2);
 ```

 |                     2D Cartons of generating Hierarchical Triple                      |                                                                              |
 | :-----------------------------------------------------------------------------------: | :--------------------------------------------------------------------------: |
 | @image html tutorial/LK1.png 1. Particle m0(30_Ms), m1(20_Ms), m2(30_Ms); width=400px | @image html tutorial/LK2.png 2. move_particles(inner_orbit, m0); width=400px |
 |        @image html tutorial/LK3.png 3. move_to_COM_frame(m0, m1); width=400px         | @image html tutorial/LK4.png 4. move_particles(outer_orbit, m2); width=400px |
 |      @image html tutorial/LK5.png 5. move_to_COM_frame(m0, m1, m2); width=400px       |                                                                              |

@endparblock 


@m_class{m-block m-info}

@par Binary Binary Scattering
@parblock
  ```cpp
 using namespace space::unit;
 using namespace space::orbit;
 using namespace space::random;
 using namespace space::consts;
 using Particle = typename DefaultSolver::Particle;
 ...
 Particle m0(1_Ms), m1(1_Ms), m2(1_Ms), m3(1_Ms);

 auto binary_orbit0 = EllipOrbit(m0.mass, m1.mass, 1_AU , 0.01, isotherm, isotherm, isotherm, isotherm);

 move_particles(binary_orbit0, m1);

 move_to_COM_frame(m0, m1);

 auto binary_orbit1 = EllipOrbit(m2.mass, m3.mass, 2_AU , 0.3, isotherm, isotherm, isotherm, isotherm);

 move_particles(binary_orbit1, m3);

 auto incident_orbit = HyperOrbit(m0.mass + m1.mass, m2.mass + m3.mass, 5_kms, 20_AU, Uniform(0, 2 * pi), 0, 0, 200_AU, Hyper::in);

 move_particles(incident_orbit, m2, m3);

 move_to_COM_frame(m0, m1, m2, m3);
 ```

  |                        2D Cartons of generating binary-binary scattering                        |                                                                                   |
  | :---------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
  | @image html tutorial/bi-bi1.png 1. Particle m0(1_Ms), m1(1_Ms), m2(1_Ms), m3(1_Ms); width=400px | @image html tutorial/bi-bi2.png 2. move_particles(binary_orbit0, m1); width=400px |
  |            @image html tutorial/bi-bi3.png 3. move_to_COM_frame(m0, m1); width=400px            | @image html tutorial/bi-bi4.png 4. move_particles(binary_orbit1, m3); width=400px |
  |     @image html tutorial/bi-bi5.png 5. move_particles(incident_orbit, m2, m3); width=400px      | @image html tutorial/bi-bi6.png 6. move_to_COM_frame(m0, m1, m2, m3); width=400px |

@endparblock

@m_class{m-block m-info}

@par Quadrupole around SMBH
@parblock
  ```cpp
 using namespace space::unit;
 using namespace space::orbit;
 using Particle = typename DefaultSolver::Particle;
 ...
 Particle m0(1_Ms), m1(1_Ms), m2(1_Ms), m3(1_Ms), M_bh(1e6_Ms);

 auto bi_orbit0 = EllipOrbit(m0.mass, m1.mass, 1_AU , 0.01, isotherm, isotherm, isotherm, isotherm);

 move_particles(bi_orbit0, m1);

 move_to_COM_frame(m0, m1);

 auto bi_orbit1 = EllipOrbit(m2.mass, m3.mass, 1_AU , 0.01, isotherm, isotherm, isotherm, isotherm);

 move_particles(bi_orbit1, m3);

 auto quad_orbit = EllipOrbit(m0.mass + m1.mass, m2.mass + m3.mass, 5_AU , 0.2, isotherm, isotherm, isotherm, isotherm);

 move_particles(quad_orbit, m2, m3);

 auto bh_orbit = EllipOrbit(M_bh.mass, M_tot(m0, m1, m2, m3), 50_AU , 0.1, isotherm, isotherm, isotherm, isotherm);

 move_particles(bh_orbit, m0, m1, m2, m3);

 move_to_COM_frame(M_bh, m0, m1, m2, m3);
 ```

  |                               2D Cartons of generating Quadrupole around SMBH                                |                                                                                         |
  | :----------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------: |
  | @image html tutorial/quad1.png 1. Particle m0(1_Ms), m1(1_Ms), m2(1_Ms), m3(1_Ms), M_bh(1e6_Ms); width=400px |      @image html tutorial/quad2.png 2. move_particles(bi_orbit0, m1); width=400px       |
  |                   @image html tutorial/quad3.png 3. move_to_COM_frame(m0, m1); width=400px                   |      @image html tutorial/quad4.png 4. move_particles(bi_orbit1, m3); width=400px       |
  |              @image html tutorial/quad5.png 5. move_particles(quad_orbit, m2, m3); width=400px               | @image html tutorial/quad6.png 6. move_particles(bh_orbit, m0, m1, m2, m3); width=400px |
  |            @image html tutorial/quad7.png 7. move_to_COM_frame(M_bh, m0, m1, m2, m3); width=400px            |                                                                                         |
@endparblock

@m_class{m-block m-info}

@par Asteroid belt
@parblock
  ```cpp
 #include<array>
 ... 
 using namespace space::unit;
 using namespace space::orbit;
 using namespace space::random;
 using Particle = typename DefaultSolver::Particle;
 ...
 Particle sun(1_Ms), earth(1_Me);

 auto earth_orbit = EllipOrbit(sun.mass, earth.mass, 1_AU , 0.01, 0_deg, 0_deg, 0_deg, isotherm);

 move_particles(earth_orbit, earth);

 move_to_COM_frame(sun, earth);
 //Default mass, radius = 0;
 std::array<Particle, 1000> asteroids;

 for(auto & a : asteroids) {
   auto bell_orbit = EllipOrbit(sun.mass + earth.mass, 0, Uniform(2.2_AU, 3.3_AU) , Uniform(0,0.2), Uniform(-5_deg, 5_deg), isotherm, isotherm, isotherm);
   move_particles(bell_orbit, a);
 }
 ```
 @image html tutorial/asteroid.png Asteroid Belt width=500px 
@endparblock


@m_class{m-note m-dim m-text-center}

@parblock
  <a href="create_kepler.html"> << Create Kepler orbit </a> | <a href="scattering.html"> Scattering Experiments >> </a> 
@endparblock