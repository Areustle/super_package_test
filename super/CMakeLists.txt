cmake_minimum_required(VERSION 3.12)

project(super VERSION 0.0)

find_package(dep REQUIRED)
find_package(pkg0 REQUIRED)

add_library(super INTERFACE)
add_library(super::super ALIAS super)

target_link_libraries(super PRIVATE dep::dep)
target_link_libraries(super PRIVATE pkg0::pkg0)

include(GNUInstallDirs)
set(CONFIG_INSTALLDIR ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})
install(TARGETS super EXPORT ${PROJECT_NAME}Targets)
install(EXPORT pkg0Targets
        NAMESPACE ${PROJECT_NAME}::
        DESTINATION ${CONFIG_INSTALLDIR})

include(CMakePackageConfigHelpers)
configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/Config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
  INSTALL_DESTINATION
  ${CONFIG_INSTALLDIR})

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        DESTINATION ${CONFIG_INSTALLDIR})

export(EXPORT ${PROJECT_NAME}Targets
       FILE ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake
       NAMESPACE ${PROJECT_NAME}::)

export(PACKAGE super)
