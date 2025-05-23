cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
project(vtvalidate LANGUAGES CXX)
cmake_policy(SET CMP0025 NEW)
set(CMAKE_CXX_STANDARD 17)

find_package(ZLIB REQUIRED)

add_library(vendor-vtzero INTERFACE)
target_include_directories(vendor-vtzero INTERFACE ${PROJECT_SOURCE_DIR}/vendor/vtzero/include)

add_library(vendor-protozero INTERFACE)
target_include_directories(vendor-protozero INTERFACE ${PROJECT_SOURCE_DIR}/vendor/protozero/include)

add_library(vendor-gzip-hpp INTERFACE)
target_include_directories(vendor-gzip-hpp INTERFACE ${PROJECT_SOURCE_DIR}/vendor/gzip-hpp/include)

add_library(
    vtvalidate
    SHARED
        src/module.cpp
        src/vtvalidate.cpp
        src/vtvalidate.hpp
)

target_include_directories(
    vtvalidate
    PRIVATE
        ${PROJECT_SOURCE_DIR}/vendor/node-api-headers/include
        ${PROJECT_SOURCE_DIR}/vendor/node-addon-api
)

target_link_libraries(
    vtvalidate
    PRIVATE
        vendor-protozero
        vendor-vtzero
        vendor-gzip-hpp
        ZLIB::ZLIB
)

target_compile_definitions(
    vtvalidate
    PRIVATE
        NAPI_VERSION=8
        NODE_ADDON_API_DISABLE_DEPRECATED
)

set_target_properties(
    vtvalidate
    PROPERTIES
        PREFIX ""
        OUTPUT_NAME "module"
        LIBRARY_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/lib/binding/$<0:>"
        SUFFIX ".node"
)

if (CMAKE_CXX_COMPILER_ID MATCHES "AppleClang")
    set_target_properties(
        vtvalidate
        PROPERTIES
            LINK_DEPENDS ${PROJECT_SOURCE_DIR}/cmake/symbols.txt
            LINK_FLAGS "-undefined dynamic_lookup -bind_at_load"
    )
else()
    set_target_properties(
        vtvalidate
        PROPERTIES
            LINK_DEPENDS ${PROJECT_SOURCE_DIR}/cmake/symbols.ld
            LINK_FLAGS "-z now"
    )
endif()
