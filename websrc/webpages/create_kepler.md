
@page create_kepler Generate Kepler orbit

SpaceHub provides a bunch of user friendly functions to generate the initial conditions you want. 

@tableofcontents

@section Kepler Create a Kepler orbit.

To get started, we first introduce how to create Kepler orbit in SpaceHub. SpaceHub use the type `space::orbit::Kepler` to holds the orbital parameters. `space::orbit::Kepler` has 9 public data members, 

- @bflabel{m1} The mass of the primary object in Kepler orbit. (>=0)
- @bflabel{m2} The mass of the secondary object in Kepler orbit. (>=0)
- @bflabel{p} The semi-latus rectum @f$ a(1-e^2) @f$. (>0)
- @bflabel{e} The eccentricity. (>=0)
- @bflabel{i} Orbit inclination. ([0, pi])
- @bflabel{Omega} Longitude of the ascending node. ([-pi,pi])
- @bflabel{omega} Argument of periapsis. ([-pi,pi])
- @bflabel{nu} True anomaly. ([-pi,pi])
- @bflabel{orbit_type} The type of the orbit. (@bflabel{Ellipse}, @bflabel{Parabola}, @bflabel{Hyperbola})
  
 @image html tutorial/Orbit1.png "credit : wikipedia" width=400px

 To create a kepler orbit with @bflabel{m1} = 1 solar mass, @bflabel{m2} = 2 solar mass, @bflabel{p} = 1 AU, @bflabel{e} = 0.4, @bflabel{i} = 10 deg, @bflabel{Omega} = 4.2 deg, @bflabel{omega} = 46.4 deg and @bflabel{nv} = 90.1 deg.

 ```cpp
 ...
 using namespace space::unit;//import the unit system of spaceHub
 using namespace space::orbit;//import orbit where Kepler located. 
 ...
 Kepler an_orbit = Kepler(1_Ms, 2_Ms, 1_AU, 0.4, 10_deg, 4.2_deg, 46.4_deg, 90.1_deg);
 ...
 ```

 Here we use the unit system of spaceHub. You can use `_Ms`, `_AU`, `_deg` etc directly after constant number(literal) to scale it with unit. You can also use the unit in this way. 

 ```cpp
 double p = 1 * AU;
 double inclination = 4.52 * deg;
 ```

The `space::orbit::Kepler` can hold elliptic orbit, parabolic and hyperbolic orbit. This is also why we use semi-latus rectum @bflabel{p} instead of semi-major axis @bflabel{a} that is undefined for parabolic orbit. The following table
gives the orbital parameters for Kepler orbit.

 |                                          Orbit type |                        Elliptic                         |                Parabolic                | Hyperbolic                                                 |
 | --------------------------------------------------: | :-----------------------------------------------------: | :-------------------------------------: | :--------------------------------------------------------- |
 |                              Eccentricity @f$ e @f$ |                   @f$ 0 \leq e <1 @f$                   |              @f$ e = 1 @f$              | @f$ e > 1 @f$                                              |
 |                            Semi-major axis @f$a @f$ |                       @f$ a>0 @f$                       |                undefined                | @f$ a < 0 @f$                                              |
 |                                 Periapsis @f$ q @f$ |                    @f$ q=a(1-e) @f$                     |       @f$q@f$ defining parameter        | @f$ q=a(1-e) @f$                                           |
 |                          Semi-latus rectum @f$p @f$ |                   @f$ p=a(1-e^2) @f$                    |              @f$ p=2q @f$               | @f$ p=a(1-e^2) @f$                                         |
 |                              Total Energy @f$ E @f$ |                 @f$ E=-\frac{u}{2a} @f$                 |               @f$ E=0 @f$               | @f$ E=-\frac{u}{2a} @f$                                    |
 |                                  Distance @f$ r @f$ |           @f$ r=\frac{p}{1+e\cos(\theta)}@f$            |  @f$  r=\frac{p}{1+e\cos(\theta)} @f$   | @f$  r=\frac{p}{1+e\cos(\theta)}@f$                        |
 |                                  Velocity @f$ v @f$ |      @f$ v=\sqrt{u(\frac{2}{r} - \frac{1}{a})}@f$       |     @f$  v=\sqrt{\frac{2u}{r}} @f$      | @f$  v=\sqrt{u(\frac{2}{r} - \frac{1}{a})}@f$              |
 | Velocity, @f$ v @f$ \n @f$ \theta@f$ : true anomaly |       @f$ v=\frac{u(1+e^2+2e\cos(\theta))}{p}@f$        | @f$  v=\frac{2u(1+\cos(\theta))}{p} @f$ | @f$  v=\frac{u(1+e^2+2e\cos(\theta))}{p}@f$                |
 |                        Eccentric anomaly  @f$ E @f$ | @f$ \cos(E)= \frac{e+\cos(\theta)}{1+e\cos(\theta)} @f$ |   @f$  E = \tan(\frac{\theta}{2}) @f$   | @f$  \cosh(E)= \frac{e+\cos(\theta)}{1+e\cos(\theta)}  @f$ |
 |                              Mean anomaly @f$ M @f$ |                 @f$ M = E-e\sin(E) @f$                  |          @f$  M = E+E^3/3 @f$           | @f$  M = e\sinh(E) - E @f$                                 |
 |                       Time to periapsis @f$t-t_0@f$ |          @f$ t-t_0 = M\sqrt{\frac{a^3}{u}} @f$          | @f$  t-t_0 = M\sqrt{\frac{2q^3}{u}} @f$ | @f$  t-t_0 = M\sqrt{\frac{a^3}{u}} @f$                     |

