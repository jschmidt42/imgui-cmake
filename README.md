Stingray CMake support for ImGui
================================

### Generate solution and Build

1. Create CMake build folder
> mkdir build && cd build

2. Generate solution with CMake
>  cmake -G "Visual Studio 14 2015 Win64" "-DCMAKE_CONFIGURATION_TYPES=Debug;Dev;Release" "-DCMAKE_INSTALL_PREFIX=./out" ..

3. Build solution
> cmake --build ./ --target imgui --config Dev
