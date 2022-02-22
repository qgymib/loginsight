cmake_minimum_required(VERSION 3.2)
project(loginsight)

# add custom cmake files
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

# add source files
add_library(${PROJECT_NAME}
    "src/main.c")

# add library dependencies
find_package(luajit REQUIRED)
target_link_libraries(${PROJECT_NAME}
    PRIVATE
        ${LUA_LIBRARIES})

# define target include search paths
target_include_directories(${PROJECT_NAME}
    PUBLIC
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src)

# add warning check
if (CMAKE_C_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(${PROJECT_NAME} PRIVATE /W4 /WX)
else ()
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Werror)
endif ()

# build test
if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    include(CTest)
endif()
if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME AND BUILD_TESTING)
    add_subdirectory(third_party/cutest)
    add_subdirectory(test)
endif()
