cmake_minimum_required(VERSION 2.8.3)
project(leishen_lslidar_c16_driver)

add_definitions(-std=c++0x)


find_package(catkin REQUIRED COMPONENTS
  roscpp
  diagnostic_updater
  nodelet

  lslidar_c16_msgs
)

find_package(Boost REQUIRED)

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES lslidar_c16_driver
  CATKIN_DEPENDS
    roscpp diagnostic_updater nodelet
    lslidar_c16_msgs
  DEPENDS
    boost
)

include_directories(
  include
  ${Boost_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)

link_directories(
  ${catkin_LIBRARY_DIRS}
)

# Leishen c16 lidar driver
add_library(leishen_lslidar_c16_driver
  src/lslidar_c16_driver.cc
)
target_link_libraries(leishen_lslidar_c16_driver
  ${catkin_LIBRARIES}
)
add_dependencies(leishen_lslidar_c16_driver
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

# Leishen c16 lidar nodelet
add_executable(leishen_lslidar_c16_driver_node
  src/lslidar_c16_driver_node.cc
)
target_link_libraries(leishen_lslidar_c16_driver_node
  leishen_lslidar_c16_driver
  ${catkin_LIBRARIES}
)
add_dependencies(leishen_lslidar_c16_driver_node
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

# Leishen c16 lidar nodelet
add_library(leishen_lslidar_c16_driver_nodelet
  src/lslidar_c16_driver_nodelet.cc
)
target_link_libraries(leishen_lslidar_c16_driver_nodelet
  leishen_lslidar_c16_driver
  ${catkin_LIBRARIES}
)
add_dependencies(leishen_lslidar_c16_driver_nodelet
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)


# install options
install(
    TARGETS 
    leishen_lslidar_c16_driver
    leishen_lslidar_c16_driver_node
    leishen_lslidar_c16_driver_nodelet
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
install(FILES nodelet_lslidar_c16.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)
