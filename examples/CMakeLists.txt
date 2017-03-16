add_executable(http_examples http_examples.cpp)
set_target_properties(http_examples PROPERTIES CXX_STANDARD 11)
target_link_libraries(http_examples asio ${CMAKE_THREAD_LIBS_INIT})

#TODO: add requirement for version 1.0.1g (can it be done in one line?)
find_package(OpenSSL)

if(OPENSSL_FOUND)
    target_link_libraries(http_examples ${OPENSSL_LIBRARIES})
    target_compile_definitions(http_examples PRIVATE HAVE_OPENSSL)

    add_executable(https_examples https_examples.cpp)
    target_compile_definitions(https_examples PRIVATE HAVE_OPENSSL)
    target_link_libraries(https_examples ${OPENSSL_LIBRARIES})
    target_link_libraries(https_examples asio ${CMAKE_THREAD_LIBS_INIT})
endif()

if(MSYS OR MSVC)
    target_link_libraries(http_examples ws2_32 wsock32)
    if(OPENSSL_FOUND)
        target_link_libraries(https_examples ws2_32 wsock32)
    endif()
endif()
