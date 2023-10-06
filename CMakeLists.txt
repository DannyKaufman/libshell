cmake_minimum_required(VERSION 3.16)

project(libshell)

file(GLOB LIBFILES include/*.h src/*.cpp src/SecondFundamentalForm/*.cpp src/MaterialModel/*.cpp)
add_library(libshell SHARED ${LIBFILES})

target_include_directories(libshell PUBLIC include)

include(libigl)

target_link_libraries(libshell PUBLIC igl::core)

set_target_properties(libshell PROPERTIES POSITION_INDEPENDENT_CODE ON)
