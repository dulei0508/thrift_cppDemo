cmake_minimum_required(VERSION 3.5)

project(Tutorial)

include_directories("${CMAKE_CURRENT_BINARY_DIR}")
include_directories("${CMAKE_CURRENT_BINARY_DIR}/src/gen-cpp")
include_directories("${CMAKE_CURRENT_BINARY_DIR}/out")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra -pedantic -O3 -Wno-long-long -fno-omit-frame-pointer")
link_directories("/usr/local/lib")
link_libraries(thrift)

set(tutorialgencpp_SOURCES
     src/gen-cpp/Calculator.cpp
     src/gen-cpp/SharedService.cpp
     src/gen-cpp/shared_constants.cpp
     src/gen-cpp/shared_types.cpp
     src/gen-cpp/tutorial_constants.cpp
     src/gen-cpp/tutorial_types.cpp
 )

add_library(tutorialgencpp STATIC ${tutorialgencpp_SOURCES})

add_executable(TutorialServer ../src/cppServer.cpp)
target_link_libraries(TutorialServer libthrift.a)
target_link_libraries(TutorialServer libthriftnb.a)
target_link_libraries(TutorialServer tutorialgencpp)

add_executable(TutorialClient ../src/cppClient.cpp)
#target_link_libraries(TutorialServer thrift)
target_link_libraries(TutorialClient tutorialgencpp)
