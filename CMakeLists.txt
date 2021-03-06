cmake_minimum_required(VERSION 3.9)
project(cares_service)

set(CMAKE_CXX_STANDARD 14)

set(INC_PREFIX ${CMAKE_CURRENT_SOURCE_DIR}/include/cares_service)

list(APPEND CXX_HDRS
    ${INC_PREFIX}/cares.hxx
    ${INC_PREFIX}/detail/channel.hxx
    ${INC_PREFIX}/detail/endpoint_sequence.hxx
    ${INC_PREFIX}/detail/error.hxx
    ${INC_PREFIX}/detail/io_object.hxx
    ${INC_PREFIX}/detail/service.hxx
    )

if (WIN32)
    find_package(c-ares CONFIG REQUIRED)
    set(CARES c-ares::cares)
else (WIN32)
    find_library(CARES NAMES libcares.a cares)
    if (CARES-NOTFOUND)
        message(FATAL "libcares not found")
    endif (CARES-NOTFOUND)
endif (WIN32)

add_library(${PROJECT_NAME} INTERFACE)
target_sources(${PROJECT_NAME} INTERFACE ${CXX_HDRS})
target_include_directories(${PROJECT_NAME} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(${PROJECT_NAME} INTERFACE ${CARES})

