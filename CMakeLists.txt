cmake_minimum_required(VERSION 3.6)

set(CMAKE_CONFIGURATION_TYPES "Debug;Dev;Release"
    CACHE STRING
    "List of supported configurations."
    FORCE)

project(imgui)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
include(CMakeSettings)

set(CMAKE_C_FLAGS)
set(CMAKE_CXX_FLAGS)
set(CMAKE_C_FLAGS_DEBUG "-D_DEBUG")
set(CMAKE_CXX_FLAGS_DEBUG "-D_DEBUG")
set(CMAKE_C_FLAGS_RELEASE "-DNDEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG")

# Setup platforms
if( PLATFORM_WINDOWS )
	add_compile_options(-DWINDOWSPC)
endif()

# Define standard configurations
if( CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_CONFIGURATION_TYPES MATCHES "Debug;Dev;Release" )
	list(APPEND Configs Debug Dev Release)
	set(CMAKE_CONFIGURATION_TYPES ${Configs} CACHE STRING "List of supported configurations." FORCE)
	set(CMAKE_INSTALL_PREFIX $ENV{SR_BIN_DIR} CACHE STRING "Default installation directory." FORCE)
	message(FATAL_ERROR "Default configuration was reset, please re-run CMake.")
endif()

# Initialize the development configuration using release configuration
set(CMAKE_C_FLAGS_DEV "${CMAKE_C_FLAGS_RELEASE}")
set(CMAKE_CXX_FLAGS_DEV "${CMAKE_CXX_FLAGS_RELEASE}")
set(CMAKE_STATIC_LINKER_FLAGS_DEV "${CMAKE_STATIC_LINKER_FLAGS_RELEASE}")

add_definitions("-DIMGUI_API=__declspec(dllexport)")

set(root ${CMAKE_CURRENT_SOURCE_DIR}/imgui)
set(INCLUDE_FILES ${root}/imgui.h ${CMAKE_CURRENT_SOURCE_DIR}/export/imconfig.h)
set(ALL_SOURCE_FILES
	${root}/stb_rect_pack.h ${root}/stb_textedit.h ${root}/stb_truetype.h
	${INCLUDE_FILES} ${root}/imgui_internal.h ${root}/imgui.cpp
	${root}/imgui_draw.cpp ${root}/imgui_demo.cpp)
add_library(${PROJECT_NAME} STATIC ${ALL_SOURCE_FILES})

export(TARGETS ${PROJECT_NAME}
       APPEND FILE ${CMAKE_BINARY_DIR}/${PROJECT_NAME}-targets.cmake)

install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}-targets DESTINATION lib)
install(FILES ${INCLUDE_FILES} DESTINATION include)
