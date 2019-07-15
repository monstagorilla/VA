cmake_minimum_required(VERSION 2.8.12)
project( VA )

set( VA_COMMON_BUILD ON )

if( MSVC )
	add_definitions( "/wd4251 /W4 -D_CRT_SECURE_NO_WARNINGS" )
endif( )

# example how to set ExternalLibs folder for an entire project (without ENV var)
list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../VistaCMakeCommon" )

# ... or with ENV variable
list( APPEND CMAKE_MODULE_PATH "$ENV{VISTA_CMAKE_COMMON}" )

include( VistaCommon )

vista_find_package( Doxygen QUIET )

if( NOT DEFINED ITA_VISTA_BUILD_STATIC )
	set( ITA_VISTA_BUILD_STATIC OFF CACHE BOOL "Build against static ViSTA libraries" )
endif( )

if( NOT DEFINED ITA_CORE_LIBS_BUILD_STATIC )
	set( ITA_VISTA_BUILD_STATIC OFF CACHE BOOL "Build against static ITA core libraries" )
endif( )

if( NOT DEFINED ITA_GEOMETRICAL_ACOUSTICS_STATIC )
	set( ITA_GEOMETRICAL_ACOUSTICS_STATIC OFF CACHE BOOL "Build against static ITA geometrical acoustics libraries" )
endif( )

if( NOT DEFINED ITA_ACOUSTIC_VISUALIZATION_STATIC )
	set( ITA_ACOUSTIC_VISUALIZATION_STATIC OFF CACHE BOOL "Build against static ITA acoustic visualization libraries" )
endif( )


if( NOT DEFINED ITA_VA_INSTALL_WITH_DLLS )
	set( ITA_VA_INSTALL_WITH_DLLS ON CACHE BOOL "Install VA with depending DLLs (use this switch with care)" )
endif( )


if( NOT DEFINED ITA_VA_WITH_CORE )
	set( ITA_VA_WITH_CORE OFF CACHE BOOL "Include VA core library" )
endif( )


if( NOT DEFINED ITA_VA_WITH_SERVER_APP )
	set( ITA_VA_WITH_SERVER_APP OFF CACHE BOOL "Include VA server application" )
endif( )


if( NOT DEFINED ITA_VA_WITH_REDSTART_APP )
	set( ITA_VA_WITH_REDSTART_APP OFF CACHE BOOL "Include Redstart, the VA GUI application" )
endif( )


if( NOT DEFINED ITA_VA_WITH_TESTS )
	set( ITA_VA_WITH_TESTS OFF CACHE BOOL "Include VA tests" )
endif( )


if( NOT DEFINED ITA_VA_GENERATE_DOCUMENTATION )
	set( ITA_VA_GENERATE_DOCUMENTATION OFF CACHE BOOL "Generate source VA documentation (requires Doxygen)" )
endif( )


if( NOT DEFINED ITA_VA_WITH_BENCHMARKS )
	set( ITA_VA_WITH_BENCHMARKS OFF CACHE BOOL "Include VA benchmarks" )
endif( )


if( NOT DEFINED ITA_VA_WITH_BINDING_MATLAB )
	set( ITA_VA_WITH_BINDING_MATLAB OFF CACHE BOOL "Include Matlab binding for VA" )
endif( )

if( NOT DEFINED ITA_VA_WITH_BINDING_CS )
	set( ITA_VA_WITH_BINDING_CS ON CACHE BOOL "Include C# binding for VA" )
endif( )

if( NOT DEFINED ITA_VA_WITH_BINDING_LUA )
	set( ITA_VA_WITH_BINDING_LUA OFF CACHE BOOL "Include Lua binding for VA" )
endif( )

if( NOT DEFINED ITA_VA_WITH_BINDING_PYTHON )
	set( ITA_VA_WITH_BINDING_PYTHON OFF CACHE BOOL "Include Python binding for VA" )
endif( )


if( NOT DEFINED ITA_VA_WITH_UNITY_SCRIPTS )
	set( ITA_VA_WITH_UNITY_SCRIPTS ON CACHE BOOL "Include VA Unity C# scripts" )
endif( )

if( NOT DEFINED ITA_VA_WITH_UNITY_EXTENSIONS_SCRIPTS )
	set( ITA_VA_WITH_UNITY_EXTENSIONS_SCRIPTS OFF CACHE BOOL "Include VA Unity Extensions C# scripts (requires further Unity packages like LHM and VRTK)" )
endif( )


if( ITA_VA_INSTALL_WITH_DLLS )
	vista_install_all_dlls( bin )
endif( )


# VA submodules

add_subdirectory( VABase )
add_subdirectory( VANet )

