##
## Copyright (c) 2018-2021, Carnegie Mellon University
## All rights reserved.
##
## See LICENSE file for full information
##

## specify this only here, lower levels use CMAKE_MINIMUM_REQUIRED_VERSION that this sets
cmake_minimum_required (VERSION 3.14)

set (CMAKE_BUILD_TYPE Release  CACHE STRING "Debug, Release, RelWithDebInfo, MinSizeRel")

project (FFTX_PROJECT
         VERSION 1.0.1
                DESCRIPTION "FFTX Project"
                LANGUAGES C CXX )

##  Prevent building directly into the source tree
string (COMPARE EQUAL "${CMAKE_SOURCE_DIR}" "${CMAKE_BINARY_DIR}" DISALLOW_COMPILE_INPLACE)
if ( DISALLOW_COMPILE_INPLACE )
    message (FATAL_ERROR "Building ${PROJECT_NAME} with CMake requires an out-of-source tree.  To proceed:
                 rm -rf CMakeCache.txt CMakeFiles/   # delete files in ${CMAKE_SOURCE_DIR}
                 mkdir <build>
                 cd <build>
                 cmake ..
                 make | make install")
endif ()

##  Find python3 -- used to marshall/run examples

find_package (Python3 COMPONENTS Interpreter)
if (${Python3_FOUND})
    ##  It exists, executable is ${Python3_EXECUTABLE}
    message ( STATUS "Found Python3: Version = ${Python3_VERSION}, Executable = ${Python3_EXECUTABLE}")
else ()
    message ( SEND_ERROR "Python3 NOT FOUND: Python is required to build/run examples")
endif ()

set ( FFTX_CMAKE_INCLUDE_DIR ${FFTX_PROJECT_SOURCE_DIR}/CMakeIncludes )

##  Add the CMakeIncludes folder so the FindSphinx module is found
set ( CMAKE_MODULE_PATH "${FFTX_PROJECT_SOURCE_DIR}/CMakeIncludes" ${CMAKE_MODULE_PATH} )

set ( FFTX_INCLUDE ${FFTX_PROJECT_SOURCE_DIR}/include )

if ( (NOT DEFINED CMAKE_BUILD_TYPE) OR (NOT CMAKE_BUILD_TYPE) )
    set ( CMAKE_BUILD_TYPE Release )
endif ()

add_subdirectory (docs)

enable_testing()

