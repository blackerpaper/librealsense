cmake_minimum_required(VERSION 3.4)
project(realsense)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})
find_package(libUSB REQUIRED)

file(GLOB_RECURSE ${PROJECT_NAME}.Sources "src/*.*")
file(GLOB_RECURSE ${PROJECT_NAME}.Headers "include/*.*")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME}.Sources} ${${PROJECT_NAME}.Headers})

target_include_directories(${PROJECT_NAME}
        PUBLIC include
        PUBLIC ${LIBUSB_1_INCLUDE_DIRS}
        )

target_link_libraries(${PROJECT_NAME}
        ${LIBUSB_1_LIBRARIES}
        )

if (APPLE)
    add_definitions(-DRS_USE_LIBUVC_BACKEND)
elseif (WIN32)
    add_definitions(-DRS_USE_WMF_BACKEND)
else ()
    add_definitions(-DRS_USE_V4L2_BACKEND)
endif ()

add_subdirectory(examples)