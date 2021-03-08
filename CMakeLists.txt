cmake_minimum_required(VERSION 3.9.2)

# set project name
project(cargs
        DESCRIPTION "A simple argument parser library"
        LANGUAGES C)

# configure requirements
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

option(CARGS_ENABLE_TESTS OFF "WHether to enable tests")


# add the main executable
add_library(${PROJECT_NAME} src/cargs.c)
target_include_directories(${PROJECT_NAME} PUBLIC include)

if (CARGS_ENABLE_TESTS)
    # include utilities
    include(CTest)
    include(cmake/CreateTestList.cmake)
    # add tests
    enable_testing()
    create_test(DEFAULT option complex)
    create_test(DEFAULT option mixed)
    create_test(DEFAULT option ending)
    create_test(DEFAULT option long_missing_value)
    create_test(DEFAULT option short_missing_value)
    create_test(DEFAULT option long_space_value)
    create_test(DEFAULT option short_space_value)
    create_test(DEFAULT option long_equal_value)
    create_test(DEFAULT option short_equal_value)
    create_test(DEFAULT option combined)
    create_test(DEFAULT option unknown_long)
    create_test(DEFAULT option unknown_short)
    create_test(DEFAULT option alias)
    create_test(DEFAULT option simple_long)
    create_test(DEFAULT option simple)
    create_test(DEFAULT option print)

    create_test_list(DEFAULT "test/tests.h")

    add_executable(cargstest
            "test/main.c"
            "test/option_test.c")
    target_link_libraries(cargstest PUBLIC cargs)
    target_include_directories(cargstest PUBLIC include)
endif ()