cmake_minimum_required(VERSION 3.4 FATAL_ERROR)
project(fileserver-task CXX)

option(BUILD_TESTS "Build tests" ON)

# Собираем с флагами для c++11
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (CMAKE_BUILD_TYPE EQUAL "Debug")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wnon-virtual-dtor -Wshadow -Wold-style-cast -Wcast-align -Wunused -Woverloaded-virtual -pedantic")
endif()

add_subdirectory(libs/cppformat)
add_subdirectory(libs/spdlog)

find_package(Boost 1.58 REQUIRED COMPONENTS filesystem system)
find_package(Threads)

set(SOURCE_FILES main.cpp)
add_executable(fileserver-task ${SOURCE_FILES})
target_link_libraries(fileserver-task
        spdlog
        ${Boost_LIBRARIES}
        Threads::Threads
)


if (BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()