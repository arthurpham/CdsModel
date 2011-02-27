############################## Sources ########################################
FILE( GLOB_RECURSE PROJ_SOURCES src/*.c* ) # Scan all source files

# Group files in virtual folders under Visual Studio
SOURCE_GROUP( "Sources" FILES ${PROJ_SOURCES} )

SET( PROJ_INCLUDES ${PROJ_INCLUDES} ../lib/include/isda )
INCLUDE_DIRECTORIES( ${PROJ_INCLUDES} ) # Include path

# Group files in virtual folders under Visual Studio
SOURCE_GROUP( "Headers" FILES ${PROJ_HEADERS} )
SOURCE_GROUP( "Sources" FILES ${PROJ_SOURCES} )

ADD_EXECUTABLE (cdsconverter ${PROJ_SOURCES})
TARGET_LINK_LIBRARIES (cdsconverter cdsmodel ${PROJ_LIBRARIES})

