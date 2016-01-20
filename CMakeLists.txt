cmake_minimum_required(VERSION 2.8)

set(META_PROJECT_NAME        "empty-cpp-project")
set(META_PROJECT_DESCRIPTION "C++ project template with CMake and Git")

option(BUILD_EXECUTABLE "Build executable"       ON)
option(BUILD_SHARED     "Build shared libraries" OFF)
option(BUILD_TEST       "Build test"             OFF)
option(BUILD_EXAMPLES   "Build examples"         ON)

project(${META_PROJECT_NAME})
include_directories(${PROJECT_SOURCE_DIR}/include)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

cmake_policy(SET CMP0054 NEW)

include(cmake/CompileOptions.cmake)
include(cmake/GetGitRevisionDescription.cmake)

get_git_head_revision(GET_REFSPEC GIT_SHA1)
string(SUBSTRING "${GIT_SHA1}" 0 12 GIT_REV)

file(GLOB_RECURSE SOURCES src/*.cpp)

if(BUILD_EXECUTABLE)
	if(WIN32)
		add_executable(${META_PROJECT_NAME} WIN32 ${SOURCES})
	else()
		add_executable(${META_PROJECT_NAME} ${SOURCES})
	endif()
else()
	if(BUILD_SHARED)
		add_library(${META_PROJECT_NAME} SHARED ${SOURCES})
	else()
		add_library(${META_PROJECT_NAME} STATIC ${SOURCES})
	endif()
endif()

if(BUILD_TEST)
	file(GLOB_RECURSE TEST_SOURCES test/*.cpp)
	add_executable(test ${TEST_SOURCES})
endif()
