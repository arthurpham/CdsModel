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
OPTION( BUILD_XLL  	 "Build the unit tests"            OFF )

MESSAGE( STATUS "Building documentation:   " ${BUILD_DOC}    )
MESSAGE( STATUS "Building tests:           " ${BUILD_TESTS}  )
MESSAGE( STATUS "Building excel:           " ${BUILD_XLL}    )

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
#FILE( COPY ${PROJ_DATA_PATH} DESTINATION ${PROJ_OUT_PATH} )

######################## Specific preprocessor  tags ##########################
IF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	ADD_DEFINITIONS(-DMACOSX)
	IF ( ${CMAKE_BUILD_TYPE} MATCHES "Debug" )
	    ADD_DEFINITIONS(-fprofile-arcs -ftest-coverage)
	    SET( PROJ_LIBRARIES ${PROJ_LIBRARIES} gcov   )
	ENDIF( ${CMAKE_BUILD_TYPE} MATCHES "Debug" )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	ADD_DEFINITIONS(-DLINUX)
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

IF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	ADD_DEFINITIONS(-DWINDOWS -D_CRT_SECURE_NO_WARNINGS)
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")

############################ Project generation ###############################
ADD_SUBDIRECTORY( lib )

ADD_SUBDIRECTORY( examples/c )

ADD_SUBDIRECTORY( cdsconverter )

IF ( BUILD_XLL )
  ADD_SUBDIRECTORY( excel )
ENDIF( BUILD_XLL )

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





