# ---------------------------------------------------
# CMakeLists.txt
#
# Author: Thomas Choquet <semoir.dense-0h@icloud.com>
# Date: 2024/08/25 19:17:56
# ---------------------------------------------------

include(FetchContent)

add_executable(example_glfw_metal_cmake "main.mm")

set_target_properties(example_glfw_metal_cmake PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)
set_target_properties(example_glfw_metal_cmake PROPERTIES FOLDER "examples/")

target_include_directories(example_glfw_metal_cmake PRIVATE "${CMAKE_CURRENT_LIST_DIR}")

FetchContent_Declare(glfw3
    GIT_REPOSITORY    https://github.com/glfw/glfw.git
    GIT_TAG           3.4
    GIT_SHALLOW       1
    GIT_PROGRESS      TRUE
    FIND_PACKAGE_ARGS
)
set(GLFW_BUILD_TESTS    OFF)
set(GLFW_BUILD_DOCS     OFF)
set(GLFW_INSTALL        OFF)
set(GLFW_BUILD_EXAMPLES OFF)
if(NOT GLFW_BUILD_WAYLAND)
    set(GLFW_BUILD_WAYLAND OFF)
endif()
FetchContent_MakeAvailable(glfw3)
target_link_libraries(imgui PUBLIC glfw)

find_library(METAL_FRAMEWORK Metal REQUIRED)
find_library(QUARTZ_CORE_FRAMEWORK QuartzCore REQUIRED)
find_library(FOUNDATION_FRAMEWORK Foundation REQUIRED)
target_link_libraries(imgui PRIVATE ${METAL_FRAMEWORK} ${QUARTZ_CORE_FRAMEWORK} ${FOUNDATION_FRAMEWORK})

target_link_libraries(example_glfw_metal_cmake PRIVATE imgui)

