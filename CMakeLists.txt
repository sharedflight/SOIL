
cmake_minimum_required(VERSION 3.5)

project (SOIL)

set(CMAKE_CXX_STANDARD 11)

set (SOIL_BUILD_TESTS false CACHE BOOL "Build tests")

if(APPLE)
    set(CMAKE_OSX_DEPLOYMENT_TARGET "10.11")
    set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
endif(APPLE)

if(ANDROID)
  # Android requires GL ES 2.0 package automatically
  set(LIBRARIES GLESv2)
else()
  find_package (OpenGL REQUIRED)
  include_directories (${OPENGL_INCLUDE_DIR})
  set(LIBRARIES ${OPENGL_LIBRARIES})
endif()

file (GLOB SOIL_SOURCES src/*.c)
file (GLOB SOIL_HEADERS src/*.h)

add_library (SOIL ${SOIL_SOURCES} ${SOIL_HEADERS})
target_link_libraries (SOIL ${LIBRARIES})

install (TARGETS SOIL
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)
install (FILES ${SOIL_HEADERS} DESTINATION include/SOIL)

if (SOIL_BUILD_TESTS)
    add_executable (test_SOIL src/test_SOIL.cpp)
    target_link_libraries (test_SOIL SOIL)
    install (TARGETS SOIL test_SOIL RUNTIME DESTINATION bin)
endif ()