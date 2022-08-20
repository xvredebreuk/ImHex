cmake_minimum_required(VERSION 3.16)
project(imgui)

set(CMAKE_CXX_STANDARD 17)

find_package(PkgConfig REQUIRED)
find_package(Freetype REQUIRED)
find_package(OpenGL REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)

if (UNIX)
    find_package(OpenGL REQUIRED)
endif ()

add_library(imgui OBJECT
    source/imgui.cpp
    source/imgui_demo.cpp
    source/imgui_draw.cpp
    source/imgui_freetype.cpp
    source/imgui_impl_glfw.cpp
    source/imgui_impl_opengl3.cpp
    source/imgui_tables.cpp
    source/imgui_widgets.cpp

    source/cimgui.cpp

    source/TextEditor.cpp

    source/imnodes.cpp

    source/implot.cpp
    source/implot_items.cpp
    source/implot_demo.cpp

    source/fonts/fontawesome_font.cpp
    source/fonts/codicons_font.cpp
    source/fonts/unifont_font.cpp
)

target_precompile_headers(imgui PRIVATE
    include/imconfig.h
    include/imgui_internal.h
    include/imgui.h
    include/imgui_freetype.h

    include/imnodes.h
    include/imnodes_internal.h

    include/implot.h
    include/implot_internal.h

    include/imstb_textedit.h
    include/imstb_truetype.h
    include/stb_image.h

    include/TextEditor.h

    include/fonts/unifont_font.h
    include/fonts/codicons_font.h
    include/fonts/fontawesome_font.h
)

add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GLAD)

target_include_directories(imgui PUBLIC include ${FREETYPE_INCLUDE_DIRS} ${GLFW_INCLUDE_DIRS})

target_link_directories(imgui PUBLIC ${GLFW_LIBRARY_DIRS})

if (WIN32)
    target_link_libraries(imgui PUBLIC Freetype::Freetype glfw3 opengl32.lib)
elseif (UNIX)
    target_link_libraries(imgui PUBLIC Freetype::Freetype glfw OpenGL::GL)
endif()
