Copyright (c) 2022 by Lucas Lapostolle
Use under LGPL.txt library license

This file is part of EVEREST.

EVEREST is free software: you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

EVEREST is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License along with EVEREST. If not, see <https://www.gnu.org/licenses/>.

If you use this code, please acknowledge the source : code EVEREST, associated with the article Lucas Lapostolle, Katell Derrien, Léo Morin, Laurent Berthe and Olivier Castelnau, Fast numerical estimation of residual stresses induced by 
laser shock peening, submitted to EJM, 2022.
It simulates the elastic-plastic stress wave propagation caused by a laser shock for a uniaxial strain. This is thus a 1D code.
It also computes the residual stresses induced by the stress wave.

To be run, the program requires four files:
- stress_wave_propagation.py: the python file to be executed.
- controle_file.py: python file which contains user-defined parameters for the simulation. It is imported in the stress_wave_propagation.py file.
- controle_file_316L_RS_template.py: python file which contains user-defined parameters for the simulation. It is imported in the stress_wave_propagation.py file. The parameters of this file are a template to reproduce the results of Figure 8(b)
  of the article "Fast numerical estimation of residual stresses induced by laser shock peening", submitted to EJM, 2022. To import control_file.py, please comment line 527 of the stress_wave_propagation.py file.
- controle_file_316L_backface_template.py: python file which contains user-defined parameters for the simulation. It is imported in the stress_wave_propagation.py file. The parameters of this file are a template to reproduce the results of Figure 5(b)
  of the article "Fast numerical estimation of residual stresses induced by laser shock peening", submitted to EJM, 2022. To import control_file.py, please comment line 575 of the stress_wave_propagation.py file.
- pressure_loading: text file defining the pressure loading for the laser application. The first column corresponds to the time marks, and the second column the corresponding values of the normalized pressure.

All these files must be in the same folder for the program to run properly.

Only controle_file.py has to be modified with the proper parameters, and the python file needs just to be run.

Lines 527 to 576 are for template simulations, and can be removed or commented.

The program will generate eight output files:
- s11.out: text file, table whose columns are the spatial distribution of the axial stress, the different columns corresponding to the different instants of the simulation.
- s22.out: text file, table whose columns are the spatial distribution of the radial stress, the different columns corresponding to the different instants of the simulation.
- v1.out: text file, table whose columns are the spatial distribution of the axial velocity, the different columns corresponding to the different instants of the simulation.
- epsp.out: text file, table whose columns are the spatial distribution of the axial plastic strain, the different columns corresponding to the different instants of the simulation.
- p.out: text file, table whose columns are the spatial distribution of the accumulated plastic strain, the different columns corresponding to the different instants of the simulation.
- vm.out: text file, table whose columns are the spatial distribution of the von Mises stress, the different columns corresponding to the different instants of the simulation.
- t.out: text file, vector containing the different time marks of the simulation.
- RS.out: text file, vector containing the spatial values of the residual stresses.

The time increment separating the results from different columns in the .out files is computed from the CFL relation cfl=ce*dt/dx, with cfl<=1, the space increment dx and the elastic velocity ce being calculated from the input
parameters L, N, E, nu and rho.
The time increment can be accessed via the command "res_propa[6][1]" once the simulation has finished, giving the first non-zero time mark of the simulation.

The input file controle_file.in requires the following parameters:
- Length of the domain (mm)
- Duration the simulation (s)
- Number of nodes of the simulation (a warning is raised if the number of nodes is not high enough to have a fine enough time sampling of the loading signal)
- Loading Amplitude (MPa)
- Young's modulus (MPa)
- Poisson ratio
- Density (t/mm^3)
- Yield strength (MPa) (A parameter of the Johnson-Cook model)
- B parameter of the Johnson-Cook model (MPa)
- C parameter of the Johnson-Cook model
- n parameter of the Johnson-Cook model
- reference strain rate for the Johnson-Cook model (/s) (an assertion error is raised if this value is 0)
- number of shots (an error is raised if this value is 0)
- Boundary condition (0 : Reflective, 1 : Transmittive)
- CFL number (1 by default)
- root finding method : the algorithm uses the function scipy.optimize.root_scalar ('ridder' by default). This argument must be one of the methods listed at https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.root_scalar.html .
- The increment interval used to store the results.

Upon execution, the code displays figures with the in-depth plastic strains and residual stresses profiles, as well as the backface velocity for illustration purposes. The corresponding lines (527 to 576) can be freely commented.
