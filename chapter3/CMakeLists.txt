cmake_minimum_required(VERSION 3.22.1)

set(PROJECT_NAME OpenXRTutorialChapter3)
project("${PROJECT_NAME}")

set(GLSL_SHADERS "../shaders/vertex-shader.glsl" "../shaders/fragment-shader.glsl")

# OpenGL GLSL
set(SHADER_DEST "${CMAKE_CURRENT_BINARY_DIR}")
foreach(FILE ${GLSL_SHADERS})
    get_filename_component(FILE_WE ${FILE} NAME_WE)
    add_custom_command(
            OUTPUT "${SHADER_DEST}/${FILE_WE}.glsl"
            COMMAND
            ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/${FILE}"
            "${SHADER_DEST}/${FILE_WE}.glsl"
            COMMENT "GLSL ${FILE}"
            MAIN_DEPENDENCY "${FILE}"
            DEPEND "${FILE}"
            WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
            VERBATIM
    )
    # Make our project depend on these files
    target_sources(${PROJECT_NAME} PRIVATE "${SHADER_DEST}/${FILE_WE}.glsl")
endforeach(FILE)