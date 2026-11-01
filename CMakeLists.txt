cmake_minimum_required(VERSION 3.16)
project(PacketAnalyzer VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Find Threads for multi-threading support
find_package(Threads REQUIRED)

# Header files location
include_directories(${CMAKE_SOURCE_DIR}/include)

# 1. Basic Packet Parser Demo
add_executable(packet_analyzer
    src/main.cpp
    src/pcap_reader.cpp
    src/packet_parser.cpp
)

# 2. Multi-threaded DPI Engine (Main Engine)
add_executable(dpi_engine
    src/dpi_mt.cpp
    src/pcap_reader.cpp
    src/packet_parser.cpp
    src/sni_extractor.cpp
    src/types.cpp
)
target_link_libraries(dpi_engine PRIVATE Threads::Threads)

# 3. Simple Single-Threaded DPI
add_executable(dpi_simple
    src/main_working.cpp
    src/pcap_reader.cpp
    src/packet_parser.cpp
    src/sni_extractor.cpp
    src/types.cpp
)

if(MSVC)
    target_compile_options(packet_analyzer PRIVATE /W4 /EHsc)
    target_compile_options(dpi_engine PRIVATE /W4 /EHsc)
    target_compile_options(dpi_simple PRIVATE /W4 /EHsc)
else()
    target_compile_options(packet_analyzer PRIVATE -Wall -Wextra)
    target_compile_options(dpi_engine PRIVATE -Wall -Wextra)
    target_compile_options(dpi_simple PRIVATE -Wall -Wextra)
endif()
