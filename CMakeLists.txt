########################################################################
# Project setup
########################################################################
cmake_minimum_required(VERSION 2.8.12...3.10)
project(SoapyRedPitaya CXX)

#select the release build type by default to get optimization flags
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
    message(STATUS "Build type not specified: defaulting to release.")
endif(NOT CMAKE_BUILD_TYPE)
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING "")

########################################################################
# build the module
########################################################################
find_package(SoapySDR NO_MODULE REQUIRED)

get_filename_component(_SoapySDR_ROOT "${SoapySDR_INCLUDE_DIRS}/.." ABSOLUTE)
message(STATUS "SoapySDR root directory: ${_SoapySDR_ROOT}")
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT AND NOT "${_SoapySDR_ROOT}" STREQUAL "/usr")
    set_property(CACHE CMAKE_INSTALL_PREFIX PROPERTY VALUE ${_SoapySDR_ROOT})
endif()

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR "${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
    #disable warnings for unused parameters
    add_compile_options(-Wno-unused-parameter)
endif()

SOAPY_SDR_MODULE_UTIL(
    TARGET RedPitaya
    SOURCES SoapyRedPitaya.cpp
)
