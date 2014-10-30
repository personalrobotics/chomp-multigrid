cmake_minimum_required(VERSION 2.8.3)
project(chomp_multigrid)

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")
find_package(catkin REQUIRED COMPONENTS cmake_modules)
find_package(Eigen REQUIRED)
find_package(PNG REQUIRED)
find_package(EXPAT REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)

find_package(CAIRO)
find_package(GLEW)

include(FindPkgConfig)
pkg_search_module(CCD REQUIRED ccd libccd)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES "${PROJECT_NAME}" mzcommon
  DEPENDS glut eigen libccd
)

include_directories(
  ${Eigen_INCLUDE_DIRS}
  ${PNG_INCLUDE_DIRS}
  ${EXPAT_INCLUDE_DIRS}
  ${GLEW_INCLUDE_DIRS}
  ${GLUT_INCLUDE_DIR}
  ${CAIRO_INCLUDE_DIRS}
  ${CCD_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)
add_definitions(
  ${Eigen_DEFINITIONS}
  ${PNG_DEFINITIONS}
)

if (CAIRO_FOUND)
  add_definitions(-DMZ_HAVE_CAIRO)
endif ()

set(CMAKE_C_FLAGS "-Wall -g")
set(CMAKE_CXX_FLAGS "-Wall -g")

if(APPLE)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wsign-compare -Wno-deprecated")
  set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -Wsign-compare -Wno-deprecated")
endif()

macro(add_gui_app name)
  if(APPLE)
    add_executable("${name}" MACOSX_BUNDLE ${ARGN})
  else()
    add_executable("${name}" ${ARGN})
  endif()
endmacro(add_gui_app)

add_subdirectory(mzcommon)
add_subdirectory(chomp)
