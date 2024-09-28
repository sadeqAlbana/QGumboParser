cmake_minimum_required(VERSION 3.16)
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Core)


# Set the project name and version
project(QGumboParser VERSION 1.0 LANGUAGES CXX C)

# Specify C++ and C standards
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)

# Add the library as a static library
qt_add_library(QGumboParser STATIC
    # QGumboParser source files
    qgumboattribute.cpp
    qgumbodocument.cpp
    qgumbonode.cpp

    # Gumbo parser source files
    gumbo-parser/src/attribute.c
    gumbo-parser/src/char_ref.c
    gumbo-parser/src/error.c
    gumbo-parser/src/parser.c
    gumbo-parser/src/string_buffer.c
    gumbo-parser/src/string_piece.c
    gumbo-parser/src/tag.c
    gumbo-parser/src/tokenizer.c
    gumbo-parser/src/utf8.c
    gumbo-parser/src/util.c
    gumbo-parser/src/vector.c
)

# Include directories for headers
target_include_directories(QGumboParser PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/gumbo-parser/src
)

# Headers to be installed or used (if needed)
set(HEADERS
    qgumboattribute.h
    qgumbodocument.h
    qgumbonode.h
    gumbo-parser/src/attribute.h
    gumbo-parser/src/char_ref.h
    gumbo-parser/src/error.h
    gumbo-parser/src/gumbo.h
    gumbo-parser/src/insertion_mode.h
    gumbo-parser/src/parser.h
    gumbo-parser/src/string_buffer.h
    gumbo-parser/src/string_piece.h
    gumbo-parser/src/token_type.h
    gumbo-parser/src/tokenizer_states.h
    gumbo-parser/src/tokenizer.h
    gumbo-parser/src/utf8.h
    gumbo-parser/src/util.h
    gumbo-parser/src/vector.h
    HtmlTag.h
)

# Compiler-specific flags
if(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    target_compile_options(QGumboParser PRIVATE -Wno-unused-parameter -Wno-sign-compare -Wno-old-style-declaration)
elseif(CMAKE_C_COMPILER_ID STREQUAL "Clang")
    target_compile_options(QGumboParser PRIVATE -Wno-unused-parameter -Wno-sign-compare)
endif()

# Install target (for Unix)
if(UNIX)
    install(TARGETS QGumboParser
        LIBRARY DESTINATION /usr/lib
        ARCHIVE DESTINATION /usr/lib
    )
endif()



target_link_libraries(QGumboParser PRIVATE Qt${QT_VERSION_MAJOR}::Core)

# Windows specific include paths for MSVC
if(MSVC)
    target_include_directories(QGumboParser PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/gumbo-parser/visualc/include)
endif()
