include_directories (.)

cmake_minimum_required (VERSION 2.6)

project (libpyin)

# TODO add linux only  -Ofast -std=c99 -Wall -fPIC

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DFP_TYPE=float")

add_library(libpyin math-funcs.c yin.c pyin.c)

add_subdirectory (external/libgvps)

#------------#
#    TEST 1
#------------#

# Data files
set(TEST_DATA_DIR "${PROJECT_BINARY_DIR}/test/")

file(COPY test/vaiueo2d.wav 
    DESTINATION ${TEST_DATA_DIR})

# Test executable and folder
set(YIN_TESTS
    external/matlabfunctions.c
    test/test.c)


add_executable(pyin-test ${YIN_TESTS})

target_link_libraries (pyin-test
    libpyin libgvps)

set_property(TARGET pyin-test PROPERTY FOLDER "tests")

# Add to test suite
enable_testing()
add_test( NAME pyin-test
    WORKING_DIRECTORY ${TEST_DATA_DIR}
    COMMAND pyin-test )