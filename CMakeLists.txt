cmake_minimum_required(VERSION 3.12)

# Disable CYGWIN Legacy Warning
set(CMAKE_LEGACY_CYGWIN_WIN32 0)

# * Set root path as output directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY $<1:${CMAKE_SOURCE_DIR}>)

# Set all version configuration
set(VERSION_MAJOR 0)
set(VERSION_MINOR 0)
set(VERSION_PATCH 1)
set(SO_VERSION ${VERSION_MAJOR}.${VERSION_MINOR})
set(FULL_VERSION ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH})

# Project info
project(ccurly VERSION ${FULL_VERSION})

# Project options
option(CCURLY_BUILD_EXAMPLES "Build ccurly examples." OFF)

# Set C++14 as the standard
set(CMAKE_CXX_STANDARD 14)

# Set generic lib name
set(CCURLY_LIB_NAME ccurly)

include(GNUInstallDirs)

# Get Source Files
set(CCURLY_SOURCES
    src/main.cpp
)

# Buil Compiler
add_executable(${CCURLY_LIB_NAME} ${CCURLY_SOURCES})

# Final Configuration Steps
set_target_properties(${CCURLY_LIB_NAME}
PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${SO_VERSION}
)
configure_file(${CCURLY_LIB_NAME}.pc.in ${CCURLY_LIB_NAME}.pc @ONLY)
target_compile_definitions(${CCURLY_LIB_NAME}
PRIVATE
    __STDC_LIB_EXT1__
)
target_include_directories(${CCURLY_LIB_NAME} PRIVATE include)
install(TARGETS ${CCURLY_LIB_NAME}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
install(FILES ${CMAKE_BINARY_DIR}/${CCURLY_LIB_NAME}.pc
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig
)
