cmake_minimum_required(VERSION 3.16)

file(READ "${CMAKE_CURRENT_SOURCE_DIR}/include/asio/version.hpp" ASIO_VERSION_HEADER)
string(REGEX MATCH "#[ \t]*define[ \t]+ASIO_VERSION[ \t]+([0-9]+)"
  ASIO_VERSION_MATCH "${ASIO_VERSION_HEADER}")
if(NOT CMAKE_MATCH_1)
  message(FATAL_ERROR "Unable to determine Asio version from include/asio/version.hpp")
endif()

set(ASIO_VERSION_NUMBER "${CMAKE_MATCH_1}")
math(EXPR ASIO_VERSION_MAJOR "${ASIO_VERSION_NUMBER} / 100000")
math(EXPR ASIO_VERSION_MINOR "(${ASIO_VERSION_NUMBER} / 100) % 1000")
math(EXPR ASIO_VERSION_PATCH "${ASIO_VERSION_NUMBER} % 100")

project(asio
  VERSION "${ASIO_VERSION_MAJOR}.${ASIO_VERSION_MINOR}.${ASIO_VERSION_PATCH}"
  LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(AsioHelpers)

asio_define_options()
asio_configure_dependencies()
asio_add_library()

if(ASIO_BUILD_TESTS)
  enable_testing()
  include(AsioTests)
  asio_add_tests()
endif()

if(ASIO_BUILD_EXAMPLES)
  include(AsioExamples)
  asio_add_examples()
endif()
