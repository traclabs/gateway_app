project(ROVER_APP C)

include_directories(fsw/mission_inc)
include_directories(fsw/platform_inc)
include_directories(fsw/edoras_conversion)

include_directories(/home/ana/ros2/edoras_ws/edoras/src/scratch/c_serialize_udp)
link_directories(/home/ana/ros2/edoras_ws/edoras/src/scratch/c_serialize_udp/build)
#include_directories(${ros_app_MISSION_DIR}/fsw/platform_inc)

# Create library for talking to robot using sockets
add_library(robot_comm_udp_test fsw/src/robot_comm_udp_test.c)

add_subdirectory(fsw/edoras_conversion)

# Create the app module
add_cfe_app(gateway_app fsw/src/gateway_app.c)
target_link_libraries(gateway_app 
                      m 
                      serialize_library 
                      robot_comm_udp_test 
                      edoras_conversion)

target_include_directories(gateway_app PUBLIC
  fsw/mission_inc
  fsw/platform_inc
  fsw/edoras_conversion
)

get_target_property(target_options gateway_app COMPILE_OPTIONS)
list(REMOVE_ITEM target_options "-pedantic")
set_property(TARGET gateway_app PROPERTY COMPILE_OPTIONS )#${target_options})

