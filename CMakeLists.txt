cmake_minimum_required (VERSION 3.1.0)
project (SDLIMAGE)

include(CheckLibraryExists)
include(CheckIncludeFiles)
include(CheckIncludeFile)
set(CMAKE_MODULE_PATH "${SDLIMAGE_SOURCE_DIR}/cmake")

find_package(SDL2 REQUIRED)

set(SDL2_LIBRARIES "${SDL2_LIBRARY}")
set(SDL2_INCLUDE_DIRS "${SDL2_INCLUDE_DIR}")

include_directories(${SDL2_INCLUDE_DIRS})

set(SDL2_SUPPORT_JPEG ON CACHE BOOL "Support for JPEG file format")
set(SDL2_SUPPORT_PNG ON CACHE BOOL "Support for PNG file format")
set(SDL2_SUPPORT_WEBP ON CACHE BOOL "Support for WEBP file format")
set(SDL2_SUPPORT_TIFF ON CACHE BOOL "Support for TIFF file format")

if(SDL2_SUPPORT_JPEG)
	add_subdirectory(external/jpeg-9)
	include_directories(external/jpeg-9)
	add_definitions(-DLOAD_JPG)
	add_definitions(-DLOAD_JPG_DYNAMIC="libjpeg-9.dll")
endif()

set(SDLIMAGE_SRC
	IMG.c
	IMG_bmp.c
	IMG_gif.c
	IMG_jpg.c
	IMG_lbm.c
	IMG_pcx.c
	IMG_png.c
	IMG_pnm.c
	IMG_tga.c
	IMG_tif.c
	IMG_webp.c
	IMG_xcf.c
	IMG_xpm.c
	IMG_xv.c
	IMG_xxx.c
	showimage.c
)

set(SDLIMAGE_SOURCES
	${SDLIMAGE_SRC}
)

include_directories(./)

add_library(SDLIMAGE SHARED
	${SDLIMAGE_SOURCES}
)

set_target_properties(SDLIMAGE
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)

target_link_libraries (SDLIMAGE
	${SDL2_LIBRARIES}
)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY build)
set_target_properties(SDLIMAGE PROPERTIES
	PREFIX ""
	OUTPUT_NAME "SDL_image"
)
