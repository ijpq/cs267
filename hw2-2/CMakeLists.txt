cmake_minimum_required(VERSION 3.14)
project(hw2_2)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED YES)

find_package(MPI REQUIRED)

# Group number
set(GROUP_NAME "None" CACHE STRING "Your group name as it appears on bCourses (no spaces)")

# Packaging system
if (NOT ${GROUP_NAME} STREQUAL None)
    set(CPACK_GENERATOR TGZ)
    set(CPACK_PACKAGE_FILE_NAME "cs267${GROUP_NAME}_hw2_2")
    install(FILES mpi.cpp DESTINATION .)
    install(FILES ${CPACK_PACKAGE_FILE_NAME}.pdf DESTINATION .)
    include(CPack)
endif ()

# We require the GNU compiler for this assignment.
if (NOT "${CMAKE_C_COMPILER_ID}" STREQUAL "GNU")
    set(Prg-Intel "PrgEnv-intel")
    set(Prg-Clang "PrgEnv-cray")
    message(WARNING
            "Must use GNU Compiler for submission. Make sure you ran:\n"
            "module swap ${Prg-${CMAKE_C_COMPILER_ID}} PrgEnv-gnu")
endif ()

add_executable(mpi main.cpp mpi.cpp common.h)
target_link_libraries(mpi PRIVATE MPI::MPI_CXX)

# Copy the job scripts
configure_file(job-mpi job-mpi COPYONLY)
