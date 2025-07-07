cmake_minimum_required(VERSION 3.15 FATAL_ERROR)
cmake_policy(SET CMP0025 NEW)
set(CMAKE_CXX_STANDARD 17)

if(WIN32)
    # Windows: Setup vcpkg toolchain and prepare zlib
    include(FetchContent)
    FetchContent_Declare(
        vcpkg
        GIT_REPOSITORY https://github.com/microsoft/vcpkg.git
        GIT_TAG        ce613c41372b23b1f51333815feb3edd87ef8a8b # 2025.04.09
    )
    FetchContent_MakeAvailable(vcpkg)

    if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
        set(CMAKE_TOOLCHAIN_FILE "${vcpkg_SOURCE_DIR}/scripts/buildsystems/vcpkg.cmake")
    endif()
    
    if(NOT EXISTS "${vcpkg_SOURCE_DIR}/vcpkg.exe")
        execute_process(
            COMMAND
                CMD /C "${vcpkg_SOURCE_DIR}/bootstrap-vcpkg.bat"
        )
    endif()

    execute_process(
        COMMAND
            CMD /C "${vcpkg_SOURCE_DIR}/vcpkg.exe" install
            zlib:x64-windows-static
    )
endif()

project(vtvalidate LANGUAGES CXX)

find_package(ZLIB REQUIRED)

add_library(vendor-vtzero INTERFACE)
target_include_directories(vendor-vtzero INTERFACE ${PROJECT_SOURCE_DIR}/vendor/vtzero/include)

add_library(vendor-protozero INTERFACE)
target_include_directories(vendor-protozero INTERFACE ${PROJECT_SOURCE_DIR}/vendor/protozero/include)

add_library(vendor-gzip-hpp INTERFACE)
target_include_directories(vendor-gzip-hpp INTERFACE ${PROJECT_SOURCE_DIR}/vendor/gzip-hpp/include)

add_library(
    ${PROJECT_NAME}
    SHARED
        ${CMAKE_JS_SRC}
        src/module.cpp
        src/vtvalidate.cpp
        src/vtvalidate.hpp
)

target_include_directories(
    ${PROJECT_NAME}
    PRIVATE
        ${CMAKE_JS_INC}
        ${PROJECT_SOURCE_DIR}/vendor/node-api-headers/include
        ${PROJECT_SOURCE_DIR}/vendor/node-addon-api
)

target_link_libraries(
    ${PROJECT_NAME}
    PRIVATE
        ${CMAKE_JS_LIB}
        vendor-protozero
        vendor-vtzero
        vendor-gzip-hpp
        ZLIB::ZLIB
)

target_compile_definitions(
    ${PROJECT_NAME}
    PRIVATE
        NAPI_VERSION=8
        NODE_ADDON_API_DISABLE_DEPRECATED
)

set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES
        PREFIX ""
        OUTPUT_NAME "module"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib/binding/$<0:>"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib/binding/$<0:>"
        SUFFIX ".node"
        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
)

if (CMAKE_CXX_COMPILER_ID MATCHES "AppleClang")
    set_target_properties(
        ${PROJECT_NAME}
        PROPERTIES
            LINK_DEPENDS ${PROJECT_SOURCE_DIR}/cmake/symbols.txt
            LINK_FLAGS "-undefined dynamic_lookup -bind_at_load"
    )
elseif(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    set_target_properties(
        ${PROJECT_NAME}
        PROPERTIES
            LINK_DEPENDS ${PROJECT_SOURCE_DIR}/cmake/symbols.def
    )
else()
    set_target_properties(
        ${PROJECT_NAME}
        PROPERTIES
            LINK_DEPENDS ${PROJECT_SOURCE_DIR}/cmake/symbols.ld
            LINK_FLAGS "-z now"
    )
endif()

if(WIN32 AND CMAKE_JS_NODELIB_DEF AND CMAKE_JS_NODELIB_TARGET)
    # Generate node.lib
    execute_process(
        COMMAND ${CMAKE_AR}
            /def:${CMAKE_JS_NODELIB_DEF}
            /out:${CMAKE_JS_NODELIB_TARGET}
            ${CMAKE_STATIC_LINKER_FLAGS}
        COMMAND_ECHO
            STDOUT
    )
endif()