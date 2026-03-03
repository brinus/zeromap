cmake_minimum_required(VERSION 3.30)
project(zeromap
    VERSION 0.1.0
    HOMEPAGE_URL https://github.com/brinus/zeromap
    LANGUAGES CXX
)

configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/include/zeromap/zeromap.hpp.in
    ${CMAKE_CURRENT_BINARY_DIR}/include/zeromap/zeromap.hpp
    @ONLY
)

add_library(zeromap INTERFACE)

target_include_directories(zeromap INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

install(TARGETS zeromap
    EXPORT zeromapTargets
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/zeromap
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING PATTERN "*.hpp"
)

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include/zeromap
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING PATTERN "*.hpp"
)

install(EXPORT zeromapTargets
    FILE zeromapTargets.cmake
    NAMESPACE zeromap::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/zeromap
)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/zeromapConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/zeromapConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/zeromap
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/zeromapConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/zeromapConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/zeromapConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/zeromap
)
