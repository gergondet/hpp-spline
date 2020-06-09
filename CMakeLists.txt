cmake_minimum_required(VERSION 3.1.3)

SET(PROJECT_NAME hpp-spline)
SET(PROJECT_DESCRIPTION "template based classes for creating and manipulating spline and bezier curves. Comes with extra options specific to end-effector trajectories in robotics.")
set(PROJECT_USE_CMAKE_EXPORT TRUE)

INCLUDE(cmake/hpp.cmake)
INCLUDE(cmake/test.cmake)
INCLUDE(cmake/python.cmake)
INCLUDE(cmake/boost.cmake)

# Disable -Werror on Unix for now.
SET(CXX_DISABLE_WERROR True)
SET(CMAKE_VERBOSE_MAKEFILE True)
SET(DOXYGEN_USE_MATHJAX YES)

COMPUTE_PROJECT_ARGS(PROJECT_ARGS LANGUAGES CXX)
PROJECT(${PROJECT_NAME} ${PROJECT_ARGS})

find_package(Eigen3 QUIET NO_CMAKE_PACKAGE_REGISTRY)
if(Eigen3_FOUND)
  add_project_dependency(Eigen3 REQUIRED NO_CMAKE_PACKAGE_REGISTRY)
else()
  add_project_dependency(Eigen3 MODULE REQUIRED)
endif()

OPTION (BUILD_PYTHON_INTERFACE "Build the python binding" ON)
IF(BUILD_PYTHON_INTERFACE)
  # search for python
  FINDPYTHON()
  include_directories(SYSTEM ${PYTHON_INCLUDE_DIRS} )

  SET(BOOST_COMPONENTS python)
  SEARCH_FOR_BOOST()

  include_directories(SYSTEM ${Boost_INCLUDE_DIRS} )

  add_subdirectory (python)
ENDIF(BUILD_PYTHON_INTERFACE)

ADD_SUBDIRECTORY(include/hpp/spline)
ADD_SUBDIRECTORY(tests)
