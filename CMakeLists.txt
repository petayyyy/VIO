cmake_minimum_required(VERSION 2.8.3)
project(px4_realsense_bridge)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  dynamic_reconfigure
  tf
  pcl_ros
  mavros
  mavros_extras
  mavros_msgs
  mavlink
)
find_package(PCL 1.7 REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp rospy std_msgs mavros_msgs geometry_msgs mav_msgs sensor_msgs message_runtime tf
)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif(NOT CMAKE_BUILD_TYPE)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${YAML_CPP_INCLUDE_DIR}
)

set(PX4_REALSENSE_BRIDGE_CPP_FILES   "src/nodes/px4_realsense_bridge.cpp"
                              "src/nodes/px4_realsense_bridge_node.cpp"
)

add_library(px4_realsense_bridge     "${PX4_REALSENSE_BRIDGE_CPP_FILES}")

add_dependencies(px4_realsense_bridge ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(px4_realsense_bridge_node src/nodes/px4_realsense_bridge_node.cpp)

target_link_libraries(px4_realsense_bridge_node
  px4_realsense_bridge
  ${catkin_LIBRARIES}
  ${YAML_CPP_LIBRARIES})
