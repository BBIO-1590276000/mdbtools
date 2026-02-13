cmake_minimum_required(VERSION 3.25)


if(NOT DEFINED MDBTOOLS_VER_REV)
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
    set(MDBTOOLS_VER_REV ${GIT_COMMIT_COUNT})
endif()

message(STATUS "Revision: ${MDBTOOLS_VER_REV}")

project("MDB Tools" VERSION 1.0.1 LANGUAGES C)

option(MDB_BUILD_LIBRARY "Build the MDB library" ON)
option(MDB_INSTALL_PER_CONFIG "Install per configuration (e.g., Debug, Release)" ON)
option(MDB_INSTALL_ONLY_LIBRARY "Install only the library without headers" OFF)
option(MDB_USE_ICONV "Use iconv for character encoding conversion" OFF)

if(NOT DEFINED MDBTOOLS_INSTALL_DIR AND MDB_INSTALL_PER_CONFIG)
    if (CMAKE_INSTALL_PREFIX)
        set(MDBTOOLS_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/$<CONFIG>" CACHE PATH "Installation directory" FORCE)
    else()
        set(MDBTOOLS_INSTALL_DIR "${CMAKE_CURRENT_LIST_DIR}/install/${CMAKE_SYSTEM_NAME}/$<CONFIG>" CACHE PATH "Installation directory" FORCE)
    endif()
endif()
message(STATUS "MDBTOOLS_INSTALL_DIR set to: ${MDBTOOLS_INSTALL_DIR}")

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

if (MDB_BUILD_LIBRARY)
    add_subdirectory(src/libmdb)
endif()