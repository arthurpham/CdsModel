FILE( GLOB_RECURSE PROJ_SOURCES src/*.c* ) # Scan all source files
FILE( GLOB_RECURSE PROJ_HEADERS include/*.h ) # Scan all public  header files

SET( PROJ_INCLUDES ${PROJ_INCLUDES} include ../lib/include/isda)
INCLUDE_DIRECTORIES( ${PROJ_INCLUDES} ) # Include path

# Group files in virtual folders under Visual Studio
SOURCE_GROUP( "Headers" FILES ${PROJ_HEADERS} )
SOURCE_GROUP( "Sources" FILES ${PROJ_SOURCES} )

ADD_LIBRARY( cdsxll SHARED ${PROJ_SOURCES} )
SET_TARGET_PROPERTIES( cdsxll PROPERTIES SUFFIX ".xll" )
FIND_LIBRARY( xlcall32 xlcall32 lib )
TARGET_LINK_LIBRARIES( cdsxll cdsmodel ${PROJ_LIBRARIES} ${xlcall32})

############################# Target Properties ###############################

SET_TARGET_PROPERTIES( cdsxll PROPERTIES SUFFIX ".xll" )
SET_TARGET_PROPERTIES( cdsxll PROPERTIES LINKER_LANGUAGE CXX ) # Language forced to C++

SET_TARGET_PROPERTIES( cdsxll PROPERTIES DEBUG_POSTFIX   "d" ) # Debug mode: a trailing "d"
SET_TARGET_PROPERTIES( cdsxll PROPERTIES RELEASE_POSTFIX ""  ) # Release mode: no postfix

#SET_TARGET_PROPERTIES( cdsxll PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for shared libraries
#SET_TARGET_PROPERTIES( cdsxll PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for module libraries
#SET_TARGET_PROPERTIES( cdsxll PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for import libraries

SET_TARGET_PROPERTIES( cdsxll PROPERTIES OUTPUT_NAME "cds" ) # Output name