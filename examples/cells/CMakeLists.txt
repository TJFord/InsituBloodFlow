#set(OSCILLATOR_CUDA OFF CACHE BOOL "Enable CUDA acceleration in the miniapp")
#if (OSCILLATOR_CUDA)
#set(ENABLE_CUDA ON)
#include(cuda)
#message(STATUS "oscillators miniapp will use CUDA")
#dif()

set(sources cellFlow.cpp)
set(libs sMPI sDIY sOPTS thread)

option(ENABLE_MPI "Enable MPI" ON)
option(ENABLE_SENSEI "Enable MPI" ON)
if(ENABLE_SENSEI)
    find_package(SENSEI,REQUIRED)
    if(SENSEI_FOUND)
      message("FOUND SENSEI")
    endif()
    list(APPEND sources Bridge.cpp LPdataAdaptor.cpp)

  list(APPEND libs sensei)
endif()
   if(ENABLE_MPI)
      message("Enabling MPI")
      find_package(MPI REQUIRED)
      if(MPI_CXX_FOUND)
          #set(CMAKE_CXX_COMPILER "${MPI_CXX_COMPILER}")
          include_directories(${MPI_CXX_INCLUDE_PATH})
          set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${MPI_CXX_COMPILE_FLAGS}")
          add_definitions(-DPLB_MPI_PARALLEL)
      endif()
   endif()

#   if (OSCILLATOR_CUDA)
#     set_source_files_properties(main.cpp
#       Block.cpp BlockInternals.cpp Oscillator.cpp
#       PROPERTIES LANGUAGE CUDA)
#   endif()

set(PALABOS_ROOT "$ENV{HOME}/insituBloodFlow/src/palabos" CACHE PATH "Path to PALABOS root directory")
set(LAMMPS_DIR "$ENV{HOME}/insituBloodFlow/src/lammps" CACHE PATH "Path to LAMMPS root directory")
set(BLOODFLOW_ROOT "$ENV{HOME}/insituBloodFlow/BloodFlowTan" CACHE PATH "Path to BloodFlow root directory")
set(LAMMPS_LIB "${LAMMPS_DIR}/src")

###############################################################################
# Palabos Library
###############################################################################
include_directories("${PALABOS_ROOT}/src")
include_directories("${PALABOS_ROOT}/externalLibraries")
include_directories("${PALABOS_ROOT}/externalLibraries/Eigen3")
include_directories("${BLOODFLOW_ROOT}/ibm")
include_directories("${LAMMPS_LIB}")
file(GLOB_RECURSE PALABOS_SRC "${PALABOS_ROOT}/src/*.cpp")
file(GLOB_RECURSE EXT_SRC "${PALABOS_ROOT}/externalLibraries/tinyxml/*.cpp")
add_library(palabos STATIC ${PALABOS_SRC} ${EXT_SRC})
###############################################################################
#  LAMMPS Library
include_directories("${LAMMPS_LIB}")
include_directories("${COUPLING_DIR}/src")
add_library(lammps STATIC IMPORTED)
set_property(TARGET lammps PROPERTY
             IMPORTED_LOCATION "${LAMMPS_LIB}/liblammps_mpi.a")
#
list(APPEND libs palabos lammps)
add_executable(cells ${sources})
target_link_libraries(cells ${libs})

#if(ENABLE_MPI)
#    target_link_libraries(cells ${MPI_CXX_LIBRARIES})
#endif()
#   if (OSCILLATOR_CUDA)
#     target_compile_definitions(oscillator PUBLIC OSCILLATOR_CUDA)
#     target_compile_features(oscillator PUBLIC cxx_std_17)
#     set_target_properties(oscillator PROPERTIES POSITION_INDEPENDENT_CODE ON)
#     set_target_properties(oscillator PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
#   endif ()

target_include_directories(cells SYSTEM PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

install(TARGETS cells RUNTIME DESTINATION bin)

#add_subdirectory(testing)
