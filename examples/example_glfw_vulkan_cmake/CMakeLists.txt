# ---------------------------------------------------
# CMakeLists.txt
#
# Author: Thomas Choquet <semoir.dense-0h@icloud.com>
# Date: 2024/08/25 19:17:56
# ---------------------------------------------------

include(FetchContent)

add_executable(example_glfw_vulkan_cmake "main.cpp")

set_target_properties(example_glfw_vulkan_cmake PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)
set_target_properties(example_glfw_vulkan_cmake PROPERTIES FOLDER "examples/")

target_include_directories(example_glfw_vulkan_cmake PRIVATE "${CMAKE_CURRENT_LIST_DIR}")

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

find_package(Vulkan REQUIRED)
target_link_libraries(imgui PUBLIC Vulkan::Vulkan)


target_link_libraries(example_glfw_vulkan_cmake PRIVATE imgui)

