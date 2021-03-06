cmake_minimum_required(VERSION 2.8.3)
project(elas_ros)

set(ELAS_DEPS roscpp image_transport cv_bridge image_geometry nav_msgs message_filters tf pcl_ros geometry_msgs)
find_package(catkin REQUIRED COMPONENTS ${ELAS_DEPS})
find_package(OpenCV REQUIRED)

catkin_package(
  CATKIN_DEPENDS roscpp
  INCLUDE_DIRS   libelas/src
  LIBRARIES      elas
)
include_directories(${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} libelas/src)

# the library
SET(CMAKE_CXX_FLAGS "-msse3")
add_library(elas
  libelas/src/descriptor.cpp
  libelas/src/elas.cpp
  libelas/src/filter.cpp
  libelas/src/matrix.cpp
  libelas/src/triangle.cpp)

# the exes
set(ELAS_TARGETS elas_ros)

foreach (target ${ELAS_TARGETS})
  add_executable(${PROJECT_NAME}_${target} src/${target}.cpp)
  set_target_properties(${PROJECT_NAME}_${target} PROPERTIES OUTPUT_NAME ${target} PREFIX "")
  target_link_libraries(${PROJECT_NAME}_${target} ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
endforeach (target)

target_link_libraries(${PROJECT_NAME}_elas_ros elas)
