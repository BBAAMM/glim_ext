cmake_minimum_required(VERSION 3.5)
project(glim_ext)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")

#################
## ROS-related ##
#################
if(DEFINED ENV{ROS_VERSION})
  if($ENV{ROS_VERSION} EQUAL 2)
    find_package(ament_cmake REQUIRED)
  elseif($ENV{ROS_VERSION} EQUAL 1)
    # ROS1
    find_package(catkin REQUIRED)
  endif()
endif()

find_package(glim REQUIRED)

add_library(glim_ext SHARED
  src/glim_ext/util/config_ext.cpp
)
target_include_directories(glim_ext PUBLIC
  include
)
target_link_libraries(glim_ext
  glim::glim
)
target_compile_options(glim_ext PRIVATE
  -std=c++17
)
list(APPEND glim_ext_LIBRARIES glim_ext)

add_subdirectory(predictor_cxx)
list(APPEND glim_ext_LIBRARIES predictor_cxx)

#############
## Install ##
#############
install(TARGETS ${glim_ext_LIBRARIES}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

#################
## ROS-related ##
#################
if(DEFINED ENV{ROS_VERSION})
  if($ENV{ROS_VERSION} EQUAL 2)
    # ROS2
    install(DIRECTORY config DESTINATION share/glim_ext)
    ament_target_dependencies(glim_ext glim)
    ament_export_libraries(${glim_ext_LIBRARIES})
    ament_package()
  elseif($ENV{ROS_VERSION} EQUAL 1)
    # ROS1
    catkin_package(
      LIBRARIES ${glim_ext_LIBRARIES}
    )
  endif()
endif()
