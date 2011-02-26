############################## Sources ########################################
FILE( GLOB_RECURSE PROJ_SOURCES src/*.c* ) # Scan all source files
FILE( GLOB_RECURSE PROJ_HEADERS include/isda/*.h ) # Scan all public  header files

# Group files in virtual folders under Visual Studio
SOURCE_GROUP( "Headers" FILES ${PROJ_HEADERS} )
SOURCE_GROUP( "Sources" FILES ${PROJ_SOURCES} )

SET( PROJ_INCLUDES ${PROJ_INCLUDES} include/isda )
INCLUDE_DIRECTORIES( ${PROJ_INCLUDES} ) # Include path

# Group files in virtual folders under Visual Studio
SOURCE_GROUP( "Headers" FILES ${PROJ_HEADERS} )
SOURCE_GROUP( "Sources" FILES ${PROJ_SOURCES} )

ADD_LIBRARY( cdsmodel STATIC ${PROJ_SOURCES} )
TARGET_LINK_LIBRARIES( cdsmodel ${PROJ_LIBRARIES} )

############################# Target Properties ###############################

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES LINKER_LANGUAGE CXX ) # Language forced to C++

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES DEBUG_POSTFIX   "d" ) # Debug mode: a trailing "d"
SET_TARGET_PROPERTIES( cdsmodel PROPERTIES RELEASE_POSTFIX ""  ) # Release mode: no postfix

#SET_TARGET_PROPERTIES( cdsmodel PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for shared libraries
#SET_TARGET_PROPERTIES( cdsmodel PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for module libraries
#SET_TARGET_PROPERTIES( cdsmodel PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for import libraries

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES OUTPUT_NAME "cdsmodel" ) # Output name