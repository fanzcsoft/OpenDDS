# Distributed under the OpenDDS License. See accompanying LICENSE
# file or http://www.opendds.org/license.html for details.
#
# This file is setup the way it is for testing purposes. For a cleaner example
# see DevGuideExamples/DCPS/Messenger/CMakeLists.txt

project(opendds_generated_global_cpp CXX)
cmake_minimum_required(VERSION 3.8.2)

find_package(OpenDDS REQUIRED)

if(OPENDDS_STATIC)
  message(FATAL_ERROR "This test cannot be run on static builds.")
endif()

if(NOT OPENDDS_OWNERSHIP_PROFILE)
  message(FATAL_ERROR "These tests require OpenDDS to be built with Ownership Profile enabled")
endif()

set(target_prefix "opendds_generated_global_")
set(publisher "${target_prefix}publisher")
set(messenger "${target_prefix}messenger")

set(src "${CMAKE_CURRENT_SOURCE_DIR}/../../../DCPS/Messenger")
set(dst ${CMAKE_CURRENT_BINARY_DIR})
set(all_targets ${publisher})

foreach(file publisher.cpp Args.h)
  configure_file(${src}/${file} ${dst}/${file} COPYONLY)
endforeach()

# Publisher
add_executable(${publisher}
  "${dst}/publisher.cpp"
)
set_target_properties(${publisher}
  PROPERTIES OUTPUT_NAME publisher
)

# For tests/Utils when OpenDDS is installed
target_include_directories(${publisher} PRIVATE "${src}/../../../")

target_link_libraries(${publisher} ${messenger} OpenDDS::OpenDDS)
