cmake_minimum_required(VERSION 3.20)
project(vstream_detection_example_cpp)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(COMPILE_OPTIONS -Wall -Wextra -Wconversion -O3 -fconcepts -Wno-ignored-qualifiers -Wno-extra -Wno-stringop-truncation -Wno-reorder)

find_package(Threads)
find_package(HailoRT REQUIRED)
find_package(OpenCV REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBCAMERA libcamera)
if (LIBCAMERA_FOUND)
	message(STATUS "libcamera library found:")
	message(STATUS "    version: ${LIBCAMERA_VERSION}")
	message(STATUS "    libraries: ${LIBCAMERA_LINK_LIBRARIES}")
	message(STATUS "    include path: ${LIBCAMERA_INCLUDE_DIRS}")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DENABLE_LIBCAMERA")
else()
    message(FATAL_ERROR "ERROR: libcamera is not found!!")
endif()

message(STATUS "Found OpenCV: " ${OpenCV_INCLUDE_DIRS})

# file(GLOB SOURCES
# ./*.cpp
# ./common/*.cpp
# )

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-overloaded-virtual -Wno-array-parameter -Wno-unused-but-set-variable -Wno-unused-variable -Wextra -Wconversion -fconcepts -Wno-ignored-qualifiers -Wno-extra -Wno-stringop-truncation -Wno-reorder")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -O0 -g -fsanitize=address,undefined")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O3 -DNDEBUG")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS} -O3 -g -DNDEBUG")

link_libraries(stdc++fs)
add_executable(${PROJECT_NAME} main.cpp common/yolo_hailortpp.cpp LibCamera.cpp)
include_directories(${OpenCV_INCLUDE_DIRS} ${LIBCAMERA_INCLUDE_DIRS})
target_compile_options(${PROJECT_NAME} PRIVATE ${COMPILE_OPTIONS})
target_link_libraries(${PROJECT_NAME} Threads::Threads HailoRT::libhailort)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} ${LIBCAMERA_LINK_LIBRARIES})
