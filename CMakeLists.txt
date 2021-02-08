cmake_minimum_required(VERSION 3.13)

project(
    r_log
    VERSION 1.0
    LANGUAGES CXX
)

add_library(r_log
    src/rlog.cpp
)
target_include_directories(r_log PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_compile_features(r_log PUBLIC cxx_std_14)
set_target_properties(r_log PROPERTIES CXX_EXTENSIONS OFF)
set_target_properties(r_log PROPERTIES PUBLIC_HEADER
    "include/r_log/rlog.h"
)

add_executable(rlog_test
    src/main.cpp
)
target_include_directories(rlog_test PRIVATE include)
target_link_libraries(rlog_test r_log)


# install
set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_BINARY_DIR}/install)
install(TARGETS r_log
        EXPORT r_logTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    r_logConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(EXPORT r_logTargets
        FILE r_logTargets.cmake
        NAMESPACE r_log::
        DESTINATION lib/cmake/r_log
)

configure_file(cmake/r_logConfig.cmake.in r_logConfig.cmake @ONLY)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/r_logConfig.cmake"
              "${CMAKE_CURRENT_BINARY_DIR}/r_logConfigVersion.cmake"
        DESTINATION lib/cmake/MyLib
)



