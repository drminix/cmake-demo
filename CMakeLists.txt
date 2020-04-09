cmake_minimum_required(VERSION 3.1) #version dictates the policies which defines behavior
project(DUMMY VERSION 1.0 DESCRIPTION "DUMMY PROJECT" LANGUAGES CXX)

# variables
set(SRCS src/sum.cpp)
set(HDRS include/sum.hpp)

add_library(my_library ${SRCS} ${HDRS}) #it defines a library as well as target
target_include_directories(my_library PUBLIC include)

# cache or option which will be stored in CMakeCache.txt and displyed in UI
set(TAKE_VAR "Favourite Animal" CACHE STRING "Type in your animal")
mark_as_advanced(TAKE_VAR)

option(BUILD_DEBUG "short-cut to create a boolean cache" OFF)

# include is like include c++ use to store custom function
# add_subdirectory when things are stored in sub directores
# target_link_libraries PUBLIC INTERFACE PRIVATE 
# target_include_libraries
# PUBLIC - consumer needs to know this library sine it is in the API
# INTERFACE - dependency only consumer needs to know this library but target doesn't use it
# PRIVATE - consumer doesn't need to know this library and this is not it in the API

set(CURRENT_PLATFORM WIN32)

if(${CURRENT_PLATFORM}) #if the variable i ON,YES,TRUE,Y or non zero nnumber
	message(STATUS "This is WIN32")
	message(STATUS ${CMAKE_MODULE_PATH})
endif() #if variable is OFF,NO,FALSE, N, 0

if(BUILD_DEBUG)
	message(STATUS "BUILD DEBUG IS SET")
endif()

#generator-expression. Unlike most cmake commands, they are evaluted at build time or even instal time
# $<KEYWORD> ==

# function or macro -- macro don't have a scope . Funtion dont leak all their variables like macros do
function(SIMPLE REQUIRED_ARG)
	message(STATUS "simpe function ${REQUIRED_ARG}, followed by ${ARGV}")
	set(${REQUIRED_ARG} "\"From simple\"" PARENT_SCOPE)
endfunction()

Simple(TOSHIL 3 3) #function invocation is case insensitive
message(STATUS TOSHIL)

# CMake alow you to access CMAKe variables from your code using `configure_file` command
# This command copies a file (traditionally ending in .in from one place to another, subsituting all CMAkr varialbes it finds)

#TOP LEVEL SOURCE DIRECTORY FOR THE CURRENT PROJECT
#THIS IS THE SOURCE DIRECTORY FOR THE MOST RECENT PROJECT() COMMAND
configure_file("${PROJECT_SOURCE_DIR}/include/Version.h.in" 
"${PROJECT_SOURCE_DIR}/include/Version.h")

# add your own cmake folder to CMAKE_MODULE_PATH so that your custom cmake files are in your search find_path
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})
	


