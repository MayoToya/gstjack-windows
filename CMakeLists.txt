cmake_minimum_required(VERSION 2.80)
project(winjackgst)

set(jackroot "c:/Program Files/Jack2")
set(jackinc "${jackroot}/include")
set(jacklibs "${jackroot}/lib")

file(TO_CMAKE_PATH $ENV{GSTREAMER_1_0_ROOT_MSVC_X86_64} gstroot)

set(gstinc ${gstroot}/include)
set(gstlibs ${gstroot}/lib)
set(srcroot gst-jack)

link_directories(${gstlibs} ${jacklibs})

include_directories(
    ${jackinc}
    ${gstinc}
    ${gstinc}/glib-2.0
    ${gstinc}/gstreamer-1.0
    ${gstlibs}/glib-2.0/include
    gst-libs
    )

set(srcs
    ${srcroot}/gstjack.c
    ${srcroot}/gstjackaudioclient.c
    ${srcroot}/gstjackaudiosink.c
    ${srcroot}/gstjackaudiosrc.c
    ${srcroot}/gstjackutil.c)

add_definitions(
    -DPACKAGE=\"xxlocal\"
    -DGETTEXT_PACKAGE=\"xxlocal\"
    -DVERSION=\"1.0.0\" -DGST_LICENSE=\"LGPL\" -DGST_PACKAGE_NAME=\"xxlocal\"
    -DGST_PACKAGE_ORIGIN=\"xxlocal\")

add_library(libgstjack SHARED ${srcs})

target_link_libraries(libgstjack
    libjack64
    glib-2.0
    gobject-2.0
    gstaudio-1.0
    gstreamer-1.0)

install(TARGETS libgstjack COMPONENT runtime DESTINATION
    ${gstlibs}/gstreamer-1.0)
