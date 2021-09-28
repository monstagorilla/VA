cmake_minimum_required (VERSION 3.16 FATAL_ERROR)

project (
	VA
	VERSION 2021.1
	LANGUAGES CXX C
)

#Set the install root folder if not specified by the user (e.g. BinDirectory/dist/Release
#TODO: $<CONFIG> is not properly resolved
IF(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
  set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_BINARY_DIR}/dist/$<CONFIG> CACHE PATH "" FORCE)
ENDIF(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)

set (CMAKE_DEBUG_POSTFIX "-d")

if (NOT DEFINED CMAKE_RUNTIME_OUTPUT_DIRECTORY AND WIN32)
	set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/bin)
	set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/lib)
	set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/lib)
endif ()

if (CMAKE_GENERATOR MATCHES "Visual Studio")
	set_property (GLOBAL PROPERTY USE_FOLDERS ON)
endif ()


#---Options---

option(BUILD_SHARED_LIBS "" ON)

#TODO: Still required?
#if( NOT DEFINED ITA_VISTA_BUILD_STATIC )
#	set( ITA_VISTA_BUILD_STATIC OFF CACHE BOOL "Build against static ViSTA libraries" )
#endif( )
#if( NOT DEFINED ITA_CORE_LIBS_BUILD_STATIC )
#	set( ITA_VISTA_BUILD_STATIC OFF CACHE BOOL "Build against static ITA core libraries" )
#endif( )
#if( NOT DEFINED ITA_GEOMETRICAL_ACOUSTICS_BUILD_STATIC )
#	set( ITA_GEOMETRICAL_ACOUSTICS_BUILD_STATIC OFF CACHE BOOL "Build against static ITA geometrical acoustics libraries" )
#endif( )
#if( NOT DEFINED ITA_ACOUSTIC_VISUALIZATION_STATIC )
#	set( ITA_ACOUSTIC_VISUALIZATION_STATIC OFF CACHE BOOL "Build against static ITA acoustic visualization libraries" )
#endif( )

#TODO: Still required?
#if( NOT DEFINED ITA_VA_INSTALL_WITH_DLLS )
#	set( ITA_VA_INSTALL_WITH_DLLS ON CACHE BOOL "Install VA with depending DLLs (use this switch with care)" )
#endif( )

#Switches for components
option( ITA_VA_WITH_CORE "Include VA core library" OFF )
option( ITA_VA_WITH_SERVER_APP "Include VA server application (requires ITA_VA_WITH_CORE)" OFF )
#TODO: Get rid of Redstart
#option( ITA_VA_WITH_REDSTART_APP "Include Redstart, the VA GUI application" OFF )
option( ITA_VA_WITH_TESTS "Include VA tests" OFF )
option( ITA_VA_GENERATE_DOCUMENTATION "Generate source VA documentation (requires Doxygen)" OFF )
option( ITA_VA_WITH_BENCHMARKS "Include VA benchmarks" OFF )

#Bindings
option( ITA_VA_WITH_BINDING_MATLAB "Include Matlab binding for VA (requires Matlab and NatNetSDK)" OFF )
option( ITA_VA_WITH_BINDING_CS "Include C# binding for VA" ON )
#TODO: Remove VALUA
#option( ITA_VA_WITH_BINDING_LUA "Include VA benchmarks" OFF )
option( ITA_VA_WITH_BINDING_PYTHON "Include Python binding for VA (requires Python)" OFF )
option( ITA_VA_WITH_UNITY_SCRIPTS "Include VA Unity C# scripts" ON )
option( ITA_VA_WITH_UNITY_EXTENSIONS_SCRIPTS "Include VA Unity Extensions C# scripts (requires further Unity packages like LHM and VRTK)" OFF )

#if( ITA_VA_INSTALL_WITH_DLLS )
	#vista_install_all_dlls( bin )
#endif( )

# Version
if( NOT DEFINED ITA_VA_VERSION_MAJOR )
	set( ITA_VA_VERSION_MAJOR "v2021" CACHE STRING "VA version major (usually 'v' and year)" )
