cmake_minimum_required(VERSION 3.10)
project(imgui_example_directx10 VERSION 1.7.8 LANGUAGES C CXX )

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(DXSDK_DIR ${PROJECT_SOURCE_DIR}/../libs/WindowsKits/10)

set(EXECUTABLE_FILE_NAME imgui_example_directx10)

set (SRC_LIST
    ${PROJECT_SOURCE_DIR}/../../imgui.cpp
    ${PROJECT_SOURCE_DIR}/../../imgui_demo.cpp
    ${PROJECT_SOURCE_DIR}/../../imgui_draw.cpp
    ${PROJECT_SOURCE_DIR}/../../imgui_widgets.cpp
    ${PROJECT_SOURCE_DIR}/../imgui_impl_dx10.cpp
    ${PROJECT_SOURCE_DIR}/../imgui_impl_win32.cpp
    ${PROJECT_SOURCE_DIR}/main.cpp
    )

SET(INCLUDE_PATH
    ${PROJECT_SOURCE_DIR}/../
    ${PROJECT_SOURCE_DIR}/../../
    ${DXSDK_DIR}/Include
    )

# ${DXSDK_DIR}/Lib/10.0.17763.0/um/x64

include_directories(${INCLUDE_PATH})
link_directories(${DXSDK_DIR}/Lib/10.0.17763.0/um/x64)

add_executable(${EXECUTABLE_FILE_NAME} ${SRC_LIST})
target_link_libraries(${EXECUTABLE_FILE_NAME} d3d10.lib d3dcompiler.lib dxgi.lib xinput.lib)

