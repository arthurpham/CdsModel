########################## Project configuration ##############################
SET( PROJ_NAME      "cdsmodel" )
SET( PROJ_PATH      ${CMAKE_SOURCE_DIR} )
SET( PROJ_OUT_PATH  ${CMAKE_BINARY_DIR} )
SET( PROJ_DATA_PATH ${CMAKE_SOURCE_DIR}/resources )
SET( PROJ_DOC_PATH  ${PROJ_OUT_PATH}/doc )
SET( PROJ_SOURCES   "" )
SET( PROJ_HEADERS   "" )
SET( PROJ_LIBRARIES "" )
SET( PROJ_INCLUDES  "" )

####################### Minimum CMake version to run ##########################
CMAKE_MINIMUM_REQUIRED( VERSION 2.8 )

############################## Project name ###################################
PROJECT( cdsmodel )

MESSAGE( "--------------------------------------------------" )
MESSAGE( STATUS "Configuring cdsmodel" )

SET( cdsmodel_MAJOR_VERSION 0 )
SET( cdsmodel_MINOR_VERSION 0 )
SET( cdsmodel_PATCH_LEVEL   1 )

########################## Modules configuration ##############################
OPTION( BUILD_DOC    "Build the doxygen documentation" OFF )
OPTION( BUILD_TESTS  "Build the unit tests"            OFF )

MESSAGE( STATUS "Building documentation:   " ${BUILD_DOC}    )
MESSAGE( STATUS "Building tests:           " ${BUILD_TESTS}  )

############################## Sources ########################################
FILE( GLOB_RECURSE PROJ_SOURCES ../lib/src/*.c* ) # Scan all source files
FILE( GLOB_RECURSE PROJ_HEADERS ../lib/include/isda/*.h ) # Scan all public  header files

SET( PROJ_INCLUDES ${PROJ_INCLUDES} ${PROJ_PATH}/../lib/include/isda )

# Group files in virtual folders under Visual Studio
SOURCE_GROUP( "Headers" FILES ${PROJ_SOURCES} )
SOURCE_GROUP( "Sources" FILES ${PROJ_HEADERS} )

############################ External libraries ###############################
# Add current source directory as module path for Find*.cmake
SET( CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJ_PATH} )

# Template for adding a library
#FIND_PACKAGE( <LibraryName> REQUIRED )
#SET( PROJ_INCLUDES  ${PROJ_INCLUDES}  ${<LibraryName>_INCLUDE_DIR} )
#SET( PROJ_LIBRARIES ${PROJ_LIBRARIES} ${<LibraryName>_LIBRARIES}   )

################################ Includes #####################################
INCLUDE_DIRECTORIES( ${PROJ_INCLUDES} ) # Include path

################################ Resources ####################################
FILE( COPY ${PROJ_DATA_PATH} DESTINATION ${PROJ_OUT_PATH} )

############################ Project generation ###############################
ADD_LIBRARY( cdsmodel SHARED ${PROJ_SOURCES} )
#ADD_EXECUTABLE( cdsmodel ${PROJ_SOURCES} )

TARGET_LINK_LIBRARIES( cdsmodel ${PROJ_LIBRARIES} )

############################## Documentation ##################################
IF ( BUILD_DOC )
  FIND_PACKAGE( Doxygen )
  SET( DOXYFILE_IN  "${PROJ_PATH}/Doxyfile.in"  )
  SET( DOXYFILE_OUT "${PROJ_OUT_PATH}/Doxyfile" )
  SET_PROPERTY( DIRECTORY APPEND PROPERTY ADDITIONAL_MAKE_CLEAN_FILES ${PROJ_DOC_PATH} )
  ADD_CUSTOM_TARGET( doc COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYFILE_OUT}
                                 "Generating documentation..."
                                 WORKING_DIRECTORY ${PROJ_PATH} )
  CONFIGURE_FILE( ${DOXYFILE_IN} ${DOXYFILE_OUT} @ONLY )
ENDIF( BUILD_DOC )

################################## Tests ######################################
IF ( BUILD_TESTS )
  ENABLE_TESTING()
  INCLUDE( CTest )
  ADD_SUBDIRECTORY( tests )
ENDIF( BUILD_TESTS )

MESSAGE( "--------------------------------------------------" )

############################# Target Properties ###############################
ADD_DEFINITIONS(-DLINUX)

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES LINKER_LANGUAGE CXX ) # Language forced to C++

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES DEBUG_POSTFIX   "d" ) # Debug mode: a trailing "d"
SET_TARGET_PROPERTIES( cdsmodel PROPERTIES RELEASE_POSTFIX ""  ) # Release mode: no postfix

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for shared libraries
SET_TARGET_PROPERTIES( cdsmodel PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for module libraries
SET_TARGET_PROPERTIES( cdsmodel PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJ_OUT_PATH} ) # Path for import libraries

SET_TARGET_PROPERTIES( cdsmodel PROPERTIES OUTPUT_NAME "cdsmodel" ) # Output name

