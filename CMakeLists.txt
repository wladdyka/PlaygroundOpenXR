cmake_minimum_required(VERSION 3.30)
project(PlaygroundOpenXR)

set(CMAKE_CXX_STANDARD 20)

# Optional override runtime
set(XR_RUNTIME_JSON
        "$ENV{XR_RUNTIME_JSON}"
        CACHE PATH
        "Optional location of a specific OpenXR runtime configuration file."
)

add_subdirectory(Chapter2)
