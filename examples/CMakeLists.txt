cmake_minimum_required(VERSION 3.0)
project(example)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})
find_package(OpenGL REQUIRED)
find_package(GLFW3 REQUIRED)

set(header third_party example.hpp)
set(library realsense ${GLFW3_LIBRARY} ${OPENGL_LIBRARIES})

file(GLOB APP_SOURCE *.cpp *.c)

foreach (cxx ${APP_SOURCE})
    get_filename_component(cxx ${cxx} NAME)
    string(REPLACE ".cpp" "" bin ${cxx})
    string(REPLACE ".c"   "" bin ${bin})
    add_executable(${bin} ${cxx} ${header})
    target_link_libraries(${bin} ${library})
endforeach ()
