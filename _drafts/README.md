# MATH20014: Group Projects

## Planetary Motion

This is a notebook for the group project planetary motion.

### Authors

* Yu Liang
* Tom Pagulatos
* Qinghong Shen
* Ben Wills



### Modules Requirement

numpy, matplotlib, datetime, astroquery



### Document
* ``config.py`` <br>
a py file including the initialization of date, staring location and other initial conditions
* ``class Object``  <br>
a class to store the physical information for each object $i$
* ``class SolarSystem`` <br>
a class to simulate the solar system and the number of planets can be modified in the config.py

### Assumptions and Preprocess

#### Assunption

Since the mass of sun is much larger than other planets and the sun is always in the center of the solar system , we assume that the sun is fixed and that only planets move in all subproblems.


#### Units

Before we start this project, we note that if the units of mass, distance and time are considered as **kg**,  **m** and **s**, then the magnitude will be too large or too small for computing and may cause a computation error in long time simulation. Thus, we first transfer the unit as following:

physical quantity |original unit        |  current unit                    | ratio
:------                    | :------                 | :------                               | :------
mass                    | kg                     | solar mass  (SM)              |  $ 1.98847e30 $
length                   | m                      | astronomical unit (AU)    |   $ 149597870700 $
time                      | s                       | day (D)                             |  $ 86400 $
velocity                | m/s                   | AU/D                               |   $ 1.7314568e6 $
acceleration        | m/s^2                | AU/D^2                          |   $ 20.0400097 $
force                   | kg m/s^2           | SM AU/D^2                    |  $ 3.9848958e31 $
energy                 | kg (m/s)^2         | SM (AU/D)^2                 |  $ 5.96131927e42 $

Accoringly, we can rewrite the **universal gravitational constant**  as

$$ G = 6.6734810\times10^{-11}\frac{m^3}{kgs^2}  = 2.9588484421\times10^{-4} \frac{AU^3}{SM D^2}$$

### Problem 1
In this problem, we only have one planet  **earth** in the system with initial condition that

### Reference

1. [PL HORIZONS on-line solar system database][1] <br>
2. [Jet Propulsion Laboratory][2]

[1]: https://docs.astropy.org/en/stable/coordinates/solarsystem.html
[2]: https://ssd.jpl.nasa.gov/?constants

