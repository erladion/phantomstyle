cmake_minimum_required(VERSION 3.5)

project(phantomstyleplugin VERSION 0.1 LANGUAGES CXX C)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(LINUX)
  set(CMAKE_CXX_FLAGS "-fpermissive")
endif()

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

include_directories(../phantom)

set(PROJECT_SOURCES
  phantomstyleplugin.cpp phantomstyleplugin.h

  ../phantom/phantomcolor.cpp ../phantom/phantomcolor.h
  ../phantom/phantomstyle.cpp ../phantom/phantomstyle.h
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    add_library(phantomstyleplugin
        SHARED
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET streamdeck APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(phantomstyleplugin SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_library(phantomstyleplugin SHARED
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(phantomstyleplugin PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

set_target_properties(phantomstyleplugin PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
)

install(TARGETS phantomstyleplugin
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(FILES phantomstyleplugin.json
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
