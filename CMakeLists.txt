cmake_minimum_required(VERSION 2.8)
project(METIS C)

set(SHARED FALSE CACHE BOOL "build a shared library")

if(MSVC)
  set(METIS_INSTALL FALSE)
else()
  set(METIS_INSTALL TRUE)
endif()

# Configure libmetis library.
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)

set( idxwidth "32" CACHE STRING "Integer length (int32 or int64)" )
set_property(CACHE idxwidth PROPERTY STRINGS 32 64)
mark_as_advanced(CLEAR idxwidth)

set( realwidth "64" CACHE STRING "Real length (float or double)" )
set_property(CACHE realwidth PROPERTY STRINGS 32 64)
mark_as_advanced(CLEAR realwidth)

if (NOT idxwidth)
    set(idxwidth 32)
endif()
if (NOT realwidth)
    set(realwidth 64)
endif()

string(CONCAT IDX "\#define IDXTYPEWIDTH " ${idxwidth} "\n")
string(CONCAT REAL "\#define REALTYPEWIDTH " ${realwidth} "\n")

make_directory(${CMAKE_BINARY_DIR}/xinclude)
file(WRITE "${CMAKE_BINARY_DIR}/xinclude/metis.h" ${IDX})
file(APPEND "${CMAKE_BINARY_DIR}/xinclude/metis.h" ${REAL})
execute_process(COMMAND cat ${CMAKE_SOURCE_DIR}/include/metis.h 
                OUTPUT_VARIABLE outVar)
file(APPEND "${CMAKE_BINARY_DIR}/xinclude/metis.h" ${outVar})
#execute_process(COMMAND cat ${CMAKE_SOURCE_DIR}/include/metis.h >> ${CMAKE_BINARY_DIR}/xinclude/metis.h)
execute_process(COMMAND cp ${CMAKE_SOURCE_DIR}/include/CMakeLists.txt ${CMAKE_BINARY_DIR}/xinclude)

include(./conf/gkbuild.cmake)

# METIS' custom options
#option(IDX64 "enable 64 bit ints" OFF)
#option(REAL64 "enable 64 bit floats (i.e., double)" OFF)
#if(IDX64)
#  set(METIS_COPTIONS "${METIS_COPTIONS} -DIDXTYPEWIDTH=64")
#else()
#  set(METIS_COPTIONS "${METIS_COPTIONS} -DIDXTYPEWIDTH=32")
#endif(IDX64)
#if(REAL64)
#  set(METIS_COPTIONS "${METIS_COPTIONS} -DREALTYPEWIDTH=64")
#else()
#  set(METIS_COPTIONS "${METIS_COPTIONS} -DREALTYPEWIDTH=32")
#endif(REAL64)
#
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${METIS_COPTIONS}")


# Add include directories.
# i.e., the -I equivalent
include_directories(build/xinclude)
include_directories(${GKLIB_PATH}/include)
include_directories(${CMAKE_INSTALL_PREFIX}/include)

# List of paths that the compiler will search for library files.
# i.e., the -L equivalent
link_directories(${GKLIB_PATH}/lib)
link_directories(${CMAKE_INSTALL_PREFIX}/lib)

# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("build/xinclude")
add_subdirectory("libmetis")
add_subdirectory("programs")
