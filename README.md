# Building and running the SWMM5 engine from the command line in Ubuntu

## Overview
 - This repository can be used to build a standalone command line executable for SWMM5 on a fresh install of Ubuntu. 
 - It has been tested on __Ubuntu Desktop 18.04, 64-bit__. 
 - This does __not__ include a build of the SWMM graphical user interface, it is __command line only__. 

## Submodules
This repository contains two submodules:  
  - [USEPA](https://github.com/USEPA)'s [Stormwater-Management-Model](https://github.com/USEPA/Stormwater-Management-Model) repository, which contains the source code for EPA SWMM5. This submodule is currently targeting the v5.1.13 release. 
  - [dleutnant](https://github.com/dleutnant)'s [swmmr](https://github.com/dleutnant/swmmr) repository, which is a library for interfacing with SWMM5 in R. This is included to provide example input files to test your swmm5 executable, none of its source files are used in building the executable itself. This submodule is curently targeting the v0.8.1 release.
  
## Acknowledgements
The Makefile in this repository is based on the [dleutnant](https://github.com/dleutnant)'s [gist](https://gist.github.com/dleutnant/872ee59a2140bd068bc84a756a5ce223) which modifies the Makefile distributed in the USEPA's packaged [SWMM5 5.1.13 distribution](https://www.epa.gov/sites/production/files/2018-08/swmm51013_engine_0.zip), available from their [official website](https://www.epa.gov/water-research/storm-water-management-model-swmm). 

## Building on a fresh Ubuntu Desktop 18.04 build

### Install dependencies 
- Open the Terminal app. (ctrl-alt-t)
- Enter the following: 
```
sudo apt update
sudo apt install git make gcc
```

### Clone repository and initialize submodules 
After navigating to the desired top-level directory to hold the swmm5-ubuntu-cli repository, enter the following into the terminal: 
```
git clone --recursive https://github.com/Kevin-M-Smith/swmm5-ubuntu-cli
```

### Confirm submodules are targeting correct verisons
Enter the following the terminal: 
```
cd swmm5-ubuntu-cli
git submodule
```
__Expected Results__

The following output should appear in the terminal. 
```
 0ab0021f9c501ac69cfa1dcc175a17e793bc22e6 Stormwater-Management-Model (v5.1.12-2-g0ab0021)
 9278214ad86c1d113d55c42bf22c0e6ab5ed2d2d swmmr (v0.8.1)
```
You can verify that these are the desired commits: 
- [0ab0021f9c501ac69cfa1dcc175a17e793bc22e6](https://github.com/USEPA/Stormwater-Management-Model/commit/0ab0021f9c501ac69cfa1dcc175a17e793bc22e6)
- [9278214ad86c1d113d55c42bf22c0e6ab5ed2d2d](https://github.com/dleutnant/swmmr/commit/9278214ad86c1d113d55c42bf22c0e6ab5ed2d2d
)

### Build
Enter the following in the terminal to execute the Makefile in the current directory against the source files in the Storm-Water-Management-Model submodule: 
```
(cd Stormwater-Management-Model/src && make -f ../../Makefile)
```

### Confirm swmm5 build version
Enter the following in the terminal to check the swmm5 build version. 
```
./swmm5 --version
```

### Confirm syntax for running a simulation
Enter the following in the terminal to get help on running swmm5: 
```
./swmm5 --help
```
As you can see the format to run a simulation is: 
```
swmm5 <input file> <report file> <output file>
```

### Run an example simulation to test the SWMM5 engine. 
Enter the following in the terminal to test the SWMM5 engine:
```
./swmm5 swmmr/inst/extdata/Example1.inp Example1.rpt Example1.out
```

### Check the output of the report file
Enter the following to grab a table from the report file for comparison with the expected output below:  
```
tail Example1.rpt -n 21 | head -n 16
```
__Expected Results__
```
  ------------------------------------------------
                                 TSS          Lead
  Link                           lbs           lbs
  ------------------------------------------------
  1                           50.051         0.010
  4                           25.988         0.005
  5                           25.974         0.005
  6                          119.061         0.024
  7                          145.020         0.029
  8                          191.112         0.038
  10                         353.347         0.071
  11                          25.126         0.005
  12                          25.110         0.005
  13                          96.258         0.019
  14                          65.743         0.013
  15                         287.482         0.057
  16                         353.239         0.071
```

### Using the SWMM5 executable
The SWMM5 executable is a stanadlone executable. Once built, you can move the swmm5 executable to wherever you'd like to use it. After moving the executable out of the directory containing the git repository, the contents of the directory can be removed from your machine. 
