cmake_minimum_required(VERSION 3.15)

project(imgui VERSION 1.0.0 LANGUAGES CXX)

# Options for backends
option(IMGUI_BUILD_GLFW_BINDING "Build GLFW backend" OFF)
option(IMGUI_BUILD_SDL2_BINDING "Build SDL2 backend" OFF)
option(IMGUI_BUILD_SDL3_BINDING "Build SDL3 backend" OFF)
option(IMGUI_BUILD_WIN32_BINDING "Build Win32 backend" OFF)
option(IMGUI_BUILD_OPENGL2_BINDING "Build OpenGL2 renderer" OFF)
option(IMGUI_BUILD_OPENGL3_BINDING "Build OpenGL3 renderer" OFF)
option(IMGUI_BUILD_VULKAN_BINDING "Build Vulkan renderer" OFF)
option(IMGUI_BUILD_DX9_BINDING "Build DirectX 9 renderer" OFF)
option(IMGUI_BUILD_DX10_BINDING "Build DirectX 10 renderer" OFF)
option(IMGUI_BUILD_DX11_BINDING "Build DirectX 11 renderer" OFF)
option(IMGUI_BUILD_DX12_BINDING "Build DirectX 12 renderer" OFF)
option(IMGUI_BUILD_METAL_BINDING "Build Metal renderer" OFF)
option(IMGUI_BUILD_WGPU_BINDING "Build WebGPU renderer" OFF)

# Core imgui library
set(IMGUI_SOURCES
    imgui/imgui.cpp
    imgui/imgui_demo.cpp
    imgui/imgui_draw.cpp
    imgui/imgui_tables.cpp
    imgui/imgui_widgets.cpp
)

set(IMGUI_HEADERS
    imgui/imgui.h
    imgui/imconfig.h
    imgui/imgui_internal.h
    imgui/imstb_rectpack.h
    imgui/imstb_textedit.h
    imgui/imstb_truetype.h
)

# Create core imgui library
add_library(imgui_core STATIC ${IMGUI_SOURCES} ${IMGUI_HEADERS})
add_library(imgui::core ALIAS imgui_core)

target_include_directories(imgui_core PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/imgui>
    $<INSTALL_INTERFACE:include>
)

set_target_properties(imgui_core PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
    EXPORT_NAME core
)

# List to track all created targets for installation
set(IMGUI_TARGETS imgui_core)

# Helper function to create backend targets
function(add_imgui_backend NAME SOURCES HEADERS)
    add_library(imgui_${NAME} STATIC ${SOURCES} ${HEADERS})
    add_library(imgui::${NAME} ALIAS imgui_${NAME})
    
    target_link_libraries(imgui_${NAME} PUBLIC imgui_core)
    
    target_include_directories(imgui_${NAME} PUBLIC 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/imgui>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends>
        $<INSTALL_INTERFACE:include>
        $<INSTALL_INTERFACE:include/backends>
    )
    
    set_target_properties(imgui_${NAME} PROPERTIES
        CXX_STANDARD 11
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
        EXPORT_NAME ${NAME}
    )
    
    set(IMGUI_TARGETS ${IMGUI_TARGETS} imgui_${NAME} PARENT_SCOPE)
endfunction()

# Platform backends
if(IMGUI_BUILD_GLFW_BINDING)
    add_imgui_backend(glfw 
        "imgui/backends/imgui_impl_glfw.cpp"
        "imgui/backends/imgui_impl_glfw.h"
    )
    find_package(glfw3 REQUIRED)
    target_link_libraries(imgui_glfw PUBLIC glfw)
endif()

if(IMGUI_BUILD_SDL2_BINDING)
    add_imgui_backend(sdl2
        "imgui/backends/imgui_impl_sdl2.cpp"
        "imgui/backends/imgui_impl_sdl2.h"
    )
    find_package(SDL2 REQUIRED)
    target_link_libraries(imgui_sdl2 PUBLIC SDL2::SDL2)
endif()

