# Example usage:
#  mkdir build
#  cd build
#  cmake -g "Visual Studio 14 2015" ..

cmake_minimum_required(VERSION 3.8)
project(imgui_example_glfw_vulkan VERSION 1.7.8 LANGUAGES C CXX )

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DVK_PROTOTYPES")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DVK_PROTOTYPES")

# GLFW
set(GLFW_DIR ../libs/glfw) # Set this to point to an up-to-date GLFW repo
option(GLFW_BUILD_EXAMPLES "Build the GLFW example programs" OFF)
option(GLFW_BUILD_TESTS "Build the GLFW test programs" OFF)
option(GLFW_BUILD_DOCS "Build the GLFW documentation" OFF)
option(GLFW_INSTALL "Generate installation target" OFF)
option(GLFW_DOCUMENT_INTERNALS "Include internals in documentation" OFF)
add_subdirectory(${GLFW_DIR} binary_dir EXCLUDE_FROM_ALL)
include_directories(${GLFW_DIR}/include)

set(VULKAN_DIR ../libs/vulkan) 
include_directories(${VULKAN_DIR})

# Dear ImGui
set(IMGUI_DIR ../../)
include_directories(${IMGUI_DIR} ..)


# Libraries
find_library(Vulcan REQUIRED)

# 如果检测不到 vulkan，手动设置路径
# set(VULKAN_LIBRARY C:/Windows/System32/vulkan-1.dll)
set(LIBRARIES "glfw;Vulkan::Vulkan")

message(${LIBRARIES})
# Use vulkan headers from glfw:
include_directories(${GLFW_DIR}/deps )


file(GLOB sources *.cpp)

add_executable(example_glfw_vulkan ${sources} ${IMGUI_DIR}/examples/imgui_impl_glfw.cpp ${IMGUI_DIR}/examples/imgui_impl_vulkan.cpp ${IMGUI_DIR}/imgui.cpp ${IMGUI_DIR}/imgui_draw.cpp ${IMGUI_DIR}/imgui_demo.cpp ${IMGUI_DIR}/imgui_widgets.cpp)
target_link_libraries(example_glfw_vulkan ${LIBRARIES})
