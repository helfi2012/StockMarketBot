cmake_minimum_required(VERSION 3.6)
project(TelegramBot)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -Wall")
set(Boost_USE_MULTITHREADED ON)

find_package(Threads REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(Boost COMPONENTS system REQUIRED)
find_package(CURL)
include_directories(/usr/local/include ${OPENSSL_INCLUDE_DIR} ${Boost_INCLUDE_DIR})
if (CURL_FOUND)
    include_directories(${CURL_INCLUDE_DIRS})
    add_definitions(-DHAVE_CURL)
endif()

include_directories(rapidxml-1.13)

add_subdirectory(fmt-5.3.0)

add_executable(TelegramBot
        main.cpp
        database/MyDatabase.cpp database/MyDatabase.h database/User.h
        stock/StockMarket.cpp stock/StockMarket.h stock/Order.h
        view/KeyboardFactory.cpp view/KeyboardFactory.h view/Controller.cpp view/Controller.h stock/Price.h
        resource-manager/StringResource.cpp resource-manager/StringResource.h stock/Stock.h)

target_link_libraries(TelegramBot /usr/local/lib/libTgBot.a ${CMAKE_THREAD_LIBS_INIT}
        ${OPENSSL_LIBRARIES}
        ${Boost_LIBRARIES}
        ${CURL_LIBRARIES}
        sqlite3
        fmt::fmt)
