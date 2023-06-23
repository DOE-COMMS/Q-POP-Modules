# Environment setup

## Install FEniCS C++ version

### GNU 9
It is necessary to use gnu compiler v9, if you are on system that defaults to another version of gnu compiler, then you need to switch to gnu-9
```sh
sudo apt install gcc-9 g++-9 gfortran-9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 90
sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-9 90
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
sudo update-alternatives --config gfortran
```

### Boost 1.71
In newer version of Boost, you will meet this error
```sh
dolfin/io/VTKWriter.cpp:27:10: fatal error: boost/detail/endian.hpp: No such file or directory
   27 | #include <boost/detail/endian.hpp>
      |          ^~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
```

We found boost 1.71 works fine with FEniCS 2019. If you are using Ubuntu 20.04, then you can `sudo apt install libboost-all-dev` to install boost 1.71. If you are on newer version of Ubuntu, then you will have to build boost from source.
```sh
wget https://boostorg.jfrog.io/artifactory/main/release/1.71.0/source/boost_1_71_0.tar.gz
tar -xzf boost_1_71_0.tar.gz
cd boost_1_71_0/
./bootstrap.sh --prefix=$HOME/boost
./b2 install
```

### Others
```sh
sudo apt install git libeigen3-dev python3-pip cmake 
pip3 install fenics-ffc --upgrade
echo 'export PATH=$PATH:$HOME/.local/bin' >> ~/.bashrc
git clone --branch=2019.1.0.post0 https://bitbucket.org/fenics-project/dolfin.git
cd dolfin
git describe --tags --abbrev=0
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=$HOME/fenics/dolfin -DBOOST_ROOT=$HOME/boost ..
make
make install
cd ../..

git clone --branch=2019.1.0.post0 https://bitbucket.org/fenics-project/mshr
cd mshr

```