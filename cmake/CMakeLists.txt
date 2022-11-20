if (POLICY CMP0048)
    cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

cmake_minimum_required(VERSION 3.20)

set(CMAKE_VERBOSE_MAKEFILE ON)

# Require out-of-source builds
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file). Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()

# Set a default build type if none was specified
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to 'Release' as none was specified.")
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

project(lzma LANGUAGES C)

set(LZMA_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../)

set(LZMA_SOURCES ${LZMA_ROOT}/C/7zAlloc.c
                 ${LZMA_ROOT}/C/7zArcIn.c
                 ${LZMA_ROOT}/C/7zBuf2.c
                 ${LZMA_ROOT}/C/7zBuf.c
                 ${LZMA_ROOT}/C/7zCrc.c
                 ${LZMA_ROOT}/C/7zCrcOpt.c
                 ${LZMA_ROOT}/C/7zDec.c
                 ${LZMA_ROOT}/C/7zFile.c
                 ${LZMA_ROOT}/C/7zStream.c
                 ${LZMA_ROOT}/C/Aes.c
                 ${LZMA_ROOT}/C/AesOpt.c
                 ${LZMA_ROOT}/C/Alloc.c
                 ${LZMA_ROOT}/C/Bcj2.c
                 ${LZMA_ROOT}/C/Bra86.c
                 ${LZMA_ROOT}/C/Bra.c
                 ${LZMA_ROOT}/C/BraIA64.c
                 ${LZMA_ROOT}/C/CpuArch.c
                 ${LZMA_ROOT}/C/Delta.c
                 ${LZMA_ROOT}/C/LzFind.c
                 ${LZMA_ROOT}/C/Lzma2Dec.c
                 ${LZMA_ROOT}/C/Lzma2Enc.c
                 ${LZMA_ROOT}/C/Lzma86Dec.c
                 ${LZMA_ROOT}/C/Lzma86Enc.c
                 ${LZMA_ROOT}/C/LzmaDec.c
                 ${LZMA_ROOT}/C/LzmaEnc.c
                 ${LZMA_ROOT}/C/LzmaLib.c
                 ${LZMA_ROOT}/C/Ppmd7.c
                 ${LZMA_ROOT}/C/Ppmd7Dec.c
                 ${LZMA_ROOT}/C/Ppmd7Enc.c
                 ${LZMA_ROOT}/C/Sha256.c
                 ${LZMA_ROOT}/C/Sort.c
                 ${LZMA_ROOT}/C/Xz.c
                 ${LZMA_ROOT}/C/XzCrc64.c
                 ${LZMA_ROOT}/C/XzCrc64Opt.c
                 ${LZMA_ROOT}/C/XzDec.c
                 ${LZMA_ROOT}/C/XzEnc.c
                 ${LZMA_ROOT}/C/XzIn.c
)

add_library(lzma STATIC ${LZMA_SOURCES})
target_include_directories(lzma PUBLIC ${LZMA_ROOT}/C/)
add_compile_definitions(_7ZIP_ST)

set_property(TARGET lzma PROPERTY CXX_STANDARD 17)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(lzma PRIVATE $<BUILD_INTERFACE:-Wno-unknown-attributes>)
endif()
