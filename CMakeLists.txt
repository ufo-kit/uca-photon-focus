# --- Common ------------------------------------------------------------------

cmake_minimum_required(VERSION 2.6)
project(ucapco C)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(PkgConfig)
include(PkgConfigVars)

add_definitions("-std=c99 -Wall")

pkg_check_modules(UCA libuca>=2.0.0 REQUIRED)
pkg_check_variable(libuca plugindir)


# --- Plugin specific ---------------------------------------------------------

find_package(PF)
find_package(FgLib5)
find_package(ClSerSis)

include_directories(${PF_INCLUDE_DIRS}
    ${clsersis_INCLUDE_DIR}
    ${FgLib5_INCLUDE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${UCA_INCLUDE_DIRS})

add_library(ucapf SHARED uca-pf-camera.c)

link_directories(${UCA_LIBRARY_DIRS})

target_link_libraries(ucapf
    ${UCA_LIBRARIES}
    ${PF_LIBRARIES}
    ${clsersis_LIBRARY}
    ${FgLib5_LIBRARY})

install(TARGETS ucapf
    LIBRARY DESTINATION ${LIBUCA_PLUGINDIR})
