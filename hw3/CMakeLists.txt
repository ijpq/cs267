cmake_minimum_required(VERSION 3.14)
project(hw3 LANGUAGES CXX)

find_package(UPCXX REQUIRED)

# Group number
set(GROUP_NAME "None" CACHE STRING "Your group name as it appears on bCourses (no spaces)")

# K-mer length
set(KMER_LEN "19" CACHE STRING "The length of the K-mers")

# Packaging system
if (NOT ${GROUP_NAME} STREQUAL None)
    set(CPACK_GENERATOR TGZ)
    set(CPACK_PACKAGE_FILE_NAME "cs267${GROUP_NAME}_hw3")
    install(FILES kmer_hash.cpp hash_map.hpp DESTINATION .)
    install(FILES ${CPACK_PACKAGE_FILE_NAME}.pdf DESTINATION .)
    include(CPack)
endif ()

# Build the kmer_hash executable
add_executable(kmer_hash kmer_hash.cpp)
target_link_libraries(kmer_hash PRIVATE UPCXX::upcxx)
target_compile_definitions(kmer_hash PRIVATE "KMER_LEN=${KMER_LEN}")

# Copy the job scripts
configure_file(job-cori-starter job-cori-starter COPYONLY)

