# How to run the example code

This example is without **in site** component. 

The embolism example is tested with the following versions:
- Palabos (https://gitlab.com/unigespc/palabos): 
`git clone git@gitlab.com:unigespc/palabos.git` (clone with ssh)
`git clone https://gitlab.com/unigespc/palabos.git` (clone with https)
- LAMMPS  (https://github.com/lammps/lammps): 
`git clone git@github.com:lammps/lammps.git` (clone with ssh)
`git clone https://github.com/lammps/lammps.git` (clone with https)
- Make sure you have cmake (version 3.17.3), make (GNU Make 4.2.1), openmpi (Open MPI 4.0.3) installed. My tested version for each are listed in the parenthesis. Other versions may work but I haven't tested yet.
 
1. Clone the `InsituBloodFlow` repository. In `BloodFlow/sites/cooley.cmake`, there are a few file paths that need to be modified. This is only required for running embolism on Cooley
`git clone https://github.com/TJFord/InsituBloodFlow.git` (clone with https)
2. Embolism needs several files to be copied from `BloodFlow/rbc` to `lammps/src`. All the files need to be copied.  These all begin with fix. 
When in `InsituBloodFlow/rbc`: 
 `cp * path/to/lammps/src`
3. The `CMakeLists.txt` file needs to be updated with correct paths. e.g., the path to `Palabos`, `Lammps` and `InsituBloodFlow/ibm`. 
4. Go to `lammps/src` and make sure the MOLECULE and MC packages are installed : 
`make yes-<packagename>`
5. Compile lammps as a library, go to `lammps/src`, type `make mpi mode=lib`. You will need to append `-std=c++11` flags in CCFLAGS in Makefile.mpi under lammps/src/MAKE folder. If you use `cmake` then please follow the lammps tutorial on how to build it. 
6. go to `examples/no_insitu` and make a directory called `build`. **NOTE:** For running on your personnal computer the command `cmake ..` should be used. 
7. A executable named `alveolus` should now exist in the `no_insitu` directory. **NOTE:** For your personnal computer, use a command like this when in the embolism directory: `mpirun -np 4 alveolus in.lmp4cell 0`


