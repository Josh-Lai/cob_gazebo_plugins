cmake_minimum_required(VERSION 3.5)
project(cob_gazebo_ros_control)

# add_definitions(-std=c++11)

#ROS2 MIGRATION ADDITIONS
set(CMAKE_CXX_STANDARD 14)
find_package(rosidl_default_generators REQUIRED)


#Find Dependencies
find_package(ament_cmake REQUIRED)
# find_package(catkin REQUIRED COMPONENTS controller_manager gazebo_ros gazebo_ros_control hardware_interface joint_limits_interface pluginlib roscpp transmission_interface urdf)
find_package(controller_manager REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(gazebo_ros2_control REQUIRED)
find_package(hardware_interface REQUIRED)
# find_package(joint_limits_interface REQUIRED)
find_package(pluginlib REQUIRED)
find_package(rclcpp REQUIRED)
# find_package(transmission_interface REQUIRED)
find_package(urdf REQUIRED)

find_package(gazebo REQUIRED)

# catkin_package(
#   CATKIN_DEPENDS controller_manager gazebo_ros gazebo_ros_control hardware_interface joint_limits_interface pluginlib roscpp transmission_interface urdf
#   #DEPENDS gazebo
#   INCLUDE_DIRS include
#   LIBRARIES hwi_switch_gazebo_ros_control hwi_switch_robot_hw_sim
# )

ament_export_dependencies(controller_manager gazebo_ros gazebo_ros_control hardware_interface joint_limits_interface pluginlib roscpp transmission_interface urdf)
ament_export_include_directories(include)
ament_export_libraries(hwi_switch_gazebo_ros_control hwi_switch_robot_hw_sim)

### BUILD ###
include_directories(include ${catkin_INCLUDE_DIRS} ${GAZEBO_INCLUDE_DIRS})

add_library(hwi_switch_robot_hw_sim src/hwi_switch_robot_hw_sim.cpp)
target_link_libraries(hwi_switch_robot_hw_sim ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES})

add_library(hwi_switch_gazebo_ros_control src/hwi_switch_gazebo_ros_control_plugin.cpp)
target_link_libraries(hwi_switch_gazebo_ros_control hwi_switch_robot_hw_sim ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES})

### INSTALL ###
install(TARGETS hwi_switch_gazebo_ros_control hwi_switch_robot_hw_sim
  LIBRARY DESTINATION lib/${PROJECT_NAME}
)

install(FILES ${PROJECT_NAME}_plugins.xml
  DESTINATION share/${PROJECT_NAME}
)

ament_package()