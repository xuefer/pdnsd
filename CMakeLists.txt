cmake_minimum_required(VERSION 3.0)
project(pdnsd C)

set(CMAKE_C_STANDARD 11)

if(ANDROID)
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall -Wextra")
include_directories(${LIB_ANCILLARY_DIR})
endif()

include_directories(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_BINARY_DIR})
include_directories(${PROJECT_SOURCE_DIR}/src)

if(ANDROID)
configure_file ( ${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h )
else()
execute_process ( COMMAND ${PROJECT_SOURCE_DIR}/configure WORKING_DIRECTORY ${PROJECT_SOURCE_DIR} )

add_compile_definitions(CONFDIR="/etc/pdnsd")
endif()

set(PDNSD_C_SOURCES
        ${PROJECT_SOURCE_DIR}/src/android.c
        ${PROJECT_SOURCE_DIR}/src/cache.c
        ${PROJECT_SOURCE_DIR}/src/cache.h
        ${PROJECT_SOURCE_DIR}/src/conf-keywords.h
        ${PROJECT_SOURCE_DIR}/src/conf-parser.c
        ${PROJECT_SOURCE_DIR}/src/conf-parser.h
        ${PROJECT_SOURCE_DIR}/src/conff.c
        ${PROJECT_SOURCE_DIR}/src/conff.h
        ${PROJECT_SOURCE_DIR}/src/consts.c
        ${PROJECT_SOURCE_DIR}/src/consts.h
        ${PROJECT_SOURCE_DIR}/src/debug.c
        ${PROJECT_SOURCE_DIR}/src/debug.h
        ${PROJECT_SOURCE_DIR}/src/dns.c
        ${PROJECT_SOURCE_DIR}/src/dns.h
        ${PROJECT_SOURCE_DIR}/src/dns_answer.c
        ${PROJECT_SOURCE_DIR}/src/dns_answer.h
        ${PROJECT_SOURCE_DIR}/src/dns_query.c
        ${PROJECT_SOURCE_DIR}/src/dns_query.h
        ${PROJECT_SOURCE_DIR}/src/error.c
        ${PROJECT_SOURCE_DIR}/src/error.h
        ${PROJECT_SOURCE_DIR}/src/freebsd_netinet_ip_icmp.h
        ${PROJECT_SOURCE_DIR}/src/hash.c
        ${PROJECT_SOURCE_DIR}/src/hash.h
        ${PROJECT_SOURCE_DIR}/src/helpers.c
        ${PROJECT_SOURCE_DIR}/src/helpers.h
        ${PROJECT_SOURCE_DIR}/src/icmp.c
        ${PROJECT_SOURCE_DIR}/src/icmp.h
        ${PROJECT_SOURCE_DIR}/src/ipvers.h
        ${PROJECT_SOURCE_DIR}/src/list.c
        ${PROJECT_SOURCE_DIR}/src/list.h
        ${PROJECT_SOURCE_DIR}/src/main.c
        ${PROJECT_SOURCE_DIR}/src/netdev.c
        ${PROJECT_SOURCE_DIR}/src/netdev.h
        ${PROJECT_SOURCE_DIR}/src/pdnsd_assert.h
        ${PROJECT_SOURCE_DIR}/src/rr_types.c
        ${PROJECT_SOURCE_DIR}/src/rr_types.h
        ${PROJECT_SOURCE_DIR}/src/servers.c
        ${PROJECT_SOURCE_DIR}/src/servers.h
        ${PROJECT_SOURCE_DIR}/src/status.c
        ${PROJECT_SOURCE_DIR}/src/status.h
        ${PROJECT_SOURCE_DIR}/src/thread.c
        ${PROJECT_SOURCE_DIR}/src/thread.h)

add_executable(pdnsd ${PDNSD_C_SOURCES})

if(ANDROID)
target_link_libraries(pdnsd ancillary log)
set_target_properties(pdnsd PROPERTIES PREFIX "lib" SUFFIX ".so")
endif()
