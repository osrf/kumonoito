cmake_minimum_required(VERSION 2.8.3)
project(kumonoito)

find_package(drake CONFIG REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  roscpp
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

add_definitions(-std=c++11)

add_executable(kumonoito src/ros_demo_publisher.cc)
target_link_libraries(kumonoito drake::drake ${catkin_LIBRARIES})

add_executable(listener src/ros_demo_listener.cc)
target_link_libraries(listener ${catkin_LIBRARIES})

add_library(jaco_common src/jaco_common.cc)
target_link_libraries(jaco_common drake::drake)

add_library(ros_robot_state_publisher src/ros_robot_state_publisher.cc)
target_link_libraries(ros_robot_state_publisher
        drake::drake
        ${catkin_LIBRARIES})

add_executable(jaco_sim src/jaco_simulation_demo.cc)
target_link_libraries(jaco_sim
        jaco_common
        drake::drake
        ros_robot_state_publisher
        ${catkin_LIBRARIES})
