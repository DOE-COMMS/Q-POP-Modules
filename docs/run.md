# Build and run

## Build

```sh
cmake -DDOLFIN_DIR=$HOME/fenics/dolfin/share/dolfin/cmake -DBOOST_ROOT=$HOME/boost -B build -S .
cmake --build build
```