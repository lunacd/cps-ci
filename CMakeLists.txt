cmake_minimum_required(VERSION 3.25)
project(
  cps-config
  LANGUAGES CXX
  VERSION 0.0.1)

add_subdirectory(src)

include(CTest)
if(BUILD_TESTING)
    enable_testing()
    add_subdirectory(tests)
endif()
