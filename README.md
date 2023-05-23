# Q-POP modules

The main modules of Q-POP. We plan to gradually open source three main modules in the next few years (this year is 2023). This is still an ongoing effort, and the first one to be released is the insulator-metal transition module, while superconductor module and the dynamic phase-field module coming later.

## Setup development environment
FEniCS C++ package must be installed.

## Folder structure
- core
- external
|- pugixml
 |- pugiconfig.hpp
 |- pugixml.cpp
 |- pugixml.hpp
- imt
|- imt-cpp
 |- main.cpp
 |- input.h
 |- VO2_2tdevice.cpp
 |- VO2_2tdevice.hpp
 |- VO2_2tdevice.ufl
|- imt-py
 |- VO2_2tdevice.py

## How to build the program
