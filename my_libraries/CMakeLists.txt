cmake_minimum_required(VERSION 3.5)
project(MyLibraries VERSION 1.0 LANGUAGES CXX)

find_package(SomeLibraries 1.0 REQUIRED)

add_library(MyLibraryA src/my_library_a.cpp)

target_include_directories(MyLibraryA
  PUBLIC
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

target_link_libraries(MyLibraryA
  PUBLIC
    SomeLibraries::SomeLibraryA
  PRIVATE
    SomeLibraries::SomeLibraryB
)

install(TARGETS MyLibraryA
  EXPORT MyLibraries-export
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)

install(EXPORT MyLibraries-export
  FILE
    MyLibrariesTargets.cmake
  NAMESPACE
    MyLibraries::
  DESTINATION
    lib/cmake/MyLibraries
)

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/include/my_libraries/my_library_a.hpp
        DESTINATION "include/my_libraries")

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/cmake/MyLibrariesConfig.cmake
        DESTINATION "lib/cmake/MyLibraries")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/MyLibrariesConfigVersion.cmake
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY SameMajorVersion)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/cmake/MyLibrariesConfigVersion.cmake
        DESTINATION "lib/cmake/MyLibraries")
