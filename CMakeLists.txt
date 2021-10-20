cmake_minimum_required(VERSION 3.18)
project(opengl_engine)

find_package(OpenGL REQUIRED)

add_subdirectory(external/glfw)

file(GLOB_RECURSE GLAD_SOURCES external/glad/*.c)
file(GLOB_RECURSE SOURCES src/*.cpp)

add_executable(${PROJECT_NAME} ${SOURCES} ${GLAD_SOURCES})

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        OpenGL::GL
        glfw
)

target_include_directories(${PROJECT_NAME}
    PRIVATE
        glfw
        external/glad/include
)

# This command will copy your assets folder to your running directory, in order to have access to your shaders, textures, etc
add_custom_command(TARGET ${PROJECT_NAME} PRE_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${PROJECT_SOURCE_DIR}/assets/ $<TARGET_FILE_DIR:${PROJECT_NAME}>/assets)