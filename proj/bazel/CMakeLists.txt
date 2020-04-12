cmake_minimum_required( VERSION 2.8 FATAL_ERROR )
set( CMAKE_VERBOSE_MAKEFILE ON )

add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/../.." cinder)

add_library(cinder_cc INTERFACE)
target_link_libraries( cinder_cc INTERFACE cinder )

set(LIBS)
set(LIB_TARGETS)
get_target_property(CINDER_LINK_LIBRARIES cinder INTERFACE_LINK_LIBRARIES)
if(CINDER_LINK_LIBRARIES)
	string(APPEND LIB_TARGETS "\n")
	foreach(LIB IN LISTS CINDER_LINK_LIBRARIES)
		string(REGEX MATCH "([a-zA-Z0-9._-]*)\\.[a-z]*$" TARGET_NAME "${LIB}")

		string(APPEND LIB_TARGETS "    \":${CMAKE_MATCH_1}\",\n")
		
		string(REPLACE "\\" "\\\\" LIB ${LIB})
		string(APPEND LIBS
			"cc_import(\n"
			"  name = \"${CMAKE_MATCH_1}\",\n"
			"  shared_library = \"${LIB}\",\n"
			"  system_provided = 1,\n"
			")\n\n"
		)
	endforeach(LIB)
endif()

set_target_properties(
	cinder PROPERTIES
	ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
)


set(DEFS)
get_target_property(CINDER_COMPILE_DEFINITIONS cinder INTERFACE_COMPILE_DEFINITIONS)
if(CINDER_COMPILE_DEFINITIONS)
	foreach(DEF IN LISTS CINDER_COMPILE_DEFINITIONS)
	string(APPEND DEFS "    \"${DEF}\",\n")
	endforeach(DEF)
endif()

set(OPTS)
get_target_property(CINDER_COMPILE_OPTIONS cinder INTERFACE_COMPILE_OPTIONS)
if("${CINDER_COMPILE_OPTIONS}")
	string(REPLACE ";" "\n" OPT_LIST "${CINDER_COMPILE_OPTIONS}")
	string(APPEND OPTS "\n")
	foreach(OPT ${OPT_LIST})
		string(APPEND OPTS "    \"${OPT}\",\n")
	endforeach(OPT)
endif()
file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/copts.txt" "${CINDER_COMPILE_OPTIONS}")

string(APPEND LIBS
	"cc_library(\n"
	"  name = \"cinder\",\n"
	"  visibility = [\"//visibility:public\"],\n"
	"  includes = [\"includes\"],\n"
	"  strip_include_prefix = \"include\",\n"
	"  srcs = [\"proj/bazel/build/$<TARGET_FILE_NAME:cinder>\"],\n"
	"  defines = [\n${DEFS}"
	"  ],\n"
	"  textual_hdrs = glob([\n"
	"    \"include/**/*.h\",\n"
	"    \"include/**/*.hpp\",\n"
	"    \"include/**/*.inl\",\n"
    "  ]),\n"
	"  alwayslink = True,\n"
	"  linkstatic = True,\n"
	"  copts = [${OPTS}],\n"
	"  deps = [${LIB_TARGETS}"
    "  ],\n"
	")\n\n"
)

file(GENERATE OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/../../../BUILD" CONTENT "${LIBS}")