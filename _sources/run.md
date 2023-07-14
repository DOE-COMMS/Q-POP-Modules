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