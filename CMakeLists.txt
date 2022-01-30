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

## Include headers
include_directories(include)

## Sources
set(serial_SRCS
    src/serial.cc
    src/impl/unix.cc
    src/impl/list_ports/list_ports_linux.cc
    include/serial/serial.h
    include/serial/v8stdint.h
)

## Add serial library
add_library(${PROJECT_NAME} ${serial_SRCS})
target_link_libraries(${PROJECT_NAME} rt pthread)

## Install executable
install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

## Install headers
install(FILES include/serial/serial.h include/serial/v8stdint.h
  DESTINATION include)

## Tests
if(COLCON_ENABLE_TESTING)
    add_subdirectory(tests)
endif()

ament_package()
