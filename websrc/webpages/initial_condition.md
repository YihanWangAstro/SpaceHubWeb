
@page initial_condition Initial condition generation

SpaceHub provides a bunch of user friendly functions to generate the initial conditions you want. 

@tableofcontents

@section Kepler Create a Kepler orbit.

To get started, we first introduce how to create Kepler orbit in SpaceHub. SpaceHub use the type `space::orbit::Kepler` to holds the orbital parameters. `space::orbit::Kepler` has 9 public data members, 

- m1 
- m2
- p
- e
- i
- Omega
- omega
- nu 
- orbit_type 

