cmake_minimum_required(VERSION 2.8.12)
project(moveit_planners_ompl)

# At least C++11 required for OMPL
# add_compile_options(-std=c++14) #CHANGED TO BELOW (c++11)
add_compile_options(-std=c++11)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

# added eigen_conversions and replaced tf2_ros with tf 
find_package(Boost REQUIRED system filesystem date_time thread serialization)
find_package(catkin REQUIRED COMPONENTS
  moveit_core
  moveit_ros_planning
  roscpp
  rosconsole
  pluginlib
  tf
  dynamic_reconfigure
  eigen_conversions
  )

find_package(ompl REQUIRED)

generate_dynamic_reconfigure_options("ompl_interface/cfg/OMPLDynamicReconfigure.cfg")

# removed ${OMPL_INCLUDE_DIRS} from INCLUDE_DIRS
catkin_package(
  LIBRARIES
    moveit_ompl_interface
    ${OMPL_LIBRARIES}
  INCLUDE_DIRS
    ompl_interface/include
  CATKIN_DEPENDS
    moveit_core
)

include_directories(ompl_interface/include
                    ${Boost_INCLUDE_DIRS}
                    ${OMPL_INCLUDE_DIRS}
                    ${catkin_INCLUDE_DIRS})

link_directories(${OMPL_LIBRARY_DIRS})

# REMOVING BELOW LINK DIRECTORIES
#link_directories(${catkin_LIBRARY_DIRS})
#link_directories(${Boost_LIBRARY_DIRS})

add_subdirectory(ompl_interface)

install(FILES ompl_interface_plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
