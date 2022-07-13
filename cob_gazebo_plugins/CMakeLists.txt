cmake_minimum_required(VERSION 3.0.2)
set(CMAKE_CXX_STANDARD 14)
project(cob_gazebo_plugins)
find_package(cob_gazebo_ros_control REQUIRED)
find_package(ament_cmake REQUIRED)
ament_target_dependencies(cob_gazebo_ros_control)

ament_package()