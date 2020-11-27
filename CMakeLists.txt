cmake_minimum_required(VERSION 3.10.0)

project(c2man VERSION 0.0.1)

add_subdirectory(libc)

file(GLOB sources *.c)
file(GLOB headers *.h)

find_package(BISON 3.7.0)
find_package(FLEX 2.6.0)

BISON_TARGET(MyParser grammar.y ${CMAKE_CURRENT_BINARY_DIR}/grammar.c DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/grammar.h)
FLEX_TARGET(MyScanner lex.l  ${CMAKE_CURRENT_BINARY_DIR}/lex.c)
ADD_FLEX_BISON_DEPENDENCY(MyScanner MyParser)

add_executable(${PROJECT_NAME}
    ${sources}
    ${headers}
    ${BISON_MyParser_OUTPUTS}
    ${FLEX_MyScanner_OUTPUTS}
)

target_link_libraries(${PROJECT_NAME} ${FLEX_LIBRARIES} helperc)

install(TARGETS ${PROJECT_NAME}
    DESTINATION "/usr/bin"
)
