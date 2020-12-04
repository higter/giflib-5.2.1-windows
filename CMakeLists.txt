cmake_minimum_required(VERSION 3.2)

project(giflib VERSION 5.2.0 LANGUAGES C)

set(GIF_SRC dgif_lib.c egif_lib.c gifalloc.c gif_err.c gif_font.c gif_hash.c openbsd-reallocarray.c)
set(GIF_HEADER gif_hash.h  gif_lib.h  gif_lib_private.h)
set(GIF_UTILSRC qprintf.c quantize.c getarg.c)
set(GIF_UTILHEADER getarg.h)

option(BUILD_UTILITIES "Build the utilities" ON)


add_library(giflib SHARED ${GIF_SRC} ${GIF_HEADER})
add_library(giflib_static STATIC ${GIF_SRC} ${GIF_HEADER})
if(MSVC)
add_definitions("-D_CRT_SECURE_NO_WARNINGS -D_CRT_STDC_NO_DEPRECATE")

set_property(TARGET giflib PROPERTY LINK_FLAGS
    /DEF:"${CMAKE_SOURCE_DIR}/giflib.def")

endif()
#add_library(giflib_utils STATIC ${GIF_UTILSRC} ${GIF_UTILHEADER})

if(BUILD_UTILITIES)

add_executable(gif2rgb gif2rgb.c ${GIF_UTILSRC})
target_link_libraries(gif2rgb giflib_static)
add_executable(gifbuild gifbuild.c ${GIF_UTILSRC})
target_link_libraries(gifbuild giflib_static)
add_executable(giffix giffix.c ${GIF_UTILSRC})
target_link_libraries(giffix giflib_static)
add_executable(giftext giftext.c ${GIF_UTILSRC})
target_link_libraries(giftext giflib_static)
add_executable(giftool giftool.c ${GIF_UTILSRC} getopt.c)
target_compile_definitions(giftool PUBLIC STATIC_GETOPT)
target_link_libraries(giftool giflib_static)
add_executable(gifclrmp gifclrmp.c ${GIF_UTILSRC})
target_link_libraries(gifclrmp giflib_static)

install(TARGETS gif2rgb gifbuild giffix giftext giftool gifclrmp
		RUNTIME DESTINATION bin)
 
endif()

install(FILES gif_lib.h DESTINATION include)
install(TARGETS giflib giflib_static
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
		

