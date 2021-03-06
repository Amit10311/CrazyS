cmake_minimum_required(VERSION 2.8.3)
project(rotors_control)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  mav_msgs
  nav_msgs
  roscpp
  sensor_msgs
  cmake_modules
)

find_package(
  Eigen REQUIRED
)

catkin_package(
  INCLUDE_DIRS include ${Eigen_INCLUDE_DIRS}
  LIBRARIES lee_position_controller position_controller crazyflie_onboard_controller roll_pitch_yawrate_thrust_controller sensfusion6 crazyflie_complementary_filter
  roll_pitch_yawrate_thrust_crazyflie
  CATKIN_DEPENDS geometry_msgs mav_msgs nav_msgs roscpp sensor_msgs
  DEPENDS Eigen
)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

add_library(lee_position_controller
  src/library/lee_position_controller.cpp
)

add_library(position_controller
   src/library/position_controller.cpp
)

add_library(crazyflie_onboard_controller
   src/library/crazyflie_onboard_controller.cpp
)

add_library(roll_pitch_yawrate_thrust_controller
  src/library/roll_pitch_yawrate_thrust_controller.cpp
)

add_library(roll_pitch_yawrate_thrust_crazyflie
  src/library/roll_pitch_yawrate_thrust_crazyflie.cpp
)

add_library(crazyflie_complementary_filter
  src/library/crazyflie_complementary_filter.cpp
)

add_library(sensfusion6
  src/library/sensfusion6.cpp
)

target_link_libraries(lee_position_controller ${catkin_LIBRARIES})
add_dependencies(lee_position_controller ${catkin_EXPORTED_TARGETS})

target_link_libraries(position_controller ${catkin_LIBRARIES})
add_dependencies(position_controller ${catkin_EXPORTED_TARGETS})

target_link_libraries(roll_pitch_yawrate_thrust_controller ${catkin_LIBRARIES})
add_dependencies(roll_pitch_yawrate_thrust_controller ${catkin_EXPORTED_TARGETS})

target_link_libraries(roll_pitch_yawrate_thrust_crazyflie ${catkin_LIBRARIES})
add_dependencies(roll_pitch_yawrate_thrust_crazyflie ${catkin_EXPORTED_TARGETS})

target_link_libraries(crazyflie_onboard_controller ${catkin_LIBRARIES})
add_dependencies(crazyflie_onboard_controller ${catkin_EXPORTED_TARGETS})

target_link_libraries(crazyflie_complementary_filter ${catkin_LIBRARIES})
add_dependencies(crazyflie_complementary_filter ${catkin_EXPORTED_TARGETS})

target_link_libraries(sensfusion6 ${catkin_LIBRARIES})
add_dependencies(sensfusion6 ${catkin_EXPORTED_TARGETS})

add_executable(lee_position_controller_node src/nodes/lee_position_controller_node.cpp)
add_dependencies(lee_position_controller_node ${catkin_EXPORTED_TARGETS})
target_link_libraries(lee_position_controller_node
  lee_position_controller ${catkin_LIBRARIES})

add_executable(position_controller_node src/nodes/position_controller_node.cpp)
add_dependencies(position_controller_node ${catkin_EXPORTED_TARGETS})
target_link_libraries(position_controller_node
  position_controller crazyflie_complementary_filter crazyflie_onboard_controller sensfusion6 ${catkin_LIBRARIES})

add_executable(roll_pitch_yawrate_thrust_controller_node
  src/nodes/roll_pitch_yawrate_thrust_controller_node.cpp)
add_dependencies(roll_pitch_yawrate_thrust_controller_node ${catkin_EXPORTED_TARGETS})
target_link_libraries(roll_pitch_yawrate_thrust_controller_node
  roll_pitch_yawrate_thrust_controller ${catkin_LIBRARIES})

add_executable(roll_pitch_yawrate_thrust_crazyflie_node
  src/nodes/roll_pitch_yawrate_thrust_crazyflie_node.cpp)
add_dependencies(roll_pitch_yawrate_thrust_crazyflie_node ${catkin_EXPORTED_TARGETS})
target_link_libraries(roll_pitch_yawrate_thrust_crazyflie_node
roll_pitch_yawrate_thrust_crazyflie ${catkin_LIBRARIES})


install(TARGETS lee_position_controller position_controller crazyflie_onboard_controller roll_pitch_yawrate_thrust_controller
  roll_pitch_yawrate_thrust_crazyflie
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(TARGETS lee_position_controller_node position_controller_node roll_pitch_yawrate_thrust_controller_node
  roll_pitch_yawrate_thrust_crazyflie_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)
