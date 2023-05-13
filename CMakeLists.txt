cmake_minimum_required(VERSION 3.17)
project(xghemical)

set(CMAKE_CXX_STANDARD 17)
set(ENV{QT5_PREFIX} /opt/homebrew/opt/qt@5/lib/cmake/Qt5)
if (NOT DEFINED ENV{QT5_PREFIX})
    message(FATAL_ERROR "QT5_PREFIX environment variable is not set")
endif ()



set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} $ENV{QT5_PREFIX})


set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_BINARY_DIR}/install)

find_package(Qt5 COMPONENTS Core Widgets Concurrent REQUIRED)


set(COMPILE_OPTIONS -std=c++23)


add_executable(xghemical
        src/main.cpp
        src/main_window.h
        src/main_window.cpp)





target_link_libraries(xghemical
        Qt5::Core
        Qt5::Widgets
        Qt5::Concurrent
        
        )



set(INSTALL_PATH ${PROJECT_BINARY_DIR}/install)

string(REPLACE "\\" "/" QT5_INST $ENV{QT5_PREFIX}/bin/Qt5Core$<$<CONFIG:Debug>:d>.dll)
string(REPLACE "\\" "/" QT5_WIDGETS_INST $ENV{QT5_PREFIX}/bin/Qt5Widgets$<$<CONFIG:Debug>:d>.dll)
string(REPLACE "\\" "/" QT5_GUI_INST $ENV{QT5_PREFIX}/bin/Qt5Gui$<$<CONFIG:Debug>:d>.dll)
string(REPLACE "\\" "/" QT5_CONCURRENT_INST $ENV{QT5_PREFIX}/bin/Qt5Concurrent$<$<CONFIG:Debug>:d>.dll)

string(REPLACE "\\" "/" ENV_QT5_PREFIX $ENV{QT5_PREFIX})

install(TARGETS xghemical
        ARCHIVE DESTINATION ${INSTALL_PATH} COMPONENT exec
        RUNTIME DESTINATION ${INSTALL_PATH} COMPONENT exec
        )

install(FILES
        ${QT5_INST}
        ${QT5_WIDGETS_INST}
        ${QT5_GUI_INST}
        ${QT5_CONCURRENT_INST}
        
        ${PROJECT_SOURCE_DIR}/README.md
        DESTINATION ${INSTALL_PATH} COMPONENT exec)

install(FILES
        ${ENV_QT5_PREFIX}/plugins/platforms/qminimal.dll
        ${ENV_QT5_PREFIX}/plugins/platforms/qoffscreen.dll
        ${ENV_QT5_PREFIX}/plugins/platforms/qdirect2d.dll
        ${ENV_QT5_PREFIX}/plugins/platforms/qwindows.dll
        DESTINATION ${INSTALL_PATH}/plugins/platforms COMPONENT exec)

add_custom_target(install_exec
        COMMAND ${CMAKE_COMMAND} -D COMPONENT=exec -P ${CMAKE_BINARY_DIR}/cmake_install.cmake
        DEPENDS $<TARGET_FILE:xghemical>
        )