cmake_minimum_required(VERSION 2.8.3)
project(eddy_hardware)
add_compile_options(-std=c++11)

# NOTE: Most dependencies can be taken care of with $(catkin_EXPORTED_TARGETS).
# There is no need to add mention the specific dependency, such as
# <package_name>_gencpp, or <package_name>_gencfg

# NOTE: To include OpenCV, must do find_package(OpenCV REQUIRED), followed by
# include_directories(...)

find_package(catkin REQUIRED
    COMPONENTS
    roslint
    roscpp
    ##eddy_msgs
    geometry_msgs
    imu_3dm_gx4
    pointgrey_camera_driver
    message_filters
    tf
    message_generation
    sensor_msgs
    cv_bridge
)

##generate_messages()

catkin_package(INCLUDE_DIRS include)
find_package(OpenCV REQUIRED) # To include, OpenCV, this MUST go BEFORE include_directories()
include_directories(include ${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS})

roslint_cpp()

# include boost
find_package(Boost REQUIRED)
include_directories(${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIR})

# add_executable(depth_processor src/depth_processor.cpp)
# target_link_libraries(depth_processor ${catkin_LIBRARIES})
# add_dependencies(depth_processor ${catkin_EXPORTED_TARGETS})

add_executable(calibrate_camera src/calibrate_camera.cpp)
target_link_libraries(calibrate_camera ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
add_dependencies(calibrate_camera ${catkin_EXPORTED_TARGETS})

add_executable(undistort_camera src/undistort_camera.cpp)
target_link_libraries(undistort_camera ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
add_dependencies(undistort_camera ${catkin_EXPORTED_TARGETS})

# add_executable(imu_processor src/imu_processor.cpp)
# target_link_libraries(imu_processor ${catkin_LIBRARIES})
# add_dependencies(imu_processor ${catkin_EXPORTED_TARGETS})
