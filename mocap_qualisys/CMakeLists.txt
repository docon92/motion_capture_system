cmake_minimum_required(VERSION 2.8.3)
project(mocap_qualisys)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")

find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  message_generation
  roscpp
  geometry_msgs
  tf
  eigen_conversions
  tf_conversions
  mocap_base
)


catkin_package(
  #INCLUDE_DIRS include
  #LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS
    geometry_msgs tf mocap_base
  DEPENDS
  Boost Eigen3
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
)

add_definitions("-std=c++0x -Wall")


# QUALISYS
add_library(mocap_qualisys_driver
  src/QualisysDriver.cpp
  include/mocap_qualisys/RTProtocol.cpp
  include/mocap_qualisys/NBC_Markup.cpp
  include/mocap_qualisys/RTPacket.cpp
  include/mocap_qualisys/Network.cpp
)
target_link_libraries(mocap_qualisys_driver
  ${catkin_LIBRARIES}
)
add_dependencies(mocap_qualisys_driver
  ${${PROJECT_NAME}_EXPORTED_TARGETS}

  ${catkin_EXPORTED_TARGETS}
)

add_executable(mocap_qualisys_node
  src/qualisys.cpp
)
target_link_libraries(mocap_qualisys_node
  mocap_qualisys_driver
  ${catkin_LIBRARIES}
)

