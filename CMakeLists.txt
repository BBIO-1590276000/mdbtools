cmake_minimum_required(VERSION 3.25)

execute_process(
    COMMAND git rev-list --count HEAD
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_COMMIT_COUNT
    OUTPUT_STRIP_TRAILING_WHITESPACE
    RESULT_VARIABLE GIT_RESULT
)

if(NOT GIT_RESULT EQUAL 0)
    set(GIT_COMMIT_COUNT 0)
endif()

message(STATUS "Commit count: ${GIT_COMMIT_COUNT}")

project("MDB Tools" VERSION 1.0.1 LANGUAGES C)

option(BUILD_LIBRARY "Build the MDB library" ON)

if(EXISTS "${CMAKE_SOURCE_DIR}/mestrelab")
    set(MESTRELAB_DIR "${CMAKE_SOURCE_DIR}/mestrelab")
    message(STATUS "MESTRELAB_DIR found: ${MESTRELAB_DIR}")
else()
    message(STATUS "mestrelab directory not found")
endif()

if (BUILD_LIBRARY)
    add_subdirectory(src/libmdb)
endif()