file(GLOB header *.h)
file(GLOB sources *.c)

add_library(
    helperc
    ${header}
    ${sources}
)

target_include_directories(helperc PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")