endif( )
if( NOT DEFINED ITA_VA_VERSION_MINOR )
	set( ITA_VA_VERSION_MINOR "a" CACHE STRING "VA version minor (usually a,b,c ...)" )
endif( )

#---End: Options---



#---VA submodules---

add_subdirectory( VABase )
add_subdirectory( VANet )

if( ITA_VA_WITH_CORE )
	add_subdirectory( VACore )
	if( ITA_VA_COPYING_GNU_GPL )
		message( "The VACore configuration you have selected requires copying under the terms of the GNU General Public License. This license includes a copyleft. If you are linking against this library version, make sure that the license agreement is not violated. Will add the required COPYING file to the distribution." )
		
	endif( )
endif( )

if( ITA_VA_WITH_SERVER_APP )
	if( NOT ITA_VA_WITH_CORE )
		message( SEND_ERROR "Cannot build VAServer without VACore. Activate ITA_VA_WITH_CORE first" )
	endif( )
	add_subdirectory( VAServer )
endif( )

#if( ITA_VA_WITH_REDSTART_APP )
	#add_subdirectory( Redstart )
#endif( )

if( ITA_VA_WITH_BINDING_MATLAB )
	add_subdirectory( VAMatlab )
endif( )

if( ITA_VA_WITH_BINDING_CS )
	add_subdirectory( VACS )
endif( )

#TODO: Remove?
#if( ITA_VA_WITH_BINDING_LUA )
	#add_subdirectory( VALua )
#endif( )

if( ITA_VA_WITH_BINDING_PYTHON )
	#TODO: Fix and test VAPython
	#add_subdirectory( VAPython )
endif( )

#---END: VA submodules---


#TODO: Still required?
if( MSVC AND NOT ITA_SUITE_COMMON_BUILD )
	set( ITA_VA_INCLUDE_EXTERNAL_MSVC_PROJECTS ON CACHE BOOL "If enabled, msvc projects of external ITA packages will be included in the solution" )
	if( ITA_VA_INCLUDE_EXTERNAL_MSVC_PROJECTS AND ITA_VA_WITH_CORE )
		#vista_add_external_msvc_project_of_package( VistaCoreLibs "VistaCoreLibs" DEPENDENT VANet VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITABase "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITAConvolution "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITACTC "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITADataSources "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITADSP "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITASampler "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITAFFT "ITACoreLibs" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITAGeo "ITAGeometricalAcoustics" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITAPropagationPathSim "ITAGeometricalAcoustics" DEPENDENT VACore SILENT )
		#vista_add_external_msvc_project_of_package( ITASimulationScheduler "ITASimulationScheduler" DEPENDENT VACore SILENT )
	endif( )
endif( )

# Documentation
if( ITA_VA_GENERATE_DOCUMENTATION )
	#TODO: Find doxygen / Test this

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

#---Install---

# (Selective) license, readme and logo
install( FILES README.md DESTINATION . COMPONENT ${PROJECT_NAME} )
install( DIRECTORY logo DESTINATION . COMPONENT ${PROJECT_NAME} )

if( ITA_VA_COPYING_GNU_GPL )

	set( SELECTED_LICENSE "GNU General Public License" )
	set( GPL_DEPENDENCIES "FFTW3" )
	configure_file( COPYING.proto "${CMAKE_CURRENT_SOURCE_DIR}/COPYING" @ONLY )
	install( FILES COPYING DESTINATION . COMPONENT ${PROJECT_NAME} )
	
else( )

	install( FILES LICENSE.md DESTINATION . COMPONENT ${PROJECT_NAME} )

endif( )


# Unity scripts
if( ITA_VA_WITH_UNITY_SCRIPTS )
	install( DIRECTORY "VAUnity" DESTINATION "unity" COMPONENT ${PROJECT_NAME} )
endif( )
if( ITA_VA_WITH_UNITY_EXTENSIONS_SCRIPTS )
	install( DIRECTORY "VAUnityExtensions" DESTINATION "unity" COMPONENT ${PROJECT_NAME} )
endif( )
