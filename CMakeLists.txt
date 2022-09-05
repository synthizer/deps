cmake_minimum_required(VERSION 3.15.0)
project(syz_deps C CXX)
include(FetchContent)

SET(THIRD_PARTY_DEPS
  boost_partial
  concurrentqueue
  cpp11-on-multicore
  dr_libs
  hedley
  miniaudio
  pdqsort
  wdl
)

foreach(DEP_NAME ${THIRD_PARTY_DEPS})
  FetchContent_Declare(${DEP_NAME}
    SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/deps/third_party/${DEP_NAME}"
  )
endforeach()

# For testing purposes, fetch them all and build.
OPTION(SYZ_DEPS_TEST "If set, FetchContent_MakeAvailable the deps and build a test program" OFF)

if(SYZ_DEPS_TEST)
  FetchContent_MakeAvailable(${THIRD_PARTY_DEPS})

  add_executable(test src/test.cpp $<TARGET_OBJECTS:wdl_objlib>)
  target_link_libraries(test
    boost_partial
    concurrentqueue
    cpp11-on-multicore
    dr_libs 
    hedley
    miniaudio
    pdqsort
  )
  target_include_directories(test PRIVATE $<TARGET_PROPERTY:wdl_objlib,INTERFACE_INCLUDE_DIRECTORIES>)
endif()
