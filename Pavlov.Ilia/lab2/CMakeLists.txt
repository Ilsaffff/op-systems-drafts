cmake_minimum_required(VERSION 3.5)

project(lab2)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_compile_options(-Wall -Werror)

find_library(LIBRT rt)
find_package(Threads)
find_package(Qt5 COMPONENTS Widgets REQUIRED)

set(HOST_SOURCES
        src/host/host.h
        src/host/host.cpp
        src/host/gui/mainwindow.cpp
        src/host/gui/mainwindow.h
        src/host/gui/mainwindow.ui
        src/state.h)

set(CLIENT_SOURCES
       src/client/client.h
       src/client/client.cpp
       src/state.h)

file(GLOB CONN_FILES src/connections/conn_*.h)
foreach(CONN_FILE ${CONN_FILES})
    get_filename_component(FILE_NAME ${CONN_FILE} NAME_WLE )
    string(REGEX REPLACE "conn_" "" CONN_TYPE ${FILE_NAME})

    add_executable(host_${CONN_TYPE} ${HOST_SOURCES} src/connections/conn.h
        src/connections/conn_${CONN_TYPE}.cpp src/connections/conn_${CONN_TYPE}.h)
    target_link_libraries(host_${CONN_TYPE} PUBLIC pthread rt Qt5::Widgets)

    add_executable(client_${CONN_TYPE} ${CLIENT_SOURCES} src/connections/conn.h
        src/connections/conn_${CONN_TYPE}.cpp src/connections/conn_${CONN_TYPE}.h)
    target_link_libraries(client_${CONN_TYPE} PUBLIC pthread rt)

endforeach()
