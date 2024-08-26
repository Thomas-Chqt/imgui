# ---------------------------------------------------
# CMakeLists.txt
#
# Author: Thomas Choquet <semoir.dense-0h@icloud.com>
# Date: 2024/08/25 19:17:56
# ---------------------------------------------------

include(FetchContent)

add_executable(example_glfw_opengl3_cmake "main.cpp")

set_target_properties(example_glfw_opengl3_cmake PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)
set_target_properties(example_glfw_opengl3_cmake PROPERTIES FOLDER "examples/")

file(GLOB_RECURSE sources "src/*.cpp" "src/*.tpp" "src/*.hpp")
target_sources(example_glfw_opengl3_cmake PRIVATE "${sources}")
target_include_directories(example_glfw_opengl3_cmake PRIVATE "${CMAKE_CURRENT_LIST_DIR}")

set(GLFW_BUILD_TESTS OFF)
set(GLFW_BUILD_DOCS  OFF)
set(GLFW_INSTALL     OFF)
FetchContent_Declare(glfw GIT_REPOSITORY https://github.com/glfw/glfw.git GIT_TAG 3.4 GIT_SHALLOW 1)
FetchContent_MakeAvailable(glfw)
set_target_properties(glfw PROPERTIES FOLDER "examples/dependencies")
set_target_properties(update_mappings PROPERTIES FOLDER "examples/dependencies")
target_compile_options(glfw PRIVATE "-w")
target_link_libraries(imgui PUBLIC glfw)

FetchContent_Declare(glad GIT_REPOSITORY https://github.com/Thomas-Chqt/GLAD.git GIT_TAG master GIT_SHALLOW 1)
FetchContent_MakeAvailable(glad)
set_target_properties(glad PROPERTIES FOLDER "examples/dependencies")
target_link_libraries(imgui PUBLIC glad)


target_link_libraries(example_glfw_opengl3_cmake PRIVATE imgui)

