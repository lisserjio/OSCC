set(CMAKE_TOOLCHAIN_FILE cmake/ArduinoToolchain.cmake) # Arduino Toolchain

cmake_minimum_required(VERSION 2.8)

project(Steering C CXX)

print_board_list()
print_programmer_list()

set(ARDUINO_DEFAULT_BOARD uno)

set(ARDUINO_DEFAULT_PORT /dev/ttyACM0)

include_directories(
	${CMAKE_SOURCE_DIR}/../../arduino_libraries/can_frame
	${CMAKE_SOURCE_DIR}/../../arduino_libraries/control_protocol_can
	${CMAKE_SOURCE_DIR}/../../arduino_libraries/PID
	${CMAKE_SOURCE_DIR}/../../arduino_libraries/common
	${CMAKE_SOURCE_DIR}/../../arduino_libraries/DAC_MCP49xx
	${CMAKE_SOURCE_DIR}/../../arduino_libraries/mcp_can
	${CMAKE_SOURCE_DIR}/include)

set(steering_global_lib_SRCS ${CMAKE_SOURCE_DIR}/src/globals.cpp)
set(steering_global_lib_HDRS ${CMAKE_SOURCE_DIR}/include/globals.h)
set(steering_global_lib_BOARD uno)

generate_arduino_library(steering_global_lib)

set(steering_control_lib_SRCS ${CMAKE_SOURCE_DIR}/src/steering_control.cpp)
set(steering_control_lib_HDRS ${CMAKE_SOURCE_DIR}/include/steering_control.h)
set(steering_control_lib_BOARD uno)

generate_arduino_library(steering_control_lib)

set(steering_control_test_lib_SRCS ${CMAKE_SOURCE_DIR}/src/test_steering_control.cpp)
set(steering_control_test_lib_HDRS ${CMAKE_SOURCE_DIR}/include/test_steering_control.h)
set(steering_control_test_lib_BOARD uno)

generate_arduino_library(steering_control_test_lib)

set( steering_control_module_SKETCH
	${CMAKE_SOURCE_DIR}/steering_control_module.ino )

set(steering_control_module_LIBS steering_global_lib
								 steering_control_lib
								 steering_control_test_lib)

generate_arduino_firmware( steering_control_module )