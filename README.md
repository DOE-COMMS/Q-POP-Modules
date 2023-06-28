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

## Input file
The program is currently solving for a rectangular VO2 device, supplied with a direct voltage through a series resistor. The input file is written in xml form. Below is an example of the input file.
```
<?xml version="1.0"?>
<input>
 <external>
  <temperature unit='K'>300.1</temperature>
  <voltage unit='V'>5.6</voltage>
  <resistor unit='Ohm'>2.5e5</resistor>
  <capacitor unit='nF'>0.0</capacitor>
  <heatdiss unit='W/m^2K'>3e6</heatdiss>
  <Lx unit='nm' mesh='100'>100.0</Lx>
  <Ly unit='nm' mesh='40'>40.0</Ly>
  <Lz unit='nm'>20.0</Lz>
 </external>
 <time>
  <endtime unit='ns'>2e3</endtime>
  <savemethod>auto</savemethod>
  <saveperiod>20</saveperiod>
 </time>
 <initialization>
  <temperature unit='K'>300.0</temperature>
  <SOP>1.119</SOP>
  <EOP>-1.293</EOP>
  <Tcvariance method='nucleus1'>
   <Tcshift unit='K'>-5.0</Tcshift>
   <radius unit='nm'>3.0</radius>
  </Tcvariance>
 </initialization>
 <solverparameters>
  <Newtonabsolutetolerance>1e-5</Newtonabsolutetolerance>
  <Newtonrelativetolerance>1e-3</Newtonrelativetolerance>
  <Newtonmaxiteration>15</Newtonmaxiteration>
  <timesteptolerance>1e-2</timesteptolerance>
  <directsolver>pastix</directsolver>
  <loglevel>INFO</loglevel>
 </solverparameters>
</input>
```
Almost all the parameters are self-explanatory. The `external` section defines external parameters: 
Name          | Explanation
------------- | -------------------
`temperature` | Ambient temperature
`voltage`     | Direct voltage applied
`resistor`    | Resistance of the series resistor
`capacitor`   | The capacitance representing the parasitic capacitance or the external capacitor parallelly connected with the series resistor
`heatdiss`    | Heat transfer coefficient from the device to the environment
`Lx`          | Width of the device
`Ly`          | Length of the device, along the electric field
`Lz`          | Thickness of the device