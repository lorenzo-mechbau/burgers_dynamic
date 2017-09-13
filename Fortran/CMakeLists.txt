# Add example executable
add_executable(dynamicburgers_fortran src/FortranExample.f90)

# Turn on Fortran preprocessing (#include directives)
if (MSVC)
    set(PROC_OPT "/fpp")
else()
    set(PROC_OPT "-cpp")
endif()
target_compile_options(dynamicburgers_fortran PRIVATE ${PROC_OPT})

# Link to opencmiss
# -----------------
#
# This simply uses the 'opencmisslibs' target created by finding the OpenCMISSLibs package earlier.
#
# Alternatively, you can also directly invoke 'find_package(Iron <IRON_VERSION>)' to explicitly
# require that version.  If you do this you will also need to change the target link library from
# 'opencmisslibs' to 'iron'.
target_link_libraries(dynamicburgers_fortran PRIVATE opencmisslibs mpi)

# Add a simple test that runs the executable
add_test(NAME DynamicBurgers_Fortran COMMAND dynamicburgers_fortran)
# Helper function defined in OCMiscFunctions.cmake
#add_opencmiss_environment(DynamicBurgers_Fortran)

# Add an install target to place the executable in the install tree.
install(TARGETS dynamicburgers_fortran DESTINATION bin)

###################
# Developer notice!
#
# If you write Fortran code and use MPI, you need to use the following MPI directives:
#
# #ifndef NOMPIMOD
#   USE MPI
# #endif
# [...]
#   IMPLICIT NONE
# [...]
# #ifdef NOMPIMOD
#     #include "mpif.h"
# #endif
#
# Reasoning: In some cases like Windows/MPICH2 there sometimes is no mpi.mod file. In order to yet make
# the example work the build system adds the definition 'NOMPIMOD', which can be checked and acted to accordingly.
#
