cmake_minimum_required(VERSION 3.1.0)

# header
file(GLOB LUA_HEADER
    "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h"
	"${CMAKE_CURRENT_SOURCE_DIR}/include/*.hpp"
)

add_library(lua_header INTERFACE)

target_include_directories(lua_header
	INTERFACE
	"${CMAKE_CURRENT_SOURCE_DIR}/include"
)
#! header

project(lua)

option(LUA_BUILD_SHARED "Build lua as a shared library" OFF)

file(GLOB LUA_SOURCES
	"${CMAKE_CURRENT_SOURCE_DIR}/src/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c"
)

list(REMOVE_ITEM LUA_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/lua.c"
)

if (LUA_BUILD_SHARED)
	add_library(${PROJECT_NAME} SHARED ${LUA_SOURCES})
else()
	add_library(${PROJECT_NAME} STATIC ${LUA_SOURCES})
endif()

target_link_libraries(${PROJECT_NAME}
	lua_header
)
target_compile_definitions(${PROJECT_NAME}
	PRIVATE
	$<$<BOOL:${LUA_BUILD_SHARED}>:LUA_BUILD_AS_DLL>
)