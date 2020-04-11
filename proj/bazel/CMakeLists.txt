cmake_minimum_required( VERSION 2.8 FATAL_ERROR )
set( CMAKE_VERBOSE_MAKEFILE ON )

include("${CMAKE_CURRENT_SOURCE_DIR}/../cmake/configure.cmake")

find_package( cinder REQUIRED PATHS 
	"${CMAKE_CURRENT_SOURCE_DIR}/../../${CINDER_LIB_DIRECTORY}")

add_library(cinder_cc INTERFACE)
target_link_libraries( cinder_cc INTERFACE cinder )

get_target_property(CINDER_LINK_LIBRARIES cinder INTERFACE_LINK_LIBRARIES)
file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/libs.txt" "${CINDER_LINK_LIBRARIES}")

get_target_property( CINDER_COMPILE_DEFINITIONS cinder
	INTERFACE_COMPILE_DEFINITIONS)
file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/cdefs.txt" "${CINDER_COMPILE_DEFINITIONS}")

get_target_property(CINDER_COMPILE_OPTIONS cinder
	INTERFACE_COMPILE_OPTIONS)
file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/copts.txt" "${CINDER_COMPILE_OPTIONS}")


