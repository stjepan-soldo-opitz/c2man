cmake_minimum_required(VERSION 3.10.0)

project(c2man VERSION 0.0.1)

add_subdirectory(libc)

execute_process(COMMAND sh ../Configure -d -e
                COMMAND cp config.h ..)

file(GLOB sources *.c)
file(GLOB headers *.h)

find_package(BISON 3.7.0)
find_package(FLEX 2.6.0)

bison_target(MyParser grammar.y ${CMAKE_CURRENT_BINARY_DIR}/y.tab.c)
flex_target(MyScanner lex.l ${CMAKE_CURRENT_BINARY_DIR}/lex.yy.c COMPILE_FLAGS -n)
add_flex_bison_dependency(MyScanner MyParser)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

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
