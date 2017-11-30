cmake_minimum_required(VERSION 2.8.3)
project(kumonoito)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  ros_drake
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES kumonoito
  CATKIN_DEPENDS roscpp
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(kumonoito src/ros_demo_publisher.cc)
target_link_libraries(kumonoito ${catkin_LIBRARIES})

add_executable(listener src/ros_demo_listener.cc)
target_link_libraries(listener ${catkin_LIBRARIES})

add_library(jaco_common src/jaco_common.cc)
target_link_libraries(jaco_common ${catkin_LIBRARIES})

add_library(ros_robot_state_publisher src/ros_robot_state_publisher.cc)
target_link_libraries(ros_robot_state_publisher
        ${catkin_LIBRARIES})

add_executable(jaco_sim src/jaco_simulation_demo.cc)
target_link_libraries(jaco_sim
        jaco_common
        ros_robot_state_publisher
        ${catkin_LIBRARIES})