if( ITA_VA_WITH_CORE )
	add_subdirectory( VACore )
	if( ITA_VA_COPYING_GNU_GPL )
		message( "The VACore configuration you have selected requires copying under the terms of the GNU General Public License. This license includes a copyleft. If you are linking against this library version, make sure that the license agreement is not violated. Will add the required COPYING file to the distribution." )
		
	endif( )
endif( )

if( ITA_VA_WITH_SERVER_APP )
	add_subdirectory( VAServer )
	vista_use_package( VACore REQUIRED FIND_DEPENDENCIES ) # ... installs VACore dll
endif( )

if( ITA_VA_WITH_REDSTART_APP )
	add_subdirectory( Redstart )
	vista_use_package( VACore REQUIRED FIND_DEPENDENCIES ) # ... installs VACore dll
endif( )

if( ITA_VA_WITH_BINDING_MATLAB )
	add_subdirectory( VAMatlab )
	vista_use_package( VAMatlab REQUIRED FIND_DEPENDENCIES ) # ... installs VAMatlab mex
endif( )

if( ITA_VA_WITH_BINDING_CS )
	add_subdirectory( VACS )
endif( )

if( ITA_VA_WITH_BINDING_LUA )
	add_subdirectory( VALua )
endif( )

if( ITA_VA_WITH_BINDING_PYTHON )
	add_subdirectory( VAPython )
endif( )


if( MSVC )
	set( ITA_VA_INCLUDE_EXTERNAL_MSVC_PROJECTS ON CACHE BOOL "If enabled, msvc projects of external ITA packages will be included in the solution" )
	if( ITA_VA_INCLUDE_EXTERNAL_MSVC_PROJECTS AND ITA_VA_WITH_CORE )
		vista_add_external_msvc_project_of_package( VistaCoreLibs "VistaCoreLibs" DEPENDENT VANet VACore SILENT )
		vista_add_external_msvc_project_of_package( ITABase "ITACoreLibs" DEPENDENT VACore SILENT )
		vista_add_external_msvc_project_of_package( ITAConvolution "ITACoreLibs" DEPENDENT VACore SILENT )
		vista_add_external_msvc_project_of_package( ITACTC "ITACoreLibs" DEPENDENT VACore SILENT )
		vista_add_external_msvc_project_of_package( ITADataSources "ITACoreLibs" DEPENDENT VACore SILENT )
		vista_add_external_msvc_project_of_package( ITADSP "ITACoreLibs" DEPENDENT VACore SILENT )
		vista_add_external_msvc_project_of_package( ITASampler "ITACoreLibs" DEPENDENT VACore SILENT )
		vista_add_external_msvc_project_of_package( ITAFFT "ITACoreLibs" DEPENDENT VACore SILENT )
	endif( )
endif( )

# version

if( NOT DEFINED ITA_VA_VERSION_MAJOR )
	set( ITA_VA_VERSION_MAJOR "v2019" CACHE STRING "VA version major (usually 'v' and year)" )
endif( )

if( NOT DEFINED ITA_VA_VERSION_MINOR )
	set( ITA_VA_VERSION_MINOR "a" CACHE STRING "VA version minor (usually a,b,c ...)" )
endif( )


# (selective) license, readme and logo

install( FILES README.md DESTINATION . )
install( DIRECTORY logo DESTINATION . )

if( ITA_VA_COPYING_GNU_GPL )

	set( SELECTED_LICENSE "GNU General Public License" )
	set( GPL_DEPENDENCIES "FFTW3" )
	configure_file( COPYING.proto "${CMAKE_CURRENT_SOURCE_DIR}/COPYING" @ONLY )
	install( FILES COPYING DESTINATION . )
	
else( )

	install( FILES LICENSE.md DESTINATION . )

endif( )


# doc

if( ITA_VA_GENERATE_DOCUMENTATION )

	if( NOT DOXYGEN_EXECUTABLE )
		message( FATAL_ERROR "VA documentation requires Doxygen." )
	endif( )
	
	set( VAWEBSITE_FOUND OFF )
	if( EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/VAWebsite" )
		set( VAWEBSITE_FOUND ON )
		set( VAWEBSITE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/VAWebsite" )
	endif( )

	add_subdirectory( "doc" )

endif( )


# unity

if( ITA_VA_WITH_UNITY_SCRIPTS )
	install( DIRECTORY "VAUnity" DESTINATION "unity" )
endif( )

if( ITA_VA_WITH_UNITY_EXTENSIONS_SCRIPTS )
	install( DIRECTORY "VAUnityExtensions" DESTINATION "unity" )
endif( )
