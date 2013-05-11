cmake_minimum_required(VERSION 2.8)
project(artemis)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set(CMAKE_INCLUDE_CURRENT_DIR  ON)

if(WIN32)
	set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/output/bin CACHE INTERNAL "Single output directory for building all executables.")
else(WIN32)
	set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/output/lib CACHE INTERNAL "Single output directory for building all libraries.")
endif(WIN32)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/output/bin 	CACHE INTERNAL "Single output directory for building all executables.")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/output/lib 	CACHE INTERNAL "Single output directory for building all libraries.")

mark_as_advanced(
	CMAKE_LIBRARY_OUTPUT_DIRECTORY
	CMAKE_RUNTIME_OUTPUT_DIRECTORY
	CMAKE_ARCHIVE_OUTPUT_DIRECTORY
)

set(CMAKE_DEBUG_POSTFIX "_d")

##
## Library
##

file(GLOB _sources_artemis artemis/*.cpp)
file(GLOB _headers_artemis artemis/*.h)

file(GLOB _sources_artemis_managers artemis/managers/*.cpp)
file(GLOB _headers_artemis_managers artemis/managers/*.h)

file(GLOB _sources_artemis_systems artemis/systems/*.cpp)
file(GLOB _headers_artemis_systems artemis/systems/*.h)

file(GLOB _sources_artemis_utils artemis/utils/*.cpp)
file(GLOB _headers_artemis_utils artemis/utils/*.h)

add_library(artemis STATIC
	${_sources_artemis} ${_headers_artemis}
	${_sources_artemis_managers} ${_headers_artemis_managers}
	${_sources_artemis_systems} ${_headers_artemis_systems}
	${_sources_artemis_utils} ${_headers_artemis_utils}
)

set_target_properties(artemis PROPERTIES PREFIX "lib")

if(WIN32)
	add_definitions(-MP)
endif()

source_group( "artemis" FILES ${_sources_artemis} ${_headers_artemis})
source_group( "artemis\\managers" FILES ${_sources_artemis_managers} ${_headers_artemis_managers})
source_group( "artemis\\systems" FILES ${_sources_artemis_systems} ${_headers_artemis_systems})
source_group( "artemis\\utils" FILES ${_sources_artemis_utils} ${_headers_artemis_utils})

##
## Sample
##

add_executable(sample sample.cpp)
target_link_libraries(sample artemis)

##
## Installing
##

install(TARGETS artemis
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)

install(FILES ${_headers_artemis} DESTINATION include/artemis)
install(FILES ${_headers_artemis_managers} DESTINATION include/artemis/managers)
install(FILES ${_headers_artemis_systems} DESTINATION include/artemis/systems)
install(FILES ${_headers_artemis_utils} DESTINATION include/artemis/utils)