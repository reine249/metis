cmake_minimum_required(VERSION 3.5)

cmake_policy(SET CMP0048 NEW)

project(METIS VERSION 1.0.0 LANGUAGES C)

set(GKLIB_PATH "${CMAKE_SOURCE_DIR}/GKlib" CACHE PATH "path to GKlib")
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

set( IDX_TYPE_WIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
set( REAL_TYPE_WIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
configure_file( ${CMAKE_SOURCE_DIR}/include/metis.h.in ${CMAKE_SOURCE_DIR}/include/metis.h )

include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
add_subdirectory("programs")

#cmake_minimum_required(VERSION 2.8)
#project(METIS C)
#
#set(SHARED FALSE CACHE BOOL "build a shared library")
#
#if(MSVC)
#  set(METIS_INSTALL FALSE)
#else()
#  set(METIS_INSTALL TRUE)
#endif()
#
## Configure libmetis library.
#if(SHARED)
#  set(METIS_LIBRARY_TYPE SHARED)
#else()
#  set(METIS_LIBRARY_TYPE STATIC)
#endif(SHARED)
#
#set( IDX_TYPE_WIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
#set( REAL_TYPE_WIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
#configure_file( ${CMAKE_SOURCE_DIR}/include/metis.h.in ${CMAKE_SOURCE_DIR}/include/metis.h )
#
#set( IDXWIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
#set_property(CACHE IDXWIDTH PROPERTY STRINGS 32 64)
#mark_as_advanced(CLEAR IDXWIDTH)
#set( REALWIDTH "64" CACHE STRING "Real length (float or double)" )
#set_property(CACHE REALWIDTH PROPERTY STRINGS 32 64)
#mark_as_advanced(CLEAR REALWIDTH)
#if (NOT IDXWIDTH)
#    set(IDXWIDTH 32)
#endif()
#if (NOT REALWIDTH)
#    set(REALWIDTH 64)
#endif()
#string(CONCAT IDX "\#define IDXTYPEWIDTH " ${IDXWIDTH} "\n")
#string(CONCAT REAL "\#define REALTYPEWIDTH " ${REALWIDTH} "\n")
#make_directory(${CMAKE_BINARY_DIR}/xinclude)
#file(WRITE "${CMAKE_BINARY_DIR}/xinclude/metis.h" ${IDX})
#file(APPEND "${CMAKE_BINARY_DIR}/xinclude/metis.h" ${REAL})
#execute_process(COMMAND cat ${CMAKE_SOURCE_DIR}/include/metis.h 
#                OUTPUT_VARIABLE outVar)
#file(APPEND "${CMAKE_BINARY_DIR}/xinclude/metis.h" ${outVar})
#execute_process(COMMAND cp ${CMAKE_SOURCE_DIR}/include/CMakeLists.txt ${CMAKE_BINARY_DIR}/xinclude)
#
## Configuration options.
#set(IDX64 "OFF" CACHE STRING "integer length")
#set(REAL64 "OFF" CACHE STRING "float length")
#set(GDB "OFF" CACHE STRING "gdb option")
#set(ASSERT "OFF" CACHE STRING "assert option")
#set(ASSERT2 "OFF" CACHE STRING "assert option 2")
#set(DEBUG "OFF" CACHE STRING "debug option")
#set(GPROF "OFF" CACHE STRING "gprof option")
##set(OPENMP "not-set" CACHE STRING "openmp option")
##set(SHARED "not-set" CACHE STRING "shared option")
##set(CMAKE_C_COMPILER "not-set" CACHE STRING "c compiler option")
#set(GKLIB_PATH "${CMAKE_SOURCE_DIR}/GKlib" CACHE STRING "path to GKLIB")
#
#include(${GKLIB_PATH}/GKlibSystem.cmake)
## Add include directories.
#include_directories(${GKLIB_PATH})
#include_directories(include)
## Recursively look for CMakeLists.txt in subdirs.
#add_subdirectory("include")
#add_subdirectory("libmetis")
#add_subdirectory("programs")
#
## METIS' custom options
##option(IDX64 "enable 64 bit ints" OFF)
##option(REAL64 "enable 64 bit floats (i.e., double)" OFF)
##if(IDX64)
##  set(METIS_COPTIONS "${METIS_COPTIONS} -DIDXTYPEWIDTH=64")
##else()
##  set(METIS_COPTIONS "${METIS_COPTIONS} -DIDXTYPEWIDTH=32")
##endif(IDX64)
##if(REAL64)
##  set(METIS_COPTIONS "${METIS_COPTIONS} -DREALTYPEWIDTH=64")
##else()
##  set(METIS_COPTIONS "${METIS_COPTIONS} -DREALTYPEWIDTH=32")
##endif(REAL64)
##
##set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${METIS_COPTIONS}")
