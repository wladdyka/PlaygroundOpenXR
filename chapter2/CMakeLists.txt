cmake_minimum_required(VERSION 3.22.1)
set(PROJECT_NAME OpenXRTutorialChapter2)
project("${PROJECT_NAME}")

# Additional Directories for find_package() to search within.
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../cmake")

# For FetchContent_Declare() and FetchContent_MakeAvailable()
include(FetchContent)

# openxr_loader - From github.com/KhronosGroup
set(BUILD_TESTS
        OFF
        CACHE INTERNAL "Build tests"
)
set(BUILD_API_LAYERS
        ON
        CACHE INTERNAL "Use OpenXR layers"
)
FetchContent_Declare(
        OpenXR
        URL_HASH MD5=924a94a2da0b5ef8e82154c623d88644
        URL https://github.com/KhronosGroup/OpenXR-SDK-Source/archive/refs/tags/release-1.0.34.zip
        SOURCE_DIR
        openxr
)
FetchContent_MakeAvailable(OpenXR)

# Files
set(SOURCES
        "main.cpp"
        "../common/GraphicsAPI.cpp"
        "../common/GraphicsAPI_OpenGL.cpp"
        "../common/OpenXRDebugUtils.cpp")
set(HEADERS
        "../common/DebugOutput.h"
        "../common/GraphicsAPI.h"
        "../common/GraphicsAPI_OpenGL.h"
        "../common/HelperFunctions.h"
        "../common/OpenXRDebugUtils.h"
        "../common/OpenXRHelper.h")

add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

if(NOT "${XR_RUNTIME_JSON}" STREQUAL "")
    set_target_properties(
            ${PROJECT_NAME} PROPERTIES VS_DEBUGGER_ENVIRONMENT
            "XR_RUNTIME_JSON=${XR_RUNTIME_JSON}"
    )
endif()

target_include_directories(
        ${PROJECT_NAME}
        PRIVATE
        # In this repo
        ../Common/
        # From OpenXR repo
        "${openxr_SOURCE_DIR}/src/common"
        "${openxr_SOURCE_DIR}/external/include"
)
target_link_libraries(${PROJECT_NAME} openxr_loader)

# OpenGL
include(../cmake/gfxwrapper.cmake)
if(TARGET openxr-gfxwrapper)
    target_link_libraries(${PROJECT_NAME} openxr-gfxwrapper)
    target_compile_definitions(
            ${PROJECT_NAME} PUBLIC XR_TUTORIAL_USE_OPENGL
    )
endif()