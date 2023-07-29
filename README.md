# Q-POP modules

The main modules of Q-POP. We plan to gradually open source three main modules in the next few years (this year is 2023). This is still an ongoing effort, and the first one to be released is the insulator-metal transition module, while the superconductor module and the dynamic phase-field module are coming later.

## Setup development environment
This program uses FEniCS C++ library for defining and solving finite-element partial differential equations. FEniCS C++ library of version 2019.1.0.post0 must be installed.

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

## How to build and run the program
The program uses cmake to build the executable. Simply create a build directory in the root directory of the package, and go to that build directory, and then enter in the command line: 
```
cmake ..
make
```
The program supports parallel computing. To run the compiled executable on, say, 8 processors, run the below command in your desired directory:
```
mpirun -np 8 directory-to-executable/VO2_2tdevice
```
The program do require an input file for specifying parameters; see next section. The output files will be generated in the current directory.

## Input file
The program is currently solving for a rectangular VO<sub>2</sub> device, supplied with a direct voltage through a series resistor. The input file is written in `xml` form and must be named as `input.xml`. Below is an example of the input file.
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
Almost all the parameters are self-explanatory. The units are fixed and just for reminding the user of the corresponding parameter's unit. The `external` section defines external parameters: 
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
`mesh`        | Number of mesh vertices along a given dimension

The `time` section defines the parameters related to the time:
Name         | Explanation
------------ | -----------
`endtime`    | Simulation end time; beginning time is zero
`savemethod` | Method of saving time-dependent solutions, used with `saveperiod` parameter. `auto` means to save every `saveperiod`-step solution; `fixed` means to save every `saveperiod`-nanosecond solution
`saveperiod` | See `savemethod`

The `initialization` section defines parameters for initialization:
Name          | Explanation
------------- | -----------
`temperature` | Initial temperature of the device
`SOP`         | Initial value for the structural order parameter
`EOP`         | Initial value for the electronic order parameter
`Tcvariance`  | `method`: how to set up a nucleus of the high-temperature phase. `nucleus1` means to set up a semicircle with a radius of `radius` and a transition temperature shift of `Tcshift`, located at the midpoint of the $y = 0$ edge

The `solverparameters` section defines the parameters for both the Newton's iteration solver (for nonlinear differential equations) and the linear solver (for solving linear equations in each iteration of the Newton's method):
Name                      | Explanation
------------------------- | -----------
`Newtonabsolutetolerance` | Absolute tolerance for the Newton iteration
`Newtonrelativetolerance` | Relative tolerance for the Newton iteration. Meeting either the absolute tolerance or the relative tolerance is considered converged
`Newtonmaxiteration`      | Limit of the iteration times for the Newton's method
`timesteptolerance`       | Relative tolerance for the adaptive time stepping error
`directsolver`            | Which direct solver to use for solving the linear problem
`loglevel`                | Log level; see [FEniCS manual](https://fenics.readthedocs.io/projects/dolfin/en/2017.2.0/apis/api_log.html "FEniCS log level")

The example shown above simulates the intrinsic voltage self-oscillation in VO<sub>2</sub> thin films, which was published in Physical Review Applied; see [Y. Shi and L.-Q. Chen, 2022](https://doi.org/10.1103/PhysRevApplied.17.014042 "Intrinsic voltage self-oscillation"). It will take about 2 hours on 16 processors of AMD EPYC 7742 CPU.

## Visualization of solutions
The program generates solutions in pvd format that can be read and plotted by [ParaView](https://www.paraview.org "ParaView website"). The solution files are:
Name       | Explanation
---------- | -----------
`eta.pvd`  | Time-dependent structural order parameter field
`psi.pvd`  | Time-dependent electronic order parameter field
`phi.pvd`  | Time-dependent electric potential
`T.pvd`    | Time-dependent temperature field
`n.pvd`    | Time-dependent electron density field
`p.pvd`    | Time-dependent hole density field
`niov.pvd` | Time-dependent ionized oxygen vacancy concentration field
`nnov.pvd` | Time-dependent neutral oxygen vacancy concentration field

Each solution file listed above links to data at different moments. The data at a moment in turn links to data parallelly computed on distributed processors. The user can just use ParaView to read the `.pvd` files and then follow the guide of [ParaView](https://docs.paraview.org/en/latest/UsersGuide/index.html "ParaView user's guide") to plot spatiotemporal fields.

The program also generates and updates on the fly a log.txt file that summarizes the temporal evolution of several physical quantities and diagnostics. The columns are summarized in the table below.
Column name  | Explanation
------------ | -----------
`#Step`      | Number of time steps
`Time`       | Time in nanosecond
`Time step`  | Current time step size
`Tfail`      | Number of times the adaptive time stepping shrinks the time step
`Nfail`      | Number of times the Newton's iterative solver diverges
`Other fail` | Number of times the linear solver diverges
`Av. EOP`    | Film-averaged electronic order parameter
`Av. SOP`    | Film-averaged structural order parameter
`V (V)`      | Voltage drop across the VO<sub>2</sub> film
`R (Ohm)`    | Resistance of the VO<sub>2</sub> film

The time-dependent voltage drop across the VO<sub>2</sub> film (Column `V (V)` in `log.txt`) generated in the example is shown in the figure below.

<p align="center">
<img src="https://github.com/DOE-COMMS/Q-POP-Modules/files/12205719/sch_osc.pdf" alt="Temporal evolution of the voltage drop across the film." width="500">
</p>

You can see the self-oscillation of the voltage output. The `psi.pvd` generated in the example is shown as several snapshots below.

<p align="center">
<img src="https://github.com/DOE-COMMS/Q-POP-Modules/files/12197852/morph.pdf" alt="Spatiotemporal evolution of the electronic order parameter" width="500">
</p>

$\Psi$ represents the electronic phases: $\Psi=0$ means metal while $|\Psi|\sim 1$ means insulator. You will see a metallic filament growing and shrinking back and forth, generating the oscillating voltage output across the VO<sub>2</sub> film.
