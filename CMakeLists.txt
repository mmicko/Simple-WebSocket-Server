cmake_minimum_required (VERSION 2.8)
project (Simple-WebSocket-Server)

find_package(Threads REQUIRED)

include_directories(.)

if(APPLE)
    set(OPENSSL_ROOT_DIR "/usr/local/opt/openssl")
endif()

#TODO: add requirement for version 1.0.1g (can it be done in one line?)
find_package(OpenSSL)

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
      set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
endif ()
	

if(OPENSSL_FOUND)
    include_directories(SYSTEM ${OPENSSL_INCLUDE_DIR})

	add_executable(wss_examples wss_examples.cpp)
	target_link_libraries(wss_examples ${OPENSSL_LIBRARIES})
	target_link_libraries(wss_examples ${CMAKE_THREAD_LIBS_INIT})
endif()

add_executable(ws_examples ws_examples.cpp)
target_link_libraries(ws_examples ${CMAKE_THREAD_LIBS_INIT})

if(MSYS)
    target_link_libraries(ws_examples ws2_32 wsock32)
    target_link_libraries(wss_examples ws2_32 wsock32)
endif()
