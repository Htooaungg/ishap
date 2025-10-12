cmake_minimum_required(VERSION 3.15)
project(ishap VERSION 0.1.0 LANGUAGES CXX)

add_library (ishap INTERFACE)
add_library(ishap::ishap ALIAS ishap)

target_include_directories (ishap INTERFACE 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

target_compile_features(ishap INTERFACE cxx_std_17)

include(CMakePackageConfigHelpers)

install(TARGETS ishap EXPORT ishapTargets)
install(DIRECTORY include/ DESTINATION include)

install(EXPORT ishapTargets
  NAMESPACE ishap::
  DESTINATION lib/cmake/ishap
)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/ishapConfigVersion.cmake"
  COMPATIBILITY SameMajorVersion
)
file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/ishapConfig.cmake"
  "include(\"\${CMAKE_CURRENT_LIST_DIR}/ishapTargets.cmake\")\n"
)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/ishapConfig.cmake"
  "${CMAKE_CURRENT_BINARY_DIR}/ishapConfigVersion.cmake"
  DESTINATION lib/cmake/ishap
)