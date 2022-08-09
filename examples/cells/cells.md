#Running cells With Sensei Instructions (Personal Computer)

1. copy the folder `cells` to `src/SENSEI/miniapps/`

2. Add option `ENABLE_CELLS` in the `CMakeLists.txt` in the folder `miniapps` following the example shown in the file

3. Install SENSEI with option `-DENABLE_CELLS=ON` to cmake options 

4. the executable `cells` will be generated under `install/SENSEI/bin`, copy it to the `src/SENSEI/miniapps/cells` folder to run it. However, it has errors `Attemps to use an MPI routine before initialize MPICH`

