# ATLAS - Kick Designer, Tuner & Layering Plugin
# !! UNVERIFIED: CMake is not installed in the cloud sandbox, so this file has never been run.
# !! The Makefile (g++) is what was actually tested. Report any CMake error and it will be fixed.
cmake_minimum_required(VERSION 3.22)
project(Atlas VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# --- Framework-independent DSP core (header-only) -------------------------------------------
add_library(atlas_dsp INTERFACE)
target_include_directories(atlas_dsp INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src)
target_compile_features(atlas_dsp INTERFACE cxx_std_20)

# --- Automated tests ------------------------------------------------------------------------
option(ATLAS_BUILD_TESTS "Build DSP unit tests" ON)
if(ATLAS_BUILD_TESTS)
    enable_testing()
    add_executable(atlas_tests tests/main.cpp tests/test_pitch.cpp tests/test_realtime.cpp)
    target_link_libraries(atlas_tests PRIVATE atlas_dsp)
    if(MSVC)
        target_compile_options(atlas_tests PRIVATE /W4 /permissive-)
    else()
        target_compile_options(atlas_tests PRIVATE -Wall -Wextra -Wpedantic)
    endif()
    add_test(NAME atlas_dsp_tests COMMAND atlas_tests)
endif()

# --- JUCE plugin (added in milestone 3) -----------------------------------------------------
# option(ATLAS_BUILD_PLUGIN "Build the VST3 / Standalone plugin (needs JUCE)" OFF)
