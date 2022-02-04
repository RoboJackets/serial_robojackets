cmake_minimum_required(VERSION 3.5)
project(serial)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic")
endif()

find_package(ament_cmake REQUIRED)


## Sources
set(serial_SRCS
src/serial.cc
include/serial/serial.h
include/serial/v8stdint.h
src/impl/unix.cc
src/impl/list_ports/list_ports_linux.cc
)

## Add serial library
add_library(${PROJECT_NAME} ${serial_SRCS})

target_link_libraries(${PROJECT_NAME} rt pthread)

target_include_directories(${PROJECT_NAME}
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

# specific order: dependents before dependencies
ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

ament_package()

## Install executable
install(
    TARGETS ${PROJECT_NAME}
    EXPORT export_${PROJECT_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

## Install headers
install(DIRECTORY include/
  DESTINATION include/)
