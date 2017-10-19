cmake_minimum_required( VERSION 2.9 )

list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" )

project( VADoc )

find_package( doxygen REQUIRED )

configure_file( doxyfile.in doxyfile @ONLY )
configure_file( doxygen.footer.html.in doxygen.footer.html @ONLY )

add_custom_target( VADoc "${DOXYGEN_EXECUTABLE}" doxyfile 
	WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
	COMMENT "Generating VA C++ API documentation with Doxygen" VERBATIM
	)

set_property( TARGET VADoc PROPERTY FOLDER "VA" )

install( DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/html" DESTINATION "share/doc" )

if( VAWEBSITE_FOUND )
	if( WIN32 )
		configure_file( "copy_doc_to_va_website.bat.proto" "copy_doc_to_va_website.bat" @ONLY )
		add_custom_command( TARGET VADoc 
					POST_BUILD COMMAND copy_doc_to_va_website.bat
					COMMENT "Copying files to VAWebsite" VERBATIM )
	endif()
endif( )
