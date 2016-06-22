cmake_minimum_required( VERSION 2.8 )

project( VA )

if( MSVC )
	add_definitions( "/wd4251 /W4 -D_CRT_SECURE_NO_WARNINGS" )
endif()

# example how to set ExternalLibs folder for an entire project (without ENV var)
list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../VistaCMakeCommon" )

# ... or with ENV variable
list( APPEND CMAKE_MODULE_PATH "$ENV{VISTA_CMAKE_COMMON}" )

include( VistaCommon )

vista_find_package( Doxygen )
vista_find_package( VistaCoreLibs REQUIRED FIND_DEPENDENCIES )

vista_find_package( ITABase )
vista_find_package( ITAConvolution )
vista_find_package( ITAFFT )
vista_find_package( ITADataSources )
vista_find_package( ITACTC )
vista_find_package( ITASampler )

# VA
add_subdirectory( VABase )
add_subdirectory( VACore )
add_subdirectory( VAMatlab )
add_subdirectory( VANet )
add_subdirectory( VAServer )
