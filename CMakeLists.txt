# Project name
set (PROJECT_NAME "rkDevelopTool")

# The version number.
set (RKDevelopTool_Mac_PACKAGE_VERSION "1.3")

file (GLOB LOCAL_CPP_FILES *.cpp)
set (SOURCE_FILES ${SOURCE_FILES} ${LOCAL_CPP_FILES})

file (GLOB LOCAL_H_FILES *.h)
set (HEADER_FILES ${HEADER_FILES} ${LOCAL_H_FILES})

cmake_minimum_required (VERSION 3.10)
project (${PROJECT_NAME})

find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBUSB libusb-1.0)
find_package(Iconv REQUIRED)

# Set your libusb path
set (libusb_INCLUDE_DIR ${LIBUSB_INCLUDE_DIRS})
set (libusb_LIBRARY ${LIBUSB_LIBRARY_DIRS})
if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set (libusb_LIBRARY ${LIBUSB_LIBRARY_DIRS}/libusb-1.0.dylib)
else()
    set (libusb_LIBRARY ${LIBUSB_LIBRARY_DIRS}/libusb-1.0.so)
endif()

# Turn off build log
# set (CMAKE_VERBOSE_MAKEFILE ON)

# Configure a header file to pass some of the CMake settings
# to the source code
configure_file (
    "${PROJECT_SOURCE_DIR}/config.h.in"
    "${PROJECT_BINARY_DIR}/config.h"
    )

# add the binary tree to the search path for include files
# so that we will find config.h
include_directories (
    "${PROJECT_BINARY_DIR}"
    "${PROJECT_SOURCE_DIR}"
    "${libusb_INCLUDE_DIR}"
    )

link_directories (${libusb_STATIC})

add_executable (${PROJECT_NAME} ${SOURCE_FILES})

target_link_libraries (
    "${PROJECT_NAME}"
    "${libusb_LIBRARY}"
    "${ICONV_LIBRARY}"
    )

