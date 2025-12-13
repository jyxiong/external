# External Libraries

A collection of precompiled C++ third-party libraries managed and built uniformly using CMake ExternalProject.

## Included Libraries

- **Assimp** - 3D model loading library (supports OBJ, FBX, glTF, etc.)
- **GLFW** - OpenGL window and input handling library
- **GLM** - OpenGL Mathematics library
- **ImGui** - Immediate mode GUI library (includes GLFW + OpenGL3 backend)
- **nlohmann_json** - Modern C++ JSON library
- **spdlog** - Fast C++ logging library
- **STB** - Single-file header library collection (image loading, fonts, etc.)

## Build Configuration

- All libraries are built as **static libraries**
- MSVC Runtime: `MultiThreaded` (Release) / `MultiThreadedDebug` (Debug)
- C++ Standard: C++20
- All tests, examples, and documentation builds are disabled

## Usage

### 1. Clone Repository (with submodules)

```bash
git clone --recursive <repository_url>
# Or if already cloned
git submodule update --init --recursive
```

### 2. Configure Build

```bash
# Use default installation path (CMAKE_INSTALL_PREFIX)
cmake -B build -DCMAKE_BUILD_TYPE=Release

# Or specify custom installation path
cmake -B build -DCMAKE_INSTALL_PREFIX=C:/libs -DCMAKE_BUILD_TYPE=Release
```

### 3. Build and Install

```bash
# Build and install all libraries
cmake --build build --config Release
```

### 4. Installation Structure

```
<CMAKE_INSTALL_PREFIX>/
├── assimp/
│   ├── lib/
│   ├── include/
│   └── lib/cmake/assimp/
├── glfw/
├── glm/
├── imgui/
├── json/
├── spdlog/
└── stb/
```

## Using in Your Project

In your CMakeLists.txt:

```cmake
# Set search paths
set(CMAKE_PREFIX_PATH 
    "C:/libs/assimp"
    "C:/libs/glfw"
    "C:/libs/imgui"
    ${CMAKE_PREFIX_PATH}
)

# Find and link libraries
find_package(assimp REQUIRED)
find_package(glfw3 REQUIRED)
find_package(nlohmann_json REQUIRED)
find_package(spdlog REQUIRED)

target_link_libraries(your_app PRIVATE
    assimp::assimp
    glfw
    nlohmann_json::nlohmann_json
    spdlog::spdlog
    imgui::core
    imgui::glfw
    imgui::opengl3
)
```

## Requirements

- CMake 3.20+
- C++20 compatible compiler
- Windows (MSVC) / Linux (GCC/Clang) / macOS

## License

Each library uses its own license. See the LICENSE file in each submodule for details.
