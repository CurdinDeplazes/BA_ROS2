cmake_minimum_required(VERSION 3.8)
project(ROS2_force_control)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(CMAKE_BUILD_TYPE Release)

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(pluginlib REQUIRED)
find_package(rclcpp REQUIRED)
find_package(controller_interface REQUIRED)
find_package(dynamic_reconfigure REQUIRED)

#set two variables for packages
set(REF_GEN_INCLUDE_DEPENDS
  rclcpp
  Eigen3
)

set(CONTROLLER_INCLUDE_DEPENDS
  controller_interface
  franka_msgs
  franka_semantic_components
  pinocchio
  rclcpp_action
  rcpputils
  realtime_tools
  trajectory_msgs
)

#find packages from previously defined variables
find_package(ament_cmake REQUIRED)
foreach(Dependency IN ITEMS ${REF_GEN_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()
foreach(Dependency IN ITEMS ${CONTROLLER_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()

# find dependencies
find_package(ament_cmake REQUIRED)
foreach(Dependency IN ITEMS ${REF_GEN_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()
foreach(Dependency IN ITEMS ${CONTROLLER_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()

ament_target_dependencies(
  cartestian_impedance_controller
  franka_semantic_components
  franka_msgs
  rclcpp_action
  ${CONTROLLER_INCLUDE_DEPENDS}
  ${REG_GEN_INCLUDE_DEPENDS}
)




add_library(
  cartesian_impedance_controller
  SHARED
  src/cartesian_impedance_controller.cpp
  src/callbacks.cpp
  src/CartesianImpedanceController_helpers.cpp
  src/equilibrium_pose_server.cpp
  src/force_action_controller.cpp
  src/ForceActionController_helpers.cpp
)

target_include_directories(ROS2_force_control PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/include
)

#find_pakage()
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
