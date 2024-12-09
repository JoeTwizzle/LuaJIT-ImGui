cmake_minimum_required(VERSION 3.20)
project(cimgui)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Ensure we're building a shared library (DLL)
option(BUILD_SHARED_LIBS "Build shared libraries (DLL)" ON)

# Include directories
include_directories(
    cimgui/imgui
    cimplot/implot
    cimguizmo/ImGuizmo
    # cimguizmo_quat/imGuIZMO.quat/imGuIZMO.quat
    cimnodes/imnodes
    # ./cimguizmo_quat/imGuIZMO.quat/imGuIZMO.quat
    ./cimgui
    ${cimCTE_inc}
)

# Definitions
add_definitions(-DIMGUI_USER_CONFIG=\"../cimconfig.h\")
add_definitions(-DIMGUI_DISABLE_OBSOLETE_FUNCTIONS=1)
add_definitions(-DIMNODES_NAMESPACE=imnodes)

if (WIN32)
    add_definitions(-DIMGUI_IMPL_API=extern\ \"C\"\ __declspec\(dllexport\))
else()
    add_definitions(-DIMGUI_IMPL_API=extern\ \"C\")
endif()

# Collect source files
set(IMGUI_SOURCES
    ./cimgui/cimgui.cpp 
    ./cimgui/imgui/imgui.cpp 
    ./cimgui/imgui/imgui_draw.cpp 
    ./cimgui/imgui/imgui_demo.cpp 
    ./cimgui/imgui/imgui_widgets.cpp 
    ./cimplot/cimplot.cpp 
    ./cimplot/implot/implot.cpp
    ./cimplot/implot/implot_items.cpp  
    ./cimplot/implot/implot_demo.cpp
    ./cimguizmo/cimguizmo.cpp
    ./cimguizmo/ImGuizmo/ImGuizmo.cpp
    # ./cimguizmo_quat/cimguizmo_quat.cpp
    # ./cimguizmo_quat/imGuIZMO.quat/imGuIZMO.quat/imGuIZMOquat.cpp  
    ./cimnodes/cimnodes.cpp
    ./cimnodes/imnodes/imnodes.cpp
    # ./cimnodes_r/cimnodes_r.cpp
    # ./cimnodes_r/ImNodes/ImNodes.cpp
    # ./cimnodes_r/ImNodes/ImNodesEz.cpp
    ${cimCTE_SRCS}
)

if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/cimgui/imgui/imgui_tables.cpp)
    list(APPEND IMGUI_SOURCES ./cimgui/imgui/imgui_tables.cpp)
endif()

# Add shared library target
add_library(cimgui SHARED ${IMGUI_SOURCES})
target_compile_definitions(cimgui PUBLIC IMGUI_DEFINE_MATH_OPERATORS=1)
target_compile_definitions(cimgui PUBLIC IMGUI_DISABLE_OBSOLETE_FUNCTIONS=1)
# Set output name to cimgui.dll (on Windows)
set_target_properties(cimgui PROPERTIES PREFIX "")
if (WIN32)
    set_target_properties(cimgui PROPERTIES SUFFIX ".dll")
endif()
set_target_properties(cimgui PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/output"
)
# Link libraries
# target_link_libraries(cimgui PUBLIC imm32)

# Installation instructions
install(TARGETS cimgui
    RUNTIME DESTINATION .
    LIBRARY DESTINATION .
    ARCHIVE DESTINATION .
)
