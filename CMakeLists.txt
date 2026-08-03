cmake_minimum_required(VERSION 3.5)
project(mcl_3dl_msgs)

set(MSG_FILES Status.msg)
set(SRV_FILES ResizeParticle.srv LoadPCD.srv)

find_package(catkin QUIET)

if(catkin_FOUND)
  # ##########################################################################
  # ROS1 (catkin)
  # ##########################################################################
  find_package(catkin REQUIRED COMPONENTS message_generation std_msgs sq_ros1_rclcpp_compat)

  add_message_files(FILES ${MSG_FILES})
  add_service_files(FILES ${SRV_FILES})
  generate_messages(DEPENDENCIES std_msgs)

  catkin_package(CATKIN_DEPENDS message_runtime std_msgs)

  # Generate ROS 2-style compat headers for ROS 1 builds
  generate_ros1_compat_headers(
    PACKAGE "${PROJECT_NAME}"
    MSG_DIR "${CMAKE_CURRENT_SOURCE_DIR}/msg"
    OUTPUT_DIR "${CATKIN_DEVEL_PREFIX}/include"
  )

else()
  # ##########################################################################
  # ROS2 (ament_cmake)
  # ##########################################################################
  find_package(ament_cmake_auto REQUIRED)
  ament_auto_find_build_dependencies()

  rosidl_generate_interfaces(${PROJECT_NAME}
    "msg/${MSG_FILES}"
    "srv/ResizeParticle.srv"
    "srv/LoadPCD.srv"
    DEPENDENCIES std_msgs
  )

  if(BUILD_TESTING)
    find_package(ament_lint_auto REQUIRED)
    # CPP_FORMATTER=clang_format: use clang-format instead of uncrustify
    list(APPEND AMENT_LINT_AUTO_EXCLUDE ament_cmake_uncrustify)
    # The upstream LICENSE text is not recognized by ament_copyright and the
    # package has no CONTRIBUTING.md; the license text is kept as-is.
    list(APPEND AMENT_LINT_AUTO_EXCLUDE ament_cmake_copyright)
    ament_lint_auto_find_test_dependencies()
  endif()

  ament_auto_package()

endif()
