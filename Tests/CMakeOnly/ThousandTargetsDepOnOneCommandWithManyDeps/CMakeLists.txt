# A project with thousand targets depending on one command with
# 10k dependencies.
cmake_minimum_required(VERSION 2.8)
project(ThousandTargetsDepOnOneCommandWithManyDeps)

# Define LARGE_LIST with 10000 input files.
set(LARGE_LIST "")
foreach(i RANGE 1 10000)
  list(APPEND LARGE_LIST "${CMAKE_CURRENT_BINARY_DIR}/input_${i}")
endforeach(i)

foreach(i ${LARGE_LIST})
  file(WRITE ${i} "input")
endforeach(i)

add_custom_command(
  OUTPUT bottle_neck
  COMMAND ${CMAKE_COMMAND} -E touch bottle_neck
  DEPENDS ${LARGE_LIST}
  COMMENT "Executing command with large number of dependencies..."
  )

set(OUTPUTS "")
foreach(i RANGE 1000)
  set(_output output_${i})
  add_custom_target(${_output}-target ALL
    DEPENDS bottle_neck
    )
  list(APPEND OUTPUTS ${_output})
endforeach(i)
