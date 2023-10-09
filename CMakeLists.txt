cmake_minimum_required(VERSION 3.15)

project(zlib-kit VERSION 1.0 LANGUAGES C CXX)

include(ExternalProject)
include(CMakePrintHelpers)
include(GNUInstallDirs)

#-----------------------------------------------------------------------
# Options
#-----------------------------------------------------------------------
option(GIT_PROGRESS "Show Git progress" TRUE)

#-----------------------------------------------------------------------
# CMAKE_BUILD_TYPE
#-----------------------------------------------------------------------
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the build type." FORCE)
endif()

set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
  "Debug;Release;RelWithDebInfo")

#-----------------------------------------------------------------------
# CMAKE_INSTALL_PREFIX
#-----------------------------------------------------------------------
set(PREFIX "${CMAKE_SOURCE_DIR}/install")
set(CMAKE_INSTALL_PREFIX "${PREFIX}" CACHE STRING "Install path prefix." FORCE)

#-----------------------------------------------------------------------
# ZLIB
#-----------------------------------------------------------------------
set(ZLIB_GIT_URL    https://github.com/madler/zlib)
set(ZLIB_GIT_TAG    04f42ceca40f73e2978b50e93806c2a18c1281fc) # v1.2.13

ExternalProject_Add(zlib
  GIT_REPOSITORY  ${ZLIB_GIT_URL}
  GIT_TAG         ${ZLIB_GIT_TAG}
  GIT_PROGRESS ON ${GIT_PROGRESS}

  SOURCE_DIR
    ${CMAKE_CURRENT_SOURCE_DIR}/src/zlib
  CMAKE_ARGS
    -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
    -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
  INSTALL_COMMAND
    ${CMAKE_MAKE_PROGRAM} install
)

