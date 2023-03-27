cmake_minimum_required(VERSION 3.1.0)

# header
add_library(luabind_header INTERFACE)

target_include_directories(luabind_header
	INTERFACE
	"${CMAKE_CURRENT_SOURCE_DIR}/include"
)
#! header

project(luabind)

option(LUABIND_BUILD_SHARED "Build luabind as a shared library" OFF)

file(GLOB LUABIND_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"
)

if (LUABIND_BUILD_SHARED)
	add_library(${PROJECT_NAME} SHARED ${LUABIND_SOURCES})
else()
	add_library(${PROJECT_NAME} STATIC ${LUABIND_SOURCES})
endif()

target_link_libraries(${PROJECT_NAME}
	PUBLIC
	lua
	luabind_header
)

target_compile_definitions(${PROJECT_NAME}
	PRIVATE
	$<$<BOOL:${LUABIND_BUILD_SHARED}>:LUABIND_DYNAMIC_LINK>
)

if (MSVC)
    add_definitions("/MP")
	add_definitions("/wd4251")
endif()
