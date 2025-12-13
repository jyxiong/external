cmake_minimum_required(VERSION 3.15)

project(stb VERSION 1.0.0 LANGUAGES C)

# STB is a collection of header-only libraries
add_library(stb INTERFACE)
add_library(stb::stb ALIAS stb)

target_include_directories(stb INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/stb>
    $<INSTALL_INTERFACE:include>
)

# Installation
install(TARGETS stb
    EXPORT stbTargets
    INCLUDES DESTINATION include
)

# Install all header files
file(GLOB STB_HEADERS "stb/*.h")
install(FILES ${STB_HEADERS}
    DESTINATION include
)

# Install stb_vorbis.c as well
install(FILES stb/stb_vorbis.c
    DESTINATION include
)

install(EXPORT stbTargets
    FILE stbTargets.cmake
    NAMESPACE stb::
    DESTINATION lib/cmake/stb
)

# Create config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/stbConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
    INSTALL_DESTINATION lib/cmake/stb
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
    DESTINATION lib/cmake/stb
)
