cmake_minimum_required(VERSION 3.8)
project(cpp_pubsub)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_srvs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "srv/CustomSrv.srv"
  DEPENDENCIES std_msgs geometry_msgs
 )
ament_export_dependencies(rosidl_default_runtime)


add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_srvs)

add_executable(listener src/subscriber_member_function_custom_srv.cpp)
ament_target_dependencies(listener rclcpp std_srvs)

rosidl_target_interfaces(listener
  ${PROJECT_NAME} "rosidl_typesupport_cpp")

install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})

install(
  FILES srv/my_mapping_rules.yaml
  DESTINATION share/${PROJECT_NAME}/srv)


ament_package()
