cmake_minimum_required (VERSION 2.6)
project (RPG)
# The version number.
set (RPG_VERSION_MAJOR 1)
set (RPG_VERSION_MINOR 0)
 

option(USE_CLANG "build application with clang" OFF) # OFF is the default
if(USE_CLANG)
    SET(CMAKE_CXX_COMPILER  "/usr/bin/clang++")
endif(USE_CLANG)

add_definitions(-std=c++14 -O2 -Wall -Wextra -pedantic)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
# Own stuff
FILE(GLOB_RECURSE SOURCE_FILES "${CMAKE_SOURCE_DIR}/src/*.cpp")
FILE(GLOB_RECURSE HEADER_FILES "${CMAKE_HEADER_DIR}/src/*.h")
FILE(GLOB_RECURSE SHADER_FILES "${CMAKE_SOURCE_DIR}/src/shader/*.*")
# GLFW
#add_subdirectory(external/glfw-3.1.2)
#include_directories(external/glfw-3.1.2/include)
find_package(PkgConfig REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)
if(NOT GLFW_FOUND)
    message(FATAL_ERROR "GLFW not found")
endif(NOT GLFW_FOUND)
include_directories(${GLFW_INCLUDE_DIRS})
#target_link_libraries(${PROJECT_NAME} ${GLFW_LIBRARIES})

# GLEW
#set(GLEW external/glew-1.13.0)
#find_path(GLEW glew.h)
#find_library(GLEW GLEW_LIBRARIES)
#include_directories(${GLEW})
#find_package(GLEW REQUIRED)
#if(NOT GLEW_FOUND)
#    message(FATAL_ERROR "GLEW not found!")
#endif(NOT GLEW_FOUND)
#include_directories(${GLEW_INCLUDE_DIRS})
#target_link_libraries(${PROJECT_NAME} ${GLEW_LIBRARIES})

# Set additional libs
#set( ADDITIONAL_LIBS 
#     -pthread
#     -lrt
#     -lm
#     -lXrandr
#     -lXinerama
#     -lX11
#    -lGL
#    -lXxf86vm
#    -ldl
#    -lXi
#    -lXcursor
#)

# Combine
set(LIBRARIES ${GLFW_LIBRARIES}) #${ADDITIONAL_LIBS})

add_executable(rpg ${SOURCE_FILES} ${HEADER_FILES} ${SHADER_FILES})
target_link_libraries(rpg ${LIBRARIES} )
