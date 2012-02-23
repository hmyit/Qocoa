project(Qocoa CXX)
cmake_minimum_required(VERSION 2.8)

find_package(Qt4 COMPONENTS QtMain QtCore QtGui REQUIRED)
include(UseQt4)

set(SOURCES
    main.cpp
    gallery.cpp
)

set(HEADERS
    gallery.h
    qsearchfield.h
    qbutton.h
    qprogressindicatorspinning.h
)

qt4_wrap_cpp(MOC_SOURCES ${HEADERS})

if(APPLE)
    list(APPEND SOURCES
        qsearchfield_mac.mm
        qbutton_mac.mm
        qprogressindicatorspinning_mac.mm
    )
    set(CMAKE_EXE_LINKER_FLAGS
        "${CMAKE_EXE_LINKER_FLAGS} -framework Foundation -framework AppKit"
    )
else()
    list(APPEND SOURCES
        qsearchfield_nonmac.cpp
        qbutton_nonmac.cpp
        qprogressindicatorspinning_nonmac.cpp
    )
    set(RESOURCES
        qsearchfield_nonmac.qrc
        qprogressindicatorspinning_nonmac.qrc
    )
    qt4_add_resources(RESOURCES_SOURCES ${RESOURCES})
endif()

add_executable(Qocoa
    WIN32 MACOSX_BUNDLE 
    ${SOURCES} ${MOC_SOURCES} ${RESOURCES_SOURCES}
)
target_link_libraries(Qocoa ${QT_LIBRARIES})