Take it easy. SpaceHub also provides you `space::orbit::EllipOrbit` and `space::orbit::HyperOrbit`. If you know a little about Object Oriented Programming, they are the derived class of `space::orbit::Kepler`. 
Besides the 9 data member as in `space::orbit::Kepler`,  `space::orbit::EllipOrbit` has an additional data member @bflabel{a} which is the semi-major axis, while `space::orbit::HyperOrbit` has an additional data member @bflabel{b} which is the impact parameter.

You can create elliptic orbit in this way.

```cpp
 ...
 using namespace space::unit;
 using namespace space::orbit;
 ...
 //now, the third parameter is semi-major axis a instead of semi-latus rectum p.
 EllipOrbit an_orbit = EllipOrbit(1_Ms, 2_Ms, 1_AU, 0.4, 10_deg, 4.2_deg, 46.4_deg, 90.1_deg);
 ...
 ```

The way to create a hyperbolic orbit is kind of different, if you are familiar with celestial dynamic, @bflabel{a} is hardly used to describe a hyperbolic orbit. Instead, we usually use @bflabel{v_inf} and @bflabel{b}, which is velocity at infinity and impact parameter, respectively.  So you can create an hyperbolic orbit in this way.

```cpp
 ...
 using namespace space::unit;
 using namespace space::orbit;
 ...
 //the 3rd parameter is velocity at infinity.
 //the 4th parameter is impact parameter.
 //the 5th-7th parameters:i, Omega, omega.
 //the 8th parameter is the distance between two object.(you cannot set particles at infinity in program)
 //the 9th parameter ; indicate this is a incident in orbit or an eject out orbit (the same 8th parameter
 //gives two different orbits, so you need to specify one of it here.)- Hyper::in or Hyper::out.
 HyperOrbit an_orbit = HyperOrbit(1_Ms, 2_Ms, 5_kms, 4_AU, 10_deg, 4.2_deg, 46.4_deg, 100_AU, Hyper::in);
 ...
```

@section random_orbit Create random phase Kepler orbit
It's frequently needed to generate random phase orbit to perform Monte Carlo simulations. SpaceHub provides a place holder `space::orbit::isotherm` to indicate a phase parameter will be generated isothermally.
This place holder can be placed to any phase orbital parameters like @bflabel{i}, @bflabel{Omega}, @bflabel{omega} and @bflabel{nu}.

If phase parameters are placed,

- @bflabel{i} : cos(i) is uniformly distributed in [-1,1].
- @bflabel{Omega} : Omega is uniformly distributed in [-pi, pi].
- @bflabel{omega} : omega is uniformly distributed in [-pi, pi].
- @bflabel{nv} : corresponding nv with @rlabel{M}(mean anomaly) is uniformly distributed in [-pi, pi]. Check the orbital parameters table above.

The way to use this place holder.

```cpp
 ...
 using namespace space::unit;
 using namespace space::orbit;
 ...
 //randomly generate all phase parameters.
 EllipOrbit orbit1 = EllipOrbit(1_Ms, 2_Ms, 1_AU, 0.4, isotherm, isotherm, isotherm, isotherm);
 ...
 //randomly generate i (the usual way to generate incident orbit in cross section calculations).
 HyperOrbit orbit2 = HyperOrbit(1_Ms, 2_Ms, 5_kms, 4_AU, isotherm, 0_deg, 0_deg, 100_AU, Hyper::in);
 ...
```

