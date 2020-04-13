[![Build Status](https://travis-ci.org/travissluka/UMD-LETKF.svg?branch=develop)](https://travis-ci.org/travissluka/UMD-LETKF)
[![codecov](https://codecov.io/gh/travissluka/UMD-LETKF/branch/develop/graph/badge.svg)](https://codecov.io/gh/travissluka/UMD-LETKF)
[![docs](https://readthedocs.org/projects/umd-letkf/badge/?version=latest)](http://umd-letkf.readthedocs.io)

For complete set of documentation, visit [umd-letkf.readthedocs.io](http://umd-letkf.readthedocs.io/).

----------

Brief Description
----------
The following library is a rewrite of the local ensemble transform Kalman filter (LETKF) originally developed by Hunt et al., 2007 [1], coded by Takemasa Miyoshi [2], with additional modifications for the ocean by Steve Penny [3]. 

It is built with the following design choices in mind:

* **model agnostic library** - A single generic LETKF library is provided that can be compiled once and then used in all domains of a coupled LETKF system. Redundancies in code are elimited this way. Most specialization for a given domain are done through configuration files, and a generic driver is provided that should handle most use cases. A custom driver can easily be built to interface with the library if model specific code needs to be added.
* **object oriented design** - Several default implmentations of classes for observation I/O, model state I/O, and localization are provided. If different functionality is required, the user can create their own derived classes and register them with the LETKF library.
* **multi-model strong coupling** - By being model agnostic, the code should allow for easy transition from weakly coupled to strongly coupled DA. The same LETKF code can be used for multiple independent executables (one for each domain), and cross-domain observations can be assimilated by selecting the appropriate observation I/O and localization classes.

[1]: Hunt, B. R., Kostelich, E. J., & Szunyogh, I. (2007). Efficient data assimilation for spatiotemporal chaos: A local ensemble transform Kalman filter. Physica D: Nonlinear Phenomena, 230(1-2), 112–126. [http://doi.org/10.1016/j.physd.2006.11.008](http://doi.org/10.1016/j.physd.2006.11.008)

[2]: Miyoshi, T. (2005). Ensemble Kalman Filter Experiments with a Primitive-Equation Global Model. University of Maryland. Retrieved from [http://hdl.handle.net/1903/3046](http://hdl.handle.net/1903/3046)

How to Build the LETKF Library on Hera
----------
## Clone LETKF
0. Set the CLONE_DIR: The directory where the system is cloned, user defined path    
   `set CLONE_DIR=USER/DEFINED/PATH` or `export ...` 
   `set MACHINE_ID=hera` or `export MACHINE_ID=orion`
1. `git clone --recursive https://github.com/NOAA-EMC/UMD-LETKF.git $CLONE_DIR/letkf`
2. `cd letkf`
3. `git submodule update --init --recursive` 

## Compile the code
0. `cd $CLONE_DIR/letkf`
1. Setup the environment at the HPC that you work on. 
This process is automated for hera and orion using the default modules:   
   `source config/env.${MACHINE_ID}`
2. `mkdir -p [...]/letkf/build`
3. Building path TBD: `cd [...]/build`

4. Run the cmake:
   `cmake -DNETCDF_DIR=$NETCDF  [...]/letkf`
5. `make -j<n>`


## Transfering the LETKF to other HPC or using different compiler
At minimum the following software is required (see the documentation of LETKF for additional options):

1. intel
2. impi
3. netcdf
4. cmake/3.9.0
5. Set the following variables:    
`setenv FC mpiifort`    
`setenv CC mpiicc`     
`setenv CXX mpiicpc` 
