cmake_minimum_required(VERSION 3.5)
project(mrs_modules_msgs)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CATKIN_DEPENDENCIES
  cmake_modules
  geometry_msgs
  message_generation
  message_runtime
  roscpp
  sensor_msgs
  std_msgs
  std_srvs
  )

find_package(catkin REQUIRED COMPONENTS
  ${CATKIN_DEPENDENCIES}
  )

add_message_files(DIRECTORY msg/sensors FILES
  SensorInfo.msg
  )

add_message_files(DIRECTORY msg/mrs_pcl_tools FILES
  PclToolsDiagnostics.msg
  )

add_message_files(DIRECTORY msg/mrs_octomap_planner FILES
  OctomapPlannerDiagnostics.msg
  )

add_message_files(DIRECTORY msg/mrs_serial FILES
  BacaProtocol.msg
  GimbalPRY.msg
  GripperDiagnostics.msg
  ParachuteDiagnostics.msg
  SerialRaw.msg
  )

add_message_files(DIRECTORY msg/gnss FILES
  Bestpos.msg
  Bestvel.msg
  Gpgga.msg
  Gpgsa.msg
  Gpgst.msg
  Gpgsv.msg
  Gprmc.msg
  GPSFix.msg
  GpsStatus.msg
  Gpvtg.msg
  RangeInformation.msg
  Range.msg
  Satellite.msg
  TersusMessageHeader.msg
  Time.msg
  TrackstatChannel.msg
  Trackstat.msg
  )

add_message_files(DIRECTORY msg/llcp FILES
  Llcp.msg
  )

add_service_files(DIRECTORY srv/llcp FILES

  SetServo.srv
  SetTrigger.srv
  )

generate_messages(DEPENDENCIES

  std_msgs
  geometry_msgs
  sensor_msgs
  )

set(LIBRARIES
  MrsModulesMsgs_Main
  )

catkin_package(
  LIBRARIES ${LIBRARIES}
  CATKIN_DEPENDS ${CATKIN_DEPENDENCIES}
  )

include_directories(
  ${catkin_INCLUDE_DIRS}
  )

add_library(MrsModulesMsgs_Main
  src/main.cpp
  )

add_dependencies(MrsModulesMsgs_Main
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
  )

target_link_libraries(MrsModulesMsgs_Main
  ${catkin_LIBRARIES}
  )

add_custom_command(TARGET MrsModulesMsgs_Main
  PRE_BUILD
  WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
  COMMAND bash -c "${CMAKE_CURRENT_SOURCE_DIR}/link_msg_files.sh"
  )

## --------------------------------------------------------------
## |                           Install                          |
## --------------------------------------------------------------

install(TARGETS ${LIBRARIES}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
  )

install(DIRECTORY msg srv
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  )
