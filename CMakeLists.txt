# ---------------------------------------------------
# CMakeLists.txt
#
# Author: Thomas Choquet <thomas.publique@icloud.com>
# Date: 2024/06/17 14:18:45
# ---------------------------------------------------

cmake_minimum_required(VERSION 3.16)

option(IM_BUILD_GLFW     "Build GLFW backend" OFF)
option(IM_BUILD_METAL    "Build GLFW backend" OFF)
option(IM_BUILD_OPENGL3  "Build GLFW backend" OFF)

option(IM_BUILD_EXAMPLES "Build examples"     OFF)

project(imgui)

enable_language(CXX)

if(IM_BUILD_METAL)
    enable_language(OBJCXX)
endif()

add_library(imgui ${IMGUI_LIBRARY_TYPE})

set_target_properties(imgui PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)

target_include_directories(imgui PUBLIC "${CMAKE_CURRENT_LIST_DIR}" "${CMAKE_CURRENT_LIST_DIR}/backends")

if(IM_BUILD_METAL)
    target_compile_options(imgui PRIVATE "-fobjc-arc")
endif()

target_sources(imgui PRIVATE
    "imconfig.h"
    "imgui_demo.cpp"
    "imgui_draw.cpp"
    "imgui_internal.h"
    "imgui_tables.cpp"
    "imgui_widgets.cpp"
    "imgui.cpp"
    "imgui.h"
    "imstb_rectpack.h"
    "imstb_textedit.h"
    "imstb_truetype.h"
)

if(IM_BUILD_GLFW)
    target_sources(imgui PRIVATE
    "backends/imgui_impl_glfw.h"
    "backends/imgui_impl_glfw.cpp"
    )
    set(BACKEND_CONTAIN_GLFW ON)
endif()

if(IM_BUILD_METAL)
    target_sources(imgui PRIVATE
        "backends/imgui_impl_metal.h"
        "backends/imgui_impl_metal.mm"
    )
endif()

if(IM_BUILD_OPENGL3)
    target_sources(imgui PRIVATE
        "backends/imgui_impl_opengl3.h"
        "backends/imgui_impl_opengl3_loader.h"
        "backends/imgui_impl_opengl3.cpp"
    )
    set(BACKEND_CONTAIN_OPENGL3 ON)
endif()

if(IM_BUILD_EXAMPLES AND BACKEND_CONTAIN_OPENGL3 AND BACKEND_CONTAIN_GLFW)
    add_subdirectory("examples/example_glfw_opengl3_cmake")
endif()
