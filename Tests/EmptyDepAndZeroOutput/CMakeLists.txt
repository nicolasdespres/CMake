#FIXME(Nicolas Despres): Find a good description.
cmake_minimum_required(VERSION 2.8)
project(EmptyDepAndZeroOutput)

include(CTest)

set(OUTPUT_FILE "${CMAKE_BINARY_DIR}/output")
add_custom_command(
  OUTPUT "${OUTPUT_FILE}"
  COMMAND "${CMAKE_COMMAND}" -E touch "${OUTPUT_FILE}"
  # It is import to specify "" and not only no dependencies at all.
  DEPENDS ""
  COMMENT "Generating ${OUTPUT_FILE}"
  )

add_custom_target(custom ALL
  DEPENDS "${OUTPUT_FILE}"
  COMMENT "Should trigger only one custom command"
  )

# It is important to name the file "0" and not anything else.
set(TEST_FILE "${CMAKE_BINARY_DIR}/0")
configure_file(
  ${CMAKE_SOURCE_DIR}/check.cmake.in
  ${CMAKE_BINARY_DIR}/check.cmake
  )
add_custom_command(
  OUTPUT "${TEST_FILE}"
  COMMAND "${CMAKE_COMMAND}" -E touch "${TEST_FILE}"
  COMMENT "BUGGY: Running command that should never run"
  )
