# Example usage:
#  mkdir build
#  cd build
#  cmake -g "Visual Studio 14 2015" ..

cmake_minimum_required(VERSION 3.8)
project(imgui_example_glfw_opengl3 VERSION 1.7.8 LANGUAGES C CXX )

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()




#include(CMakeDependentOption)

#CMAKE_DEPENDENT_OPTION(USE_GLAD "Use GLAD" OFF
#                       "USE_GLAD;NOT USE_GL3W" ON)

option(USE_GLAD "Use GLAD" ON)

# GLFW
set(GLFW_DIR ../libs/glfw) # Set this to point to an up-to-date GLFW repo
option(GLFW_BUILD_EXAMPLES "Build the GLFW example programs" OFF)
option(GLFW_BUILD_TESTS "Build the GLFW test programs" OFF)
option(GLFW_BUILD_DOCS "Build the GLFW documentation" OFF)
option(GLFW_INSTALL "Generate installation target" OFF)
option(GLFW_DOCUMENT_INTERNALS "Include internals in documentation" OFF)
add_subdirectory(${GLFW_DIR} binary_dir EXCLUDE_FROM_ALL)
include_directories(${GLFW_DIR}/include)

# Dear ImGui
set(IMGUI_DIR ../../)
include_directories(${IMGUI_DIR} ..)

# Libraries
find_package(OpenGL REQUIRED COMPONENTS OpenGL)

set(LIBRARIES glfw OpenGL::GL)

# Use headers from glfw:
include_directories(${GLFW_DIR}/deps)

set(SRC_LIST
    main.cpp
    ${IMGUI_DIR}/examples/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/examples/imgui_impl_opengl3.cpp
    ${IMGUI_DIR}/imgui.cpp ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_demo.cpp ${IMGUI_DIR}/imgui_widgets.cpp
    )

if(USE_GLAD)
    message("USING GLAD")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DIMGUI_IMPL_OPENGL_LOADER_GLAD2")
    set(GLAD_PATH ${GLFW_DIR}/deps)
    include_directories(${GLAD_PATH})
    list(APPEND SRC_LIST ${GLAD_PATH}/glad_gl.c)
    message("********   GLAD, ONLY OpenGL 3.3 support. *********")
else()
    message("USING GL3W")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DIMGUI_IMPL_OPENGL_LOADER_GL3W")
    set(GL3W_PATH ${PROJECT_SOURCE_DIR}/../libs/gl3w)
    include_directories(${GL3W_PATH})
    list(APPEND SRC_LIST ${GL3W_PATH}/GL/gl3w.c)
    message("********   GL3W, OpenGL 3.0 support. *********")
endif()

# 这句话很重要，必须在CACHE中删除 USE_GLAD，修改才能即时影响
unset(USE_GLAD CACHE)


add_executable(example_glfw_opengl3 ${SRC_LIST})
target_link_libraries(example_glfw_opengl3 ${LIBRARIES} )
