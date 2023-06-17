# Q-POP modules

The main modules of Q-POP. We plan to gradually open source three main modules in the next few years (this year is 2023). This is still an ongoing effort, and the first one to be released is the insulator-metal transition module, while superconductor module and the dynamic phase-field module coming later.

## Setup development environment
FEniCS C++ package must be installed.

## Folder structure
```sh
├── core
├── external
│   └── pugixml
│       ├── pugiconfig.hpp
│       ├── pugixml.cpp
│       └── pugixml.hpp
└── imt
    ├── imt-cpp
    │   ├── input.h
    │   ├── main.cpp
    │   └── VO2_2tdevice.ufl
    └── imt-py
        ├── VO2_GSBlockATP.py
        └── VO2_Nit_EfficAdap_v4.py
```

## How to build the program
The program uses cmake to build the executable. Simply create a build directory in the root directory of the package, and go to that build directory, and then enter in the command line: 

```
cmake ..
make
```
