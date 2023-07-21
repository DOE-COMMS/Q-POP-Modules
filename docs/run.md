# Build and run

## Build

For Ubuntu 22.04
```sh
source $HOME/fenics/dolfin/share/dolfin/dolfin.conf
cmake -DBOOST_ROOT=$HOME/boost -B build -S .
cmake --build build
```

For Ubuntu 20.04

```sh
source $HOME/fenics/dolfin/share/dolfin/dolfin.conf
cmake -B build -S .
cmake --build build
```

Or if you don't want to source the dolfin.conf file, you can run cmake configuration like this
```sh
cmake -DDOLFIN_DIR=$HOME/fenics/dolfin/share/dolfin/cmake -B build -S .
```

Then build as normal, `cmake --build build`

## Run

To run the simulation, 
1. You can cd into the `build/imt/imt-cpp` folder, copy a input file to here, and run the `qpop-imt` executable directly from the build directory.
2. Or you can add the path of `build/imt/imt-cpp` folder to the PATH environment variable, and run the `qpop-imt` command from any folder with an input file.
