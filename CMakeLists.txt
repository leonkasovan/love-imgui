cmake_minimum_required(VERSION 3.0)

if(${CMAKE_CURRENT_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_BINARY_DIR})
	message(FATAL_ERROR "Prevented in-tree build. Use: cmake -Bbuild -S.")
endif()

project(love-imgui)

# Add LuaJIT manually if not found
set(LUAJIT_INCLUDE_DIR "/ucrt64/include/luajit-2.1")
set(LUAJIT_LIBRARY "/ucrt64/lib/libluajit-5.1.dll.a")

if(NOT EXISTS ${LUAJIT_INCLUDE_DIR}/lua.h)
	message(FATAL_ERROR "LuaJIT header not found at ${LUAJIT_INCLUDE_DIR}")
endif()

if(NOT EXISTS ${LUAJIT_LIBRARY})
	message(FATAL_ERROR "LuaJIT library not found at ${LUAJIT_LIBRARY}")
endif()

message(STATUS "Using LuaJIT headers: ${LUAJIT_INCLUDE_DIR}")
message(STATUS "Using LuaJIT library: ${LUAJIT_LIBRARY}")

add_library(love-imgui MODULE
	src/libimgui/imconfig.h
	src/libimgui/imgui.cpp
	src/libimgui/imgui.h
	src/libimgui/imgui_draw.cpp
	src/libimgui/imgui_demo.cpp
	src/libimgui/imgui_widgets.cpp
	src/libimgui/imgui_internal.h
	src/libimgui/stb_rect_pack.h
	src/libimgui/stb_textedit.h
	src/libimgui/stb_truetype.h
	src/imgui_impl.cpp
	src/wrap_imgui_impl.cpp
	src/imgui_impl.h
	src/wrap_imgui_impl.h
	src/imgui_iterator.h
	src/dostring_cache.cpp
)

target_include_directories(love-imgui PRIVATE ${LUAJIT_INCLUDE_DIR})
target_link_libraries(love-imgui PRIVATE ${LUAJIT_LIBRARY})

set_target_properties(love-imgui PROPERTIES PREFIX "" OUTPUT_NAME "imgui")

install(TARGETS love-imgui LIBRARY DESTINATION .)
