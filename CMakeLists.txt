cmake_minimum_required(VERSION 3.25)

execute_process(
    COMMAND git rev-list --count HEAD
    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
    OUTPUT_VARIABLE GIT_COMMIT_COUNT
    OUTPUT_STRIP_TRAILING_WHITESPACE
    RESULT_VARIABLE GIT_RESULT
)

if(NOT GIT_RESULT EQUAL 0)
    set(GIT_COMMIT_COUNT 0)
endif()

message(STATUS "Commit count: ${GIT_COMMIT_COUNT}")

if (CMAKE_INSTALL_PREFIX)
    set(INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/$<CONFIG>" CACHE PATH "Installation directory" FORCE)
else()
    set(INSTALL_DIR "${CMAKE_CURRENT_LIST_DIR}/install/${CMAKE_SYSTEM_NAME}/$<CONFIG>" CACHE PATH "Installation directory" FORCE)
endif()
message(STATUS "INSTALL_DIR set to: ${INSTALL_DIR}")

project("MDB Tools" VERSION 1.0.1 LANGUAGES C)

option(BUILD_LIBRARY "Build the MDB library" ON)

if(EXISTS "${CMAKE_CURRENT_LIST_DIR}/include")
    set(MDBTOOLS_INCLUDE_DIR "${CMAKE_CURRENT_LIST_DIR}/include")
    message(STATUS "MDBTOOLS_INCLUDE_DIR found: ${MDBTOOLS_INCLUDE_DIR}")
else()
    message(STATUS "include directory not found")
endif()

if(EXISTS "${CMAKE_CURRENT_LIST_DIR}/mestrelab")
    set(MESTRELAB_DIR "${CMAKE_CURRENT_LIST_DIR}/mestrelab")
    message(STATUS "MESTRELAB_DIR found: ${MESTRELAB_DIR}")
else()
    message(STATUS "mestrelab directory not found")
endif()

if (BUILD_LIBRARY)
    add_subdirectory(src/libmdb)
endif()