cmake_minimum_required (VERSION 2.8.8)
project (Simple-Web-Server)

set(SWS_STANDALONE OFF)

#if (NOT SWS_STANDALONE AND ("${CMAKE_CURRENT_SOURCE_DIR}" STREQUAL "${CMAKE_SOURCE_DIR}"))
#    set(SWS_STANDALONE ON)
#    add_subdirectory(externals EXCLUDE_FROM_ALL)
#endif()

find_package(Threads REQUIRED)

#set(BOOST_COMPONENTS system thread filesystem date_time)
# Late 2017 TODO: remove the following checks and always use std::regex
if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.9)
        set(BOOST_COMPONENTS ${BOOST_COMPONENTS} regex)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUSE_BOOST_REGEX")
    endif()
endif()

if(APPLE)
    set(OPENSSL_ROOT_DIR "/usr/local/opt/openssl")
endif()

# Adds simple_web_server as an INTERFACE target
add_library(simple_web_server INTERFACE)
target_include_directories(simple_web_server INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

if (SWS_STANDALONE)
    add_subdirectory(examples)
    
    enable_testing()
    add_subdirectory(tests)

    install(FILES server_http.hpp client_http.hpp server_https.hpp client_https.hpp DESTINATION include/simple-web-server)
endif()