if(IMGUI_BUILD_SDL3_BINDING)
    add_imgui_backend(sdl3
        "imgui/backends/imgui_impl_sdl3.cpp"
        "imgui/backends/imgui_impl_sdl3.h"
    )
    find_package(SDL3 REQUIRED)
    target_link_libraries(imgui_sdl3 PUBLIC SDL3::SDL3)
endif()

if(IMGUI_BUILD_WIN32_BINDING)
    add_imgui_backend(win32
        "imgui/backends/imgui_impl_win32.cpp"
        "imgui/backends/imgui_impl_win32.h"
    )
endif()

# Renderer backends
if(IMGUI_BUILD_OPENGL2_BINDING)
    add_imgui_backend(opengl2
        "imgui/backends/imgui_impl_opengl2.cpp"
        "imgui/backends/imgui_impl_opengl2.h"
    )
endif()

if(IMGUI_BUILD_OPENGL3_BINDING)
    add_imgui_backend(opengl3
        "imgui/backends/imgui_impl_opengl3.cpp"
        "imgui/backends/imgui_impl_opengl3.h;imgui/backends/imgui_impl_opengl3_loader.h"
    )
endif()

if(IMGUI_BUILD_VULKAN_BINDING)
    add_imgui_backend(vulkan
        "imgui/backends/imgui_impl_vulkan.cpp"
        "imgui/backends/imgui_impl_vulkan.h"
    )
    find_package(Vulkan REQUIRED)
    target_link_libraries(imgui_vulkan PUBLIC Vulkan::Vulkan)
endif()

if(IMGUI_BUILD_DX9_BINDING)
    add_imgui_backend(dx9
        "imgui/backends/imgui_impl_dx9.cpp"
        "imgui/backends/imgui_impl_dx9.h"
    )
    target_link_libraries(imgui_dx9 PUBLIC d3d9)
endif()

if(IMGUI_BUILD_DX10_BINDING)
    add_imgui_backend(dx10
        "imgui/backends/imgui_impl_dx10.cpp"
        "imgui/backends/imgui_impl_dx10.h"
    )
    target_link_libraries(imgui_dx10 PUBLIC d3d10 dxgi)
endif()

if(IMGUI_BUILD_DX11_BINDING)
    add_imgui_backend(dx11
        "imgui/backends/imgui_impl_dx11.cpp"
        "imgui/backends/imgui_impl_dx11.h"
    )
    target_link_libraries(imgui_dx11 PUBLIC d3d11 dxgi)
endif()

if(IMGUI_BUILD_DX12_BINDING)
    add_imgui_backend(dx12
        "imgui/backends/imgui_impl_dx12.cpp"
        "imgui/backends/imgui_impl_dx12.h"
    )
    target_link_libraries(imgui_dx12 PUBLIC d3d12 dxgi d3dcompiler)
endif()

if(IMGUI_BUILD_METAL_BINDING)
    add_imgui_backend(metal
        "imgui/backends/imgui_impl_metal.mm"
        "imgui/backends/imgui_impl_metal.h"
    )
    find_library(METAL_LIBRARY Metal)
    find_library(METALKIT_LIBRARY MetalKit)
    target_link_libraries(imgui_metal PUBLIC ${METAL_LIBRARY} ${METALKIT_LIBRARY})
endif()

if(IMGUI_BUILD_WGPU_BINDING)
    add_imgui_backend(wgpu
        "imgui/backends/imgui_impl_wgpu.cpp"
        "imgui/backends/imgui_impl_wgpu.h"
    )
endif()

# Installation rules
install(TARGETS ${IMGUI_TARGETS}
    EXPORT imguiTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(FILES ${IMGUI_HEADERS}
    DESTINATION include
)

# Install all backend headers
file(GLOB BACKEND_HEADERS "imgui/backends/*.h")
install(FILES ${BACKEND_HEADERS}
    DESTINATION include/backends
)

install(EXPORT imguiTargets
    FILE imguiTargets.cmake
    NAMESPACE imgui::
    DESTINATION lib/cmake/imgui
)

# Create config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/imguiConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/imguiConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake"
    INSTALL_DESTINATION lib/cmake/imgui
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/imguiConfigVersion.cmake"
    DESTINATION lib/cmake/imgui
)
