﻿# CMakeList.txt : CMake project for VorbisPlugin, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.6)

project(VorbisPlugin)

set (OGG_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../../dependency/ogg)
set (VORBIS_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../../dependency/vorbis)
set (OGG_LIBRARIES ${CMAKE_CURRENT_BINARY_DIR}/ogg_build/lib)

add_subdirectory(${OGG_ROOT} ${CMAKE_CURRENT_BINARY_DIR}/ogg_build)
add_subdirectory(${VORBIS_ROOT} ${CMAKE_CURRENT_BINARY_DIR}/vorbis_build)

include_directories (VorbisPlugin 
    $<BUILD_INTERFACE:${OGG_ROOT}/include>
    $<BUILD_INTERFACE:${VORBIS_ROOT}>/include)

# Add source to this project's executable.
add_library (VorbisPlugin SHARED
    #../../src/encoder.c
    #../../src/PluginTest.c
    ../../src/VorbisPluginEncoder.c
    ../../src/VorbisPluginDecoder.c
)
if (WIN32)
  set(VORBIS_LIBRARY_NAME vorbis.lib)
  set(VORBIS_FILE_LIBRARY_NAME vorbisfile.lib)
  set(OGG_LIBRARY_NAME ogg.lib)

else()
  set(VORBIS_LIBRARY_NAME libvorbis.a)
  set(VORBIS_FILE_LIBRARY_NAME libvorbisfile.a)
  set(OGG_LIBRARY_NAME libogg.a)
endif()


target_link_libraries(VorbisPlugin ${CMAKE_CURRENT_BINARY_DIR}/vorbis_build/lib/${VORBIS_LIBRARY_NAME})
target_link_libraries(VorbisPlugin ${CMAKE_CURRENT_BINARY_DIR}/vorbis_build/lib/${VORBIS_FILE_LIBRARY_NAME})
target_link_libraries(VorbisPlugin ${CMAKE_CURRENT_BINARY_DIR}/ogg_build/${OGG_LIBRARY_NAME})


# TODO: Add tests and install targets if needed.