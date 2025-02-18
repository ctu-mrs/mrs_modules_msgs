cmake_minimum_required(VERSION 3.5)
project(mrs_modules_msgs)

set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(ament_cmake REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)
find_package(std_srvs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)

# these files are auto-generated using the copy script
include(msg_list.txt)
include(srv_list.txt)

rosidl_generate_interfaces(${PROJECT_NAME}
    ${msg_files}
    ${srv_files}
    DEPENDENCIES builtin_interfaces geometry_msgs std_msgs sensor_msgs std_srvs
  )

# add_custom_command(TARGET MrsModulesMsgs_Main
#   PRE_BUILD
#   WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
#   COMMAND bash -c "${CMAKE_CURRENT_SOURCE_DIR}/link_msg_files.sh"
#   )

## --------------------------------------------------------------
## |                           Install                          |
## --------------------------------------------------------------

# install(TARGETS ${PROJECT_NAME}
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
#   )

# install(DIRECTORY msg srv
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#   )

ament_package()