cmake_minimum_required(VERSION 3.20)
project(kinematics)

# C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

### libIGL options: choose between header only and compiled static library
option(LIBIGL_USE_STATIC_LIBRARY     "Use libigl as static library" OFF)
option(LIBIGL_GLFW                   "Build target igl::glfw" ON)

include(libigl)

# Use solution files or src files
option(USE_SOLUTION "Use solution files" OFF)

set(SRCFILES "${CMAKE_CURRENT_SOURCE_DIR}/src")
if(USE_SOLUTION)
  set(SRCFILES "${CMAKE_CURRENT_SOURCE_DIR}/solution")
endif()
file(GLOB SRCFILES CONFIGURE_DEPENDS ${SRCFILES}/*.cpp)

# Add executable
add_executable(${PROJECT_NAME} "main.cpp" ${SRCFILES})

# Include project headers
target_include_directories(${PROJECT_NAME}
  PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/include/"
)

# Include JSON
target_include_directories(${PROJECT_NAME}
  SYSTEM PRIVATE
  "${CMAKE_CURRENT_SOURCE_DIR}/json/"
)

# Link libraries
target_link_libraries(${PROJECT_NAME}
  PRIVATE
    igl::core
    igl::glfw
)

