# Copyright 2019 Peter Dimov Distributed under the Boost Software License,
# Version 1.0. See accompanying file LICENSE_1_0.txt or copy at
# http://www.boost.org/LICENSE_1_0.txt

cmake_minimum_required(VERSION 3.5...3.16)

project(
  Preprocessor
  VERSION 1.84.0
  LANGUAGES CXX)

add_library(preprocessor INTERFACE)
add_library(Boost::preprocessor ALIAS preprocessor)

target_include_directories(
  preprocessor INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
                         $<INSTALL_INTERFACE:include>)

if(BUILD_TESTING)

  add_subdirectory(test)

endif()

if(BOOST_PREPROCESSOR_INSTALL)
  include(GNUInstallDirs)

  # Install Preprocessor headers.
  install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

  # Install Preprocessor binaries.
  install(
    TARGETS preprocessor
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

  # Install Preprocessor configuration files.
  include(CMakePackageConfigHelpers)

  configure_package_config_file(
    PreprocessorConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/preprocessor)

  write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY SameMajorVersion)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
                ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
          DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/preprocessor)

  # Perform installation.
  install(
    EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE Boost::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/preprocessor)
endif(BOOST_PREPROCESSOR_INSTALL)
