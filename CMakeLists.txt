cmake_minimum_required(VERSION 3.0.2)
project(nmea_ros_driver)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  std_msgs
  roscpp
  rospy
  sensor_msgs
  geometry_msgs
  message_filters
  message_generation
  serial
  tf
  tf2
)

find_package(Boost REQUIRED COMPONENTS system)

add_message_files(
  FILES
  GPSPoint.msg
  GPSRPY.msg
  XXGGA.msg
  XXGSA.msg
  XXGSV.msg
  XXRMC.msg
  XXVTG.msg
  XXGLL.msg
  NTR.msg
  PJK.msg
  TRA.msg
  HEADINGA.msg
  BESTPOSA.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES nmea_ros_driver
  CATKIN_DEPENDS std_msgs message_runtime roscpp rospy sensor_msgs serial 
#  DEPENDS system_lib
  DEPENDS Boost
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_executable(nmea_driver_node 
  src/nmea_ros_driver.cpp
)
add_dependencies(nmea_driver_node 
  ${${PROJECT_NAME}_EXPORTED_TARGETS} 
  ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(nmea_driver_node
  ${catkin_LIBRARIES}
)

add_executable(nmea_analyze_node 
  src/nmea_msg_analyze.cpp
)
add_dependencies(nmea_analyze_node 
  ${${PROJECT_NAME}_EXPORTED_TARGETS} 
  ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(nmea_analyze_node
  ${catkin_LIBRARIES}
)

add_executable(nmea_odom_node 
  src/nmea_to_odom.cpp
)
add_dependencies(nmea_odom_node 
  ${${PROJECT_NAME}_EXPORTED_TARGETS} 
  ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(nmea_odom_node
  ${catkin_LIBRARIES}
)