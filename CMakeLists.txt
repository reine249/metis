cmake_minimum_required(VERSION 3.5)

cmake_policy(SET CMP0048 NEW)

project(METIS VERSION 1.0.0 LANGUAGES C)

set(GKLIB_PATH "${CMAKE_CURRENT_SOURCE_DIR}/GKlib" CACHE PATH "path to GKlib")
set(SHARED FALSE CACHE BOOL "build a shared library")

set(METIS_INSTALL TRUE)

# Configure libmetis library.
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)

set( IDX_TYPE_WIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
set( REAL_TYPE_WIDTH "32" CACHE STRING "Integer length (int32 or int64)" )
configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/include/metis.h.in ${CMAKE_CURRENT_SOURCE_DIR}/include/metis.h )

#include(${GKLIB_PATH}/GKlibSystem.cmake)
include(${CMAKE_CURRENT_SOURCE_DIR}/GKlib/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
add_subdirectory("programs")

