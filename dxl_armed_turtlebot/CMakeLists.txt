cmake_minimum_required(VERSION 2.8.3)
project(dxl_armed_turtlebot)

find_package(catkin REQUIRED COMPONENTS roseus) # add roseus to gen messages

catkin_package()

if(CATKIN_ENABLE_TESTING)
  find_package(catkin REQUIRED COMPONENTS rostest roslaunch)
  # roslacunch check
  file(GLOB LAUNCH_FILES launch/*.launch)
  foreach(LAUNCH_FILE ${LAUNCH_FILES})
    if("${LAUNCH_FILE}" STREQUAL "${CMAKE_CURRENT_SOURCE_DIR}/launch/dxl_armed_turtlebot_bringup.launch")
      find_package(freenect_launch QUIET)
      if(NOT freenect_launch_FOUND)
        continue()
      endif()
    endif()
    roslaunch_add_file_check(${LAUNCH_FILE})
  endforeach()
  # test
  find_package(pr2eus QUIET)
  if(pr2eus_FOUND)
    add_rostest(test/test-dxl-armed-turtlebot.test)
  endif()
endif()
