cmake_minimum_required (VERSION 2.8)

find_package(Doxygen)

# Helper functions
macro(path_linux_to_win MsysPath ResultingPath)
  string(REGEX REPLACE "^/([a-zA-Z])/" "\\1:/" ${ResultingPath} "${MsysPath}")
endmacro()
macro(path_win_to_linux MsysPath ResultingPath)
  string(REGEX REPLACE "^([a-zA-Z]):/" "/\\1/" ${ResultingPath} "${MsysPath}")
endmacro()

# enable if doxygen found
option(BUILD_DOCUMENTATION "Create and install the HTML based API documentation (requires Doxygen)" ${DOXYGEN_FOUND})
if(DOXYGEN_VERSION VERSION_LESS "1.8.0")
    message(WARNING "Doxygen version " ${DOXYGEN_VERSION} " is older than 1.8.0, disabling documentation build")
    set(BUILD_DOCUMENTATION FALSE)
endif()

if(BUILD_DOCUMENTATION)
    if(NOT DOXYGEN_FOUND)
        message(FATAL_ERROR "Doxygen is needed to build the documentation.")
    endif()

    path_win_to_linux(${CMAKE_CURRENT_SOURCE_DIR} CMAKE_CURRENT_SOURCE_DIR_LINUX)
    path_win_to_linux(${PROJECT_SOURCE_DIR} PROJECT_SOURCE_DIR_LINUX)

    set(doxyfile_in ${CMAKE_CURRENT_SOURCE_DIR}/doxygen/Doxyfile.in)
    set(doxyfile ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)
    configure_file(${doxyfile_in} ${doxyfile} @ONLY)
    add_custom_target(doc_build_only
        COMMAND ${DOXYGEN_EXECUTABLE} ${doxyfile}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating API documentation with Doxygen"
        VERBATIM)
    #install the test
    add_custom_target(doc
        COMMAND ${CMAKE_BUILD_TOOL} install
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        DEPENDS doc_build_only
        )

    install(
        DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/doc/
        DESTINATION ./doc )
endif()



#TODO comment this if used in project
# set default install path
if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set (CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/dist" CACHE PATH "Install path prefix, prepended onto install directories." FORCE )
endif()

message(STATUS "Building doc:         ${BUILD_DOCUMENTATION}")


