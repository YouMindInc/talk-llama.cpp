# llama library

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

file(GLOB LLAMA_MODEL_SOURCES models/*.cpp)

set(LLAMA_SOURCES
    llama.cpp
    llama-adapter.cpp
    llama-arch.cpp
    llama-batch.cpp
    llama-chat.cpp
    llama-context.cpp
    llama-cparams.cpp
    llama-grammar.cpp
    llama-graph.cpp
    llama-hparams.cpp
    llama-impl.cpp
    llama-io.cpp
    llama-kv-cache.cpp
    llama-kv-cache-iswa.cpp
    llama-memory-recurrent.cpp
    llama-memory-hybrid.cpp
    llama-memory.cpp
    llama-mmap.cpp
    llama-model-loader.cpp
    llama-model-saver.cpp
    llama-model.cpp
    llama-quant.cpp
    llama-sampling.cpp
    llama-vocab.cpp
    unicode.cpp
    unicode-data.cpp
    ${LLAMA_MODEL_SOURCES}
)

add_library(llama ${LLAMA_SOURCES})

set_target_properties(llama PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${SOVERSION}
)

target_include_directories(llama PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/include
)

target_compile_features(llama PUBLIC cxx_std_17)

find_package(Threads REQUIRED)
target_link_libraries(llama PUBLIC ggml Threads::Threads)

if (BUILD_SHARED_LIBS)
    set_target_properties(llama PROPERTIES POSITION_INDEPENDENT_CODE ON)
    target_compile_definitions(llama PRIVATE LLAMA_SHARED LLAMA_BUILD)
endif()

if(WIN32)
    target_compile_definitions(llama PRIVATE -D_WIN32_WINNT=0x0602)
endif()

set_target_properties(llama PROPERTIES
    PUBLIC_HEADER "${CMAKE_SOURCE_DIR}/include/llama.h"
)

install(TARGETS llama LIBRARY PUBLIC_HEADER)

set(TARGET llama)
include(DefaultTargetOptions)
