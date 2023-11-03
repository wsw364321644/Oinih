cmake_minimum_required(VERSION 3.23)

set(VERSION "0.0.1")

project(Oinih VERSION ${VERSION})

enable_language(CXX ASM)
option(OINIH_STATIC_CRT "STATIC CRT Build ." ON)

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
  set(CMAKE_INSTALL_PREFIX
    "${CMAKE_BINARY_DIR}/rundir"
    CACHE STRING "Directory to install  after building" FORCE)
endif()

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

if(OINIH_STATIC_CRT)
  if(MSVC)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
  endif()
endif()

set(TARGET_NAME inih)
add_library(${TARGET_NAME} SHARED)
target_sources(
  ${TARGET_NAME}
  PRIVATE ${PROJECT_SOURCE_DIR}/deps/inih/ini.h
  ${PROJECT_SOURCE_DIR}/deps/inih/ini.c

)
target_include_directories(${TARGET_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/deps/inih ${PROJECT_SOURCE_DIR}/deps/inih/cpp)

set(TARGET_NAME ${TARGET_NAME}_a)
add_library(${TARGET_NAME} STATIC)
target_sources(
  ${TARGET_NAME}
  PRIVATE ${PROJECT_SOURCE_DIR}/deps/inih/ini.h
  ${PROJECT_SOURCE_DIR}/deps/inih/ini.c

)
target_include_directories(${TARGET_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/deps/inih ${PROJECT_SOURCE_DIR}/deps/inih/cpp)

set(TARGET_NAME inireader)
add_library(${TARGET_NAME} STATIC)
target_sources(
  ${TARGET_NAME}
  PRIVATE 
  ${PROJECT_SOURCE_DIR}/deps/inih/cpp/INIReader.cpp
  ${PROJECT_SOURCE_DIR}/deps/inih/cpp/INIReader.h
)
target_include_directories(${TARGET_NAME} PUBLIC  ${PROJECT_SOURCE_DIR}/deps/inih/cpp)
target_link_libraries (${TARGET_NAME} PRIVATE inih_a)