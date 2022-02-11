cmake_minimum_required(VERSION 3.5.1)
project(ua-parser-lib-builder LANGUAGES CXX)

if (TARGET re2)
    include_directories(BEFORE ${RE2_INCLUDE_DIR})
else()
    add_subdirectory(contrib/re2)
    set(RE2_INCLUDE_DIR contrib/re2)
    include_directories(BEFORE ${RE2_INCLUDE_DIR})
endif()


set(BUILD_SHARED_LIBS OFF)

SET(EXT_CXX_FLAGS " -std=c++0x -Wall -Werror -g -fPIC -O3")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${EXT_CXX_FLAGS}")

add_library(uaparser_cpp STATIC
    contrib/uap-cpp/UaParser.cpp
    contrib/uap-cpp/internal/Pattern.cpp
    contrib/uap-cpp/internal/AlternativeExpander.cpp
    contrib/uap-cpp/internal/SnippetIndex.cpp
    contrib/uap-cpp/internal/ReplaceTemplate.cpp
)

target_link_libraries(uaparser_cpp PRIVATE
    re2
)

get_directory_property(hasParent PARENT_DIRECTORY)
set(uaparser_cpp_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/contrib/uap-cpp)
if(hasParent)
    set(uaparser_cpp_INCLUDE_DIR ${uaparser_cpp_INCLUDE_DIR} PARENT_SCOPE)
endif()

