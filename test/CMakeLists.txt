##
# Copyright Soramitsu Co., Ltd. All Rights Reserved.
# SPDX-License-Identifier: Apache-2.0
##

# The target_sources() command converts relative paths to absolute.
if (POLICY CMP0076)
    cmake_policy(SET CMP0076 NEW)
endif ()

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)

function(disable_clang_tidy target)
    set_target_properties(${target} PROPERTIES
            C_CLANG_TIDY ""
            CXX_CLANG_TIDY ""
            )
endfunction()

function(addtest test_name)
    add_executable(${test_name} ${ARGN})
    target_sources(${test_name} PUBLIC
            ${ARGN}
            )
    target_link_libraries(${test_name}
            GTest::gtest
            GTest::gmock_main
            )
    target_include_directories(${test_name} PRIVATE
            ${PROJECT_SOURCE_DIR}/include
            )
    set(xml_output "--gtest_output=xml:${CMAKE_BINARY_DIR}/xunit/xunit-${test_name}.xml")
    add_test(
            NAME ${test_name}
            COMMAND $<TARGET_FILE:${test_name}> ${xml_output}
    )
    set_target_properties(${test_name} PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/tests_bin
            ARCHIVE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/tests_lib
            LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/tests_lib
            )
    disable_clang_tidy(${test_name})
endfunction()

# for tests' XML reports
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/xunit)

addtest(scale_decoder_stream_test
        scale_decoder_stream_test.cpp
        )
target_link_libraries(scale_decoder_stream_test
        scale
        )

addtest(scale_encode_append_test
        scale_encode_append_test.cpp
        )
target_link_libraries(scale_encode_append_test
        scale_encode_append
        )

addtest(scale_compact_test
        scale_compact_test.cpp
        )
target_link_libraries(scale_compact_test
        scale
        buffer
        )

addtest(scale_enum_test
        scale_enum_test.cpp
        )
target_link_libraries(scale_enum_test
        scale
        buffer
        )

addtest(scale_fixed_test
        scale_fixed_test.cpp
        )
target_link_libraries(scale_fixed_test
        scale
        buffer
        )

addtest(scale_boolean_test
        scale_boolean_test.cpp
        )
target_link_libraries(scale_boolean_test
        scale
        buffer
        )

addtest(scale_optional_test
        scale_optional_test.cpp
        )
target_link_libraries(scale_optional_test
        scale
        buffer
        )

addtest(scale_collection_test
        scale_collection_test.cpp
        )
target_link_libraries(scale_collection_test
        scale
        buffer
        )

addtest(scale_variant_test
        scale_variant_test.cpp
        )
target_link_libraries(scale_variant_test
        scale
        buffer
        )

addtest(scale_pair_test
        scale_pair_test.cpp
        )
target_link_libraries(scale_pair_test
        scale
        buffer
        )

addtest(scale_strings_test
        scale_strings_test.cpp
        )
target_link_libraries(scale_strings_test
        scale
        )

addtest(scale_convenience_functions_test
        scale_convenience_functions_test.cpp
        )
target_link_libraries(scale_convenience_functions_test
        scale
        )

addtest(scale_tuple_test
        scale_tuple_test.cpp
        )
target_link_libraries(scale_tuple_test
        scale
        )

addtest(scale_array_test
        scale_array_test.cpp
        )
target_link_libraries(scale_array_test
        scale
        )

addtest(scale_encode_counter_test
        scale_encode_counter_test.cpp
        )
target_link_libraries(scale_encode_counter_test
        scale
        )