cmake_minimum_required(VERSION 3.16)
project(CuteLogger VERSION 1.0 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED)

qt_standard_project_setup()

qt_add_library(CuteLogger
    include/AbstractAppender.h
    include/AbstractStringAppender.h
    include/ConsoleAppender.h
    include/CuteLogger_global.h
    include/FileAppender.h
    include/Logger.h
    include/RollingFileAppender.h
    src/AbstractAppender.cpp
    src/AbstractStringAppender.cpp
    src/ConsoleAppender.cpp
    src/FileAppender.cpp
    src/Logger.cpp
    src/RollingFileAppender.cpp
)
target_include_directories(CuteLogger PUBLIC
    include
)

target_compile_definitions(CuteLogger PUBLIC
    CUTELOGGER_LIBRARY
)

target_link_libraries(CuteLogger PUBLIC
    Qt::Core
)

if(WIN32)
    target_sources(CuteLogger PUBLIC
        include/OutputDebugAppender.h
        src/OutputDebugAppender.cpp
    )
endif()

if(ANDROID)
    target_sources(CuteLogger PUBLIC
        include/AndroidAppender.h
        src/AndroidAppender.cpp
    )
endif()

install(TARGETS CuteLogger
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    FRAMEWORK DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