You can also manually random the phase parameter of an existed orbit by calling the its methods `suffle_i()`, `suffle_Omega()`, `suffle_omega()` and `suffle_nu()`.

```cpp
 ...
 using namespace space::unit;
 using namespace space::orbit;
 ...
 //generate an elliptic orbit.
 EllipOrbit orbit = EllipOrbit(1_Ms, 2_Ms, 1_AU, 0.4, 0_deg, 0_deg, 0_deg, 0_deg);

 orbit.suffle_omega();//after this, omega is a random number between -pi and pi.  
 ...
 ```

You can also use the random number generators provided by SpaceHub to randomlize some parameters. We will introduce the random number generators carefully in details in another topic, but we can use it a bit here.

```cpp
 ...
 using namespace space::unit;
 using namespace space::orbit;
 using namespace space::random;//import the random generators
 ...
 //generate an elliptic orbit with Omega uniformly distributed in [0, 45] deg.
 EllipOrbit orbit = EllipOrbit(1_Ms, 2_Ms, 1_AU, 0.4, 0_deg, Uniform(0_deg, 45_deg), 0_deg, 0_deg);
 ...
```

@section anomaly Anomaly Calculation

SpaceHub provides anomaly calculations between True anomaly @bflabel{T}, Mean anomaly @bflabel{M} and Eccentric anomaly @bflabel{E}.

|                                                     Eccentric anomaly |                              Mean anomaly                              |
| --------------------------------------------------------------------: | :--------------------------------------------------------------------: |
| @image html tutorial/ecc_anomaly.png "credit : wikipedia" width=400px | @image html tutorial/mean_anomaly.png "credit : wikipedia" width=400px |

- Scalar space::orbit::M_anomaly_to_E_anomaly(Scalar M_anomaly, Scalar e);
- Scalar space::orbit::E_anomaly_to_T_anomaly(Scalar E_anomaly, Scalar e);
- Scalar space::orbit::M_anomaly_to_T_anomaly(Scalar M_anomaly, Scalar e);
- Scalar space::orbit::T_anomaly_to_E_anomaly(Scalar T_anomaly, Scalar e);
- Scalar space::orbit::E_anomaly_to_M_anomaly(Scalar E_anomaly, Scalar e);
- Scalar space::orbit::T_anomaly_to_M_anomaly(Scalar T_anomaly, Scalar e);

The four functions accept floating point number with any precision like @bflabel{float}, @bflabel{double}... `e` is eccentricity. The equations are listed in the table above.

```cpp
 ...
 using namespace space::orbit;
 using namespace space::consts;//import constant numbers pi
 ...
 double mean_anomaly = 0.3 * pi;
 double eccentricity = 0.6;

 double e_anomaly = M_anomaly_to_E_anomaly(mean_anomaly, eccentricity);
 ...
```

@section coord_gen  Position/Velocity generation

The initial conditions for N-body system are basically a group of position and velocity with corresponding particle properties like mass and etc.
SpaceHub provides two functions to transfer a Kepler orbit to corresponding position and velocity and versus. The definition of the position and velocity here is the relative
position and velocity between two components in a Kepler orbit.

- space::orbit::orbit_to_coord;
- space::orbit::coord_to_orbit;
  
Just to use it in this way

```cpp
...
 using namespace space::unit;
 using namespace space::orbit;
 ...
 EllipOrbit orbit = EllipOrbit(1_Ms, 1_Me, 1_AU, 0, 0_deg, 0_deg, 0_deg, 0_deg);

 auto [pos, vel] = orbit_to_coord(orbit);
 ...
```

The function `space::orbit::orbit_to_coord` returns two 3D-vectors(x,y,z), the first is the position and the second is the velocity. You can also do it reversely,

```cpp
...
 using namespace space::unit;
 using namespace space::orbit;
 ...
 double m1 = 1_Ms;
 double m2 = 0.1_Ms;

 Vector pos(1_AU, 0, 0);
 Vector vel(0, 30_kms, 0);
 
 auto orbit = coord_to_orbit(m1, m2, pos, vel);
 ...
```

The type `Vector` can be any type once it has three public member `x`, `y`, `z` with type of floating point number. We will introduce the type system of SpaceHub later. Indeed the two functions are hardly used because SpaceHub provides you easier way to generate the initial conditions. We will introduce it in next section.

@m_class{m-note m-dim m-text-center}

@parblock
  <a href="tutorial.html"> Tutorial </a> | <a href="particle_manip.html"> Particle manipulation >> </a>
@endparblock


