if(NOT ("$ENV{ROS_Distributions}" STREQUAL "electric" OR "$ENV{ROS_DISTRO}" STREQUAL "fuerte"))
  include(catkin.cmake)
  return()
endif()
cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
rosbuild_init()
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(PC_NITE_DEV_INCLUDE_DIRS "/usr/include/nite")
find_package(PkgConfig)
pkg_check_modules(PC_LIBOPENNI REQUIRED libopenni)
include_directories(
  ${PC_LIBOPENNI_INCLUDE_DIRS}
  ${PC_NITE_DEV_INCLUDE_DIRS})
if($ENV{ROS_DISTRO} STREQUAL "groovy")
  add_definitions(-DUSE_PCL_AS_PCL_MSGS)
endif()
rosbuild_add_executable(openni_swipe src/openni_swipe.cpp)

# https://code.ros.org/trac/ros-pkg/ticket/5287
set(PC_NITE_DEV_LIBRARIES XnVNite_1_3_1;OpenNI)
target_link_libraries(openni_swipe ${PC_NITE_DEV_LIBRARIES})

