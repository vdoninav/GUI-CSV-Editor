cmake_minimum_required(VERSION 3.5)

project(healthServices VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

set(PROJECT_SOURCES
        main.cpp

        mainwindow.cpp
        mainwindow.h
        mainwindow.ui

        csvmodel.h
        csvmodel.cpp

        rowadddialog.h
        rowadddialog.cpp
        rowadddialog.ui

        roweditdialog.h
        roweditdialog.cpp
        roweditdialog.ui

        helpwindow.h
        helpwindow.cpp
        helpwindow.ui

        filterdialog.h
        filterdialog.cpp
        filterdialog.ui

        csvfiltermodel.h
        csvfiltermodel.cpp

        addrowcommand.h
        addrowcommand.cpp

        removerowcommand.h
        removerowcommand.cpp

        editrowcommand.h
        editrowcommand.cpp

        celleditcommand.h
        celleditcommand.cpp

        csvitemdelegate.h
        csvitemdelegate.cpp

        logowindow.h
        logowindow.cpp
        logowindow.ui
)

add_definitions(-DCSV_FILE_PATH=\"${CMAKE_CURRENT_SOURCE_DIR}/MetroHealth83Original.csv\")

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(healthServices
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET healthServices APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(healthServices SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(healthServices
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(healthServices PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

set_target_properties(healthServices PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

install(TARGETS healthServices
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(healthServices)
endif()
