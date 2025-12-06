cmake_minimum_required(VERSION 3.12)

project(BinaryShield VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Fetch Zydis from GitHub
include(FetchContent)
FetchContent_Declare(
    zydis
    GIT_REPOSITORY https://github.com/zyantific/zydis.git
    GIT_TAG master
)
FetchContent_MakeAvailable(zydis)

set(SOURCES
    src/main.cpp
    src/function.cpp
    src/instruction.cpp
    src/pe.cpp
    src/util.cpp
    src/virtual_instruction.cpp
    src/vm_handler.cpp
    src/vm_section.cpp
    src/vm.cpp
)

set(HEADERS
    src/function.h
    src/instruction.h
    src/pe.h
    src/util.h
    src/virtual_instruction.h
    src/vm_handler.h
    src/vm_section.h
    src/vm.h
)

add_executable(binaryshield ${SOURCES} ${HEADERS})

target_link_libraries(binaryshield PRIVATE Zydis)

target_include_directories(binaryshield PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)

if(MSVC)
    target_compile_options(binaryshield PRIVATE /W4)
else()
    target_compile_options(binaryshield PRIVATE -Wall -Wextra -Wpedantic)
endif()

# Install target
install(TARGETS binaryshield RUNTIME DESTINATION bin)
