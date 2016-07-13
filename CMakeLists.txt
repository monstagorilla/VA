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


if( NOT DEFINED ITA_VA_INSTALL_WITH_DLLS )
	set( ITA_VA_INSTALL_WITH_DLLS ON CACHE BOOL "Install VA with depending DLLs (use this switch with care)" )
endif( NOT DEFINED ITA_VA_INSTALL_WITH_DLLS )

if( NOT DEFINED ITA_VA_WITH_BINDING_MATLAB )
	set( ITA_VA_WITH_BINDING_MATLAB OFF CACHE BOOL "Include Matlab binding for VA" )
endif( NOT DEFINED ITA_VA_WITH_BINDING_MATLAB )

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
if (ITA_VA_WITH_BINDING_MATLAB )
	add_subdirectory( VAMatlab )
endif( ITA_VA_WITH_BINDING_MATLAB )
add_subdirectory( VANet )
add_subdirectory( VAServer )

if( ITA_VA_INSTALL_WITH_DLLS )
	vista_install_all_dlls( bin )
endif()