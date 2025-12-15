cmake_minimum_required(VERSION 3.20)

project(external VERSION 1.0.0 LANGUAGES C CXX)

include(ExternalProject)

# Global settings
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Common CMake arguments for all external projects
set(COMMON_CMAKE_ARGS
    -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
    -DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded$<$<CONFIG:Debug>:Debug>
    -DCMAKE_CXX_STANDARD=20
    -DCMAKE_DEBUG_POSTFIX=d
    -DBUILD_SHARED_LIBS=OFF
)

# ============================================================================
# Assimp
# ============================================================================
ExternalProject_Add(
    assimp
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/assimp
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/assimp
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DASSIMP_BUILD_TESTS=OFF
        -DASSIMP_BUILD_ASSIMP_TOOLS=OFF
        -DASSIMP_BUILD_SAMPLES=OFF
        -DASSIMP_BUILD_DOCS=OFF
        -DASSIMP_INSTALL=ON
        -DASSIMP_BUILD_ZLIB=ON
        -DASSIMP_NO_EXPORT=ON
        -DASSIMP_BUILD_ALL_IMPORTERS_BY_DEFAULT=OFF
        -DASSIMP_BUILD_OBJ_IMPORTER=ON
        -DASSIMP_BUILD_FBX_IMPORTER=ON
        -DASSIMP_BUILD_GLTF_IMPORTER=ON
)

# ============================================================================
# GLFW
# ============================================================================
ExternalProject_Add(
    glfw
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/glfw
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/glfw
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DGLFW_BUILD_DOCS=OFF
        -DGLFW_BUILD_TESTS=OFF
        -DGLFW_BUILD_EXAMPLES=OFF
        -DGLFW_INSTALL=ON
        -DGLFW_BUILD_WAYLAND=OFF
        -DGLFW_BUILD_X11=ON
)

# ============================================================================
# GLM
# ============================================================================
ExternalProject_Add(
    glm
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/glm
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/glm
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DGLM_ENABLE_CXX_20=ON
        -DGLM_BUILD_TESTS=OFF
)

# ============================================================================
# ImGui
# ============================================================================
ExternalProject_Add(
    imgui
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/imgui
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/imgui
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DCMAKE_PREFIX_PATH=${CMAKE_INSTALL_PREFIX}/glfw
        -DIMGUI_BUILD_GLFW_BINDING=ON
        -DIMGUI_BUILD_OPENGL3_BINDING=ON
    
    DEPENDS glfw
)

# ============================================================================
# nlohmann_json
# ============================================================================
ExternalProject_Add(
    json
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/json
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/json
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DJSON_BuildTests=OFF
        -DJSON_Install=ON
        -DJSON_MultipleHeaders=OFF
)

# ============================================================================
# spdlog
# ============================================================================
ExternalProject_Add(
    spdlog
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/spdlog
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/spdlog
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DSPDLOG_BUILD_EXAMPLE=OFF
        -DSPDLOG_BUILD_TESTS=OFF
        -DSPDLOG_INSTALL=ON
        -DSPDLOG_USE_STD_FORMAT=ON
        -DSPDLOG_BUILD_SHARED=OFF
)

# ============================================================================
# STB
# ============================================================================
ExternalProject_Add(
    stb
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/stb
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/stb
    
    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
)

# ============================================================================
# tinygltf
# ============================================================================
ExternalProject_Add(
    tinygltf
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/tinygltf
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/tinygltf

    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
        -DTINYGLTF_BUILD_LOADER_EXAMPLE=OFF
        -DTINYGLTF_INSTALL_VENDOR=OFF
)

# ============================================================================
# tinyobjloader
# ============================================================================
ExternalProject_Add(
    tinyobjloader
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/tinyobjloader
    INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/tinyobjloader

    CMAKE_ARGS
        ${COMMON_CMAKE_ARGS}
        -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
)