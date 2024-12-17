# Copyright (c) 2023, EyeLights SAS. All rights reserved.


cmake_minimum_required(VERSION 3.5)
project(imgui_glfw LANGUAGES CXX)


add_library(${PROJECT_NAME} SHARED backends/imgui_impl_glfw.cpp
                                    backends/imgui_impl_opengl3.cpp
                                    misc/cpp/imgui_stdlib.cpp
                                    imgui_draw.cpp 
                                    imgui_tables.cpp
                                    imgui_widgets.cpp
                                    imgui.cpp)

include_directories(./backends)
include_directories(./)
include_directories(./misc/cpp/)



target_link_libraries(${PROJECT_NAME} ) 
link_libraries(${X11_LIBRARIES})


if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(CMAKE_CXX_FLAGS "-std=c++2a -std=gnu++2a -O0 -DDEBUG -g -Wall -Wextra -pedantic-errors")
elseif (CMAKE_BUILD_TYPE STREQUAL "Release")
    set(CMAKE_CXX_FLAGS "-std=c++2a -std=gnu++2a -O3 -DNDEBUG")
elseif (CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    set(CMAKE_CXX_FLAGS "-std=c++2a -std=gnu++2a -O2 -DNDEBUG -g")
    if (PROFILING)
        set(CMAKE_CXX_FLAGS "-std=c++2a -std=gnu++2a -O2 -DNDEBUG -Wall -Wextra -pedantic-errors -g -pg")
        set(CMAKE_MODULE_LINKER_FLAGS "-lc_p")
    endif()

elseif (CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
    set(CMAKE_CXX_FLAGS "-std=c++2a -std=gnu++2a -Os -DNDEBUG")
endif()

set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/../libs)


install(TARGETS ${PROJECT_NAME} DESTINATION ${cmake_files_install_dir})
install(FILES ${CMAKE_BINARY_DIR}/../imgui.h
        DESTINATION include RENAME imgui.h)
install(FILES ${CMAKE_BINARY_DIR}/../backends/imgui_impl_glfw.h
        DESTINATION include RENAME imgui_impl_glfw.h)
install(FILES ${CMAKE_BINARY_DIR}/../backends/imgui_impl_opengl3.h
        DESTINATION include RENAME imgui_impl_opengl3.h)


