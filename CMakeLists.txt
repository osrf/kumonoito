cmake_minimum_required(VERSION 2.8.3)
project(kumonoito)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslint
  ros_drake
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES kumonoito
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_library(jaco_common src/jaco_common.cc
        src/controlled_jaco_simulation.cc
        src/ros_subscriber_system.cc src/ros_publisher_system.cc src/robot_table_planner.cc)
target_link_libraries(jaco_common ${catkin_LIBRARIES})

add_library(ros_joint_state_publisher src/ros_joint_state_publisher.cc)
target_link_libraries(ros_joint_state_publisher
        ${catkin_LIBRARIES})

add_library(ros_tf_publisher src/ros_tf_publisher.cc)
target_link_libraries(ros_tf_publisher
        ${catkin_LIBRARIES})

add_library(ros_publisher_system src/ros_publisher_system.cc)
target_link_libraries(ros_publisher_system
        ${catkin_LIBRARIES})

add_library(ros_subscriber_system src/ros_subscriber_system.cc)
target_link_libraries(ros_subscriber_system
        ${catkin_LIBRARIES})


add_executable(jaco_passive_simulation_demo src/jaco_passive_simulation_demo.cc)
target_link_libraries(jaco_passive_simulation_demo
        jaco_common
        ros_joint_state_publisher
        ${catkin_LIBRARIES})

add_executable(controlled_jaco_simulation_demo src/controlled_jaco_simulation.cc)
target_link_libraries(controlled_jaco_simulation_demo
        jaco_common
        ros_joint_state_publisher
        ros_tf_publisher
        ros_subscriber_system)

# Run QA tool
roslint_cpp()
