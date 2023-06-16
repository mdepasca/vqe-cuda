cmake_minimum_required(VERSION 3.22 FATAL_ERROR)
project(bencomp VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CUDAQ_DIR $ENV{CUDA_QUANTUM_PATH}/lib/cmake/cudaq)
set(CMAKE_FIND_PACKAGE_PREFER_CONFIG TRUE)
find_package(CUDAQ REQUIRED)

set(CMAKE_CUDAQ_ARCHIVE_CREATE "echo \"creating libvqecudaq.a archive ... \"")
add_library(vqecudaq)
set_target_properties(vqecudaq PROPERTIES LANGUAGE CUDAQ)
target_sources(vqecudaq
    PRIVATE

    # Sources
    ${CMAKE_CURRENT_SOURCE_DIR}/src/vqe_cudaq.cpp
    PUBLIC

    # Headers
    ${CMAKE_CURRENT_SOURCE_DIR}/include/vqecudaq/vqe_cudaq.hpp
)

target_include_directories(vqecudaq PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

target_link_libraries(vqecudaq nvqir-custatevec)

set(CMAKE_CUDAQ_ARCHIVE_FINISH "echo \"... Created libvqecudaq.a archive\"")
