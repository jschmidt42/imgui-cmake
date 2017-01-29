Stingray CMake support for ImGui
================================

See https://www.stingrayengine.com for more information on Stingray.

### Generate solution and Build

1. Create CMake build folder
> mkdir build && cd build

2. Generate solution with CMake
>  cmake -G "Visual Studio 14 2015 Win64" "-DCMAKE_CONFIGURATION_TYPES=Debug;Dev;Release" "-DCMAKE_INSTALL_PREFIX=./out" ..

3. Build solution
> cmake --build ./ --target imgui --config Dev

4. How to embed in a Stingray plugin using Cmake

In your stingray plugin `CMakeLists.txt` include the following snippet:

```cmake
list(APPEND CMAKE_ARGS "-D${CACHE_VAR}${CACHE_VAR_TYPE}=${${CACHE_VAR}}")
ExternalProject_Add(imgui
    GIT_REPOSITORY https://github.com/jschmidt42/imgui-cmake.git
    CMAKE_ARGS "${CMAKE_ARGS};-DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}"
)
```
