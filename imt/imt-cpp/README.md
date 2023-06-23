# IMT C++ program

##　Build and run the program

```sh
ffc -l dolfin VO2_2tdevice.ufl
mkdir build
cd build
cmake -DDOLFIN_DIR=$HOME/fenics/dolfin/share/dolfin/cmake -DBOOST_ROOT=$HOME/boost ..
make
```
## Customize the program