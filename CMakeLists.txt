# ---------------------------------------------------
# CMakeLists.txt
#
# Author: Thomas Choquet <thomas.publique@icloud.com>
# Date: 2024/06/17 14:18:45
# ---------------------------------------------------

cmake_minimum_required(VERSION 3.16)

option(IM_BUILD_GLFW     "Build GLFW backend"    OFF)
option(IM_BUILD_METAL    "Build Metal backend"   OFF)
option(IM_BUILD_OPENGL3  "Build Opengl3 backend" OFF)
option(IM_BUILD_VULKAN   "Build Vulkan backend"  OFF)
option(IM_BUILD_EXAMPLES "Build examples"        OFF)

project(imgui)

enable_language(CXX)
if(IM_BUILD_METAL)
    enable_language(OBJCXX)
endif()

add_library(imgui STATIC)

target_compile_features(imgui PUBLIC cxx_std_17)

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
endif()

if(IM_BUILD_VULKAN)
    target_sources(imgui PRIVATE
        "backends/imgui_impl_vulkan.h"
        "backends/imgui_impl_vulkan.cpp"
    )
endif()

if(IM_BUILD_EXAMPLES AND IM_BUILD_OPENGL3 AND IM_BUILD_GLFW)
    add_subdirectory("examples/example_glfw_opengl3_cmake")
endif()

if(IM_INSTALL)
    install(TARGETS imgui
        RUNTIME DESTINATION "bin"
        LIBRARY DESTINATION "lib"
        ARCHIVE DESTINATION "lib"
    )
    install(FILES "imgui.h" "imconfig.h" TYPE INCLUDE)
endif()